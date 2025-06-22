# Nhậu môn EmT - Custom YOLOv5 Object Detection

This project demonstrates how to assemble a custom dataset and train a YOLOv5 model to detect objects in the "Nhậu môn EmT" dataset. The workflow includes dataset preparation, training, evaluation, and inference.

## Project Structure

```
├── data.yaml
├── Emer_Test02.ipynb
├── README.dataset.txt
├── README.roboflow.txt
├── train/
│   ├── images/
│   └── labels/
├── valid/
│   ├── images/
│   └── labels/
├── test/
│   ├── images/
│   └── labels/
```

- **data.yaml**: Dataset configuration for YOLOv5.
- **Emer_Test02.ipynb**: Main notebook for training and evaluation.
- **train/**, **valid/**, **test/**: Dataset splits in YOLOv5 format.

## Getting Started

### 1. Clone YOLOv5 and Install Dependencies

```python
!git clone https://github.com/ultralytics/yolov5
%cd yolov5
%pip install -qr requirements.txt
%pip install -q roboflow
```

### 2. Download the Dataset

The dataset is exported from [Roboflow](https://roboflow.com/). You can use the provided notebook to download and extract the dataset:

```python
from roboflow import Roboflow
rf = Roboflow(api_key="YOUR_API_KEY")
project = rf.workspace("minh-ld18876-sinhvien-hoasen-edu-vn").project("nhau-mon-emt")
version = project.version(1)
dataset = version.download("yolov5")
```

### 3. Train the Model

```python
!python train.py --img 600 --batch 16 --epochs 4000 --data /content/datasets/Nhậu-môn-EmT-1/data.yaml --weights yolov5s.pt
```

- Adjust `--img`, `--batch`, `--epochs`, and `--weights` as needed.

### 4. Evaluate Performance

- Training logs and metrics are saved to Tensorboard and log files.
- Focus on `mAP_0.5` for model performance.

```python
%load_ext tensorboard
%tensorboard --logdir runs
```

### 5. Run Inference

Run inference on test images:

```python
!python detect.py --weights runs/train/exp/weights/best.pt --img 600 --conf 0.1 --source /content/yolov5/yolov5/SET06
```

Display inference results:

```python
import glob
from IPython.display import Image, display

for imageName in glob.glob('/content/yolov5/yolov5/runs/detect/exp2/*'):
    display(Image(filename=imageName))
```

## Dataset

- 119 images, annotated in YOLOv5 PyTorch format.
- Preprocessing: auto-orientation, resized to 640x640.
- See [README.roboflow.txt](README.roboflow.txt) for details.

## References

- [YOLOv5 by Ultralytics](https://github.com/ultralytics/yolov5)
- [Roboflow Documentation](https://docs.roboflow.com/)
- [Mean Average Precision (mAP)](https://blog.roboflow.com/mean-average-precision/)

---

**License:** See [LICENSE](LICENSE) for details.
