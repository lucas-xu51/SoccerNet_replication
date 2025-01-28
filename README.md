# SoccerNet_replication

# Jersey Number Pipeline 环境准备与运行指南

## 环境准备

### 依赖安装
- 使用 Conda 创建 Python 3.x 环境。
- 克隆仓库：
  ```bash
  git clone https://github.com/mkoshkina/jersey-number-pipeline.git
  ```

- 进入项目目录：
  ```bash
  cd jersey-number-pipeline
  ```

- 检查并安装 `setup.py` 中的依赖。
  对于 SoccerNet 数据集，运行以下命令：
  ```bash
  & C:/Users/grizz/anaconda3/envs/myenv/python.exe setup.py SoccerNet
  ```

### 数据集准备
- 获取 SoccerNet 数据集并解压。
- 数据集存放形式：
  ```
  ./data/SoccerNet/train/images
  ```

---

## 运行指南

### 运行 `main.py`
- 设置 `dataset` 和 `part` 参数（例如选择 `SoccerNet` 和 `train`）。
- 如果运行时报错，尤其与 `centroid_reid.py` 文件相关，请参照以下处理方法。

### 常见问题及解决方案

#### 1. `centroid_reid.py` 文件问题
- 确保正确定义 `configuration.py` 文件中的 `jersey_id_result`：
  ```python
  'train': {
      # ...其他参数...
      'jersey_id_result': 'jersey_id_results_train.json'
  }
  ```

- 修改 `Legibility_classifier.py` 中的引用：
  ```python
  # 原代码：
  from sam.sam import ...
  
  # 修改为：
  from san2.sam import ...
  ```

#### 2. 模型下载
- 下载预训练模型：
  [下载链接](https://drive.google.com/drive/folders/1NWD2Q0JGasGm9HTcOy4ZqsIqK4-IfknK)
- 将模型存放至以下目录：
  ```
  reid/centroid-reid/models
  ```

#### 3. 更新 checkpoint
- 执行以下命令更新 checkpoint：
  ```bash
  python -m pytorch_lightning.utilities.upgrade_checkpoint \
  "C:/Users/grizz/OneDrive/Desktop/COSC419/jersey-number-pipeline/jersey-number-pipeline/reid/centroids-reid/models/market1501_resnet50_256_128_epoch_120.ckpt" \
  --map-to-cpu
  ```

#### 4. `pytorch_lightning` 更新问题
- **问题 1**：`seed_everything` 导入路径问题。
  
  修改文件 `reid/centroids-reid/utils/misc.py`：
  ```python
  # 原代码：
  from pytorch_lightning.utilities.seed import seed_everything
  
  # 修改为：
  from pytorch_lightning import seed_everything
  ```

- **问题 2**：`Callback` 导入路径问题。
  
  修改文件 `reid/centroids-reid/callbacks/chechpointer_callback.py`：
  ```python
  # 原代码：
  from pytorch_lightning.callbacks.base import Callback
  
  # 修改为：
  from pytorch_lightning.callbacks import Callback
  ```

---

## 当前问题及解决方法

### 问题描述
- 使用 `self.hparam` 参数时，可能由于 `pytorch_lightning` 的更新，导致无法正确修改或传递参数。

### 解决方案
1. 确定兼容的 `pytorch_lightning` 版本：
   - 尝试降级到较低版本，如 1.5.x 或 1.6.x，查看是否解决问题。
   - 使用以下命令安装特定版本：
     ```bash
     pip install pytorch-lightning==1.6.0
     ```

2. 如果降级无效，建议更新代码逻辑，避免直接修改 `self.hparam`。
   - 使用 `self.save_hyperparameters()` 方法传递超参数：
     ```python
     class MyLightningModule(pl.LightningModule):
         def __init__(self, **kwargs):
             super().__init__()
             self.save_hyperparameters(kwargs)
     ```

---

## 注意事项
- 确保已正确安装所有依赖，并检查 Python 和 PyTorch 的版本兼容性。
- 任何其他问题，请参考官方文档或提交 issue 至项目仓库。

