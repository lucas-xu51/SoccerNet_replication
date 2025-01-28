# Jersey Number Pipeline Setup and Troubleshooting Guide

## Environment Setup

### Prerequisites
- **Conda**: Ensure you have Conda installed.
- **Python 3.x**: Confirm that your Python version is 3.x.

### Clone Repository
Clone the repository from GitHub: https://github.com/mkoshkina/jersey-number-pipeline/tree/main
```bash
git clone https://github.com/mkoshkina/jersey-number-pipeline.git
```

## Steps to Run the Project

### 1. Install Dependencies
Check the dependencies in `setup.py` and install them. Run the following command to set up the project. If training with SoccerNet, specify `SoccerNet` as the parameter:
```bash
C:/Users/grizz/anaconda3/envs/myenv/python.exe c:/Users/grizz/OneDrive/Desktop/COSC419/jersey-number-pipeline/jersey-number-pipeline/setup.py SoccerNet
```
This section of code automatically creates the conda virtual environment and downloads the dependencies

### 2. Prepare the Dataset
Download and extract the dataset. Ensure the structure is as follows:
```
./data/SoccerNet/train/images
```

### 3. Run the Main Script
Run `main.py` and set the dataset and part. For example, use `SoccerNet` as the dataset and `train` as the part.

### 4. Centroid ReID File Issue (The problem I had)
Some possible solutions to errors

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

#### Download the Pretrained Model (necessary)
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

### 7. `self.hparam` Parameter Issue
When the Hparam parameter is updated to a dictionary, the self.hparam parameter may not be passed properly due to the pytorch.lighting update or other reasons
My current problem: I do not know the required version of pytorch.lighting, whether the reduced version will solve it or how to pass the parameters correctly
```python
self.hparams = AttributeDict(hparams)
```
(this code in reid/centroid-reid/modelling/bases.py)
