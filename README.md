# Jersey Number Pipeline Setup and Troubleshooting Guide

## Environment Setup

### Prerequisites
- **Conda**: Ensure you have Conda installed.
- **Python 3.x**: Confirm that your Python version is 3.x.

### Clone Repository
Clone the repository from GitHub:
```bash
git clone https://github.com/mkoshkina/jersey-number-pipeline.git
cd jersey-number-pipeline
```

## Steps to Run the Project

### 1. Install Dependencies
Check the dependencies in `setup.py` and install them. Run the following command to set up the project. If training with SoccerNet, specify `SoccerNet` as the parameter:
```bash
C:/Users/grizz/anaconda3/envs/myenv/python.exe c:/Users/grizz/OneDrive/Desktop/COSC419/jersey-number-pipeline/jersey-number-pipeline/setup.py SoccerNet
```

### 2. Prepare the Dataset
Download and extract the dataset. Ensure the structure is as follows:
```
./data/SoccerNet/train/images
```

### 3. Run the Main Script
Run `main.py` and set the dataset and part. For example, use `SoccerNet` as the dataset and `train` as the part.

### 4. Centroid ReID File Issue
If you encounter errors while running `centroid_reid.py`, follow these steps:

#### Define `jersey_id_result` in `configuration.py`:
```python
"train": {
    # Other configurations...
    "jersey_id_result": "jersey_id_results_train.json"
}
```

#### Update Import in `legibility_classifier.py`:
Replace:
```python
from sam.sam import ...
```
With:
```python
from san2.sam import ...
```

#### Download the Pretrained Model
Download the trained model from the following link:
[Trained Model](https://drive.google.com/drive/folders/1NWD2Q0JGasGm9HTcOy4ZqsIqK4-IfknK)

Place the downloaded model in the following directory:
```
reid/centroid-reid/models
```

### 5. Update the Checkpoint
Run the following command to update the checkpoint:
```bash
python -m pytorch_lightning.utilities.upgrade_checkpoint "C:\Users\grizz\OneDrive\Desktop\COSC419\jersey-number-pipeline\jersey-number-pipeline\reid\centroids-reid\models\market1501_resnet50_256_128_epoch_120.ckpt" --map-to-cpu
```

### 6. Fix Issues Due to PyTorch Lightning Updates
#### Update Seed Import
In `reid/centroids-reid/utils/misc.py`, replace:
```python
from pytorch_lightning.utilities.seed import seed_everything
```
With:
```python
from pytorch_lightning import seed_everything
```

#### Update Callback Import
In `reid/centroids-reid/callbacks/chechpointer_callback.py`, replace:
```python
from pytorch_lightning.callbacks.base import Callback
```
With:
```python
from pytorch_lightning.callbacks import Callback
```

### 7. Handle `self.hparam` Parameter Issue
The `self.hparam` parameter might not work due to updates in PyTorch Lightning. Convert it to a dictionary instead. For example:
```python
self.hparams = {"param_name": value}
```

### PyTorch Lightning Version
If you encounter compatibility issues, try downgrading PyTorch Lightning to a version compatible with the repository codebase. For example:
```bash
pip install pytorch-lightning==1.5.10
```

## Summary
Follow the steps above to set up the environment, resolve compatibility issues, and successfully run the project. Ensure you adjust for updates in external libraries and pretrained models as necessary.

