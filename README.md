# Dendrite Segmentation in SEM Images

## Project Overview

This project investigates automatic segmentation of dendritic structures in scanning electron microscopy (SEM) images.

Two complementary approaches are implemented and evaluated:

1. Classical computer vision pipeline
2. Deep learning segmentation model based on YOLO

The goal is to compare traditional morphological image processing with modern deep learning methods for detecting dendrites in noisy SEM images.

---

# Dataset

The dataset contains **14 SEM images** of lithium dendritic structures.

Images were manually annotated using **Roboflow** with polygon segmentation labels and exported in **YOLO segmentation format**.

Dataset structure:


dataset/
└── SEM_Dendrite_Segmentation_v2.v3i.yolov11/
├── train/
│ ├── images/
│ └── labels/
│
├── valid/
│ ├── images/
│ └── labels/
│
├── test/
│ ├── images/
│ └── labels/
│
└── data.yaml


Images are divided into two difficulty groups:

**EASY**

- relatively clean background
- clear dendritic structures

**HARD**

- strong SEM noise
- faint dendritic branches
- complex dendritic morphology

---

# Project Structure


project_root/

artifacts/
├── image1/
├── image2/
├── image3/
├── image4/
├── image5/
├── image6/
├── best.pt
└── drive_link.txt

notebooks/
├── data/
│ ├── demo_images/
│ ├── Easy/
│ └── Hard/
│
├── dataset/
│ └── SEM_Dendrite_Segmentation_v2.v3i.yolov11/
│
├── datasets/
│ └── for_annotation/
│
├── outputs/
│ ├── classic_masks/
│ ├── overlays/
│ ├── skeletons/
│ ├── classic_metrics.csv
│ ├── classic_vs_gt_metrics.csv
│ └── comparison_metrics.csv
│
├── runs/
│ └── segment/runs_local/
│
├── 01_explore.ipynb
├── 02_classic_pipeline.ipynb
├── 03_ml_model.ipynb
├── 04_comparison.ipynb
└── 05_inference_demo.ipynb


---

# Classical Segmentation Pipeline

The classical pipeline extracts dendritic structures using deterministic image processing operations.

Main stages:

1. Histogram normalization and preprocessing
2. **Bottom crop (93 pixels)** to remove SEM overlay
3. Bilateral filtering for edge-preserving denoising
4. Background flattening using morphological operations
5. Sauvola adaptive thresholding
6. Morphological cleanup
7. **Branch separation using Distance Transform and Watershed**
8. **Skeletonization of dendrite structures**

The pipeline produces:

- binary segmentation masks
- overlay visualizations
- skeleton representations for topology analysis

---

# Deep Learning Model

A deep learning segmentation model based on **YOLOv11** was trained using the annotated dataset.

Two architectures were evaluated.

### Initial experiment

Model:


YOLOv11n-seg


Parameters:

- epochs: 50
- image size: 640
- batch size: 4

This experiment served as a baseline for validating the training pipeline.

### Final model

The final model uses:


YOLOv11s-seg


Parameters:

- epochs: 150
- image size: 640
- batch size: 4

The model was trained using transfer learning with pretrained YOLO segmentation weights.

The best performing model is stored as:


artifacts/best.pt


Training logs are stored in:


runs/segment/runs_local/


---

# Evaluation

Segmentation quality was evaluated by comparing predicted masks with the manually annotated ground truth masks.

Metrics used:

- IoU (Intersection over Union)
- Dice coefficient
- Precision
- Recall

Results are stored in:


outputs/classic_metrics.csv
outputs/classic_vs_gt_metrics.csv
outputs/comparison_metrics.csv


---

# Demo Inference

The trained YOLO model is applied to additional SEM images to demonstrate inference on external data.

Images are located in:


notebooks/data/demo_images


The notebook:


05_inference_demo.ipynb


visualizes for each image:

- original SEM image
- predicted binary mask
- mask overlay on the image
- YOLO rendered segmentation result

Before inference, a fixed bottom crop is applied to remove the SEM overlay.

---

# Artifacts

The `artifacts` directory contains:


best.pt


trained YOLO segmentation model weights.

Example outputs for several images are provided in:


image1 - image6


---

# Requirements

Python **3.9+**

Install dependencies:


pip install -r requirements.txt


Main libraries:

- numpy
- opencv
- scikit-image
- matplotlib
- ultralytics (YOLO)

---

# Running the Project

### 1. Dataset exploration


01_explore.ipynb


Visual inspection of SEM images and annotations.

### 2. Classical segmentation pipeline


02_classic_pipeline.ipynb


Runs the full classical dendrite segmentation pipeline.

### 3. YOLO model training


03_ml_model.ipynb


Trains the segmentation model.

### 4. Method comparison


04_comparison.ipynb


Compares classical segmentation with YOLO predictions.

### 5. Inference demo


05_inference_demo.ipynb


Runs the trained model on external SEM images.

---

# Conclusion

This project compares classical image processing and deep learning approaches for dendrite segmentation in SEM images.

The classical pipeline provides an interpretable baseline and achieves relatively strong segmentation performance using morphological operations.

The YOLO deep learning model achieves slightly higher IoU, Dice, and Recall scores and demonstrates improved robustness to noisy SEM images and complex dendritic structures.

Although the dataset used in this project is relatively small (14 images), the results highlight the potential of deep learning methods for microscopy image segmentation tasks.