# Brain Tumor Segmentation using 3D U-Net Architectures

## Overview

This project presents a comparative study of deep learning architectures for automated brain tumor segmentation using multi-modal MRI scans from the BraTS 2020 dataset. The objective is to accurately identify and segment tumor regions while analyzing the trade-offs between segmentation performance and computational efficiency.

The project evaluates:

* Baseline 3D U-Net
* EfficientNet-Based 3D U-Net
* EfficientNet + Attention 3D U-Net

The models were implemented using PyTorch and trained on volumetric MRI data using mixed-precision training and tumor-aware patch sampling.

---

## Dataset

### BraTS 2020 Dataset

The Brain Tumor Segmentation (BraTS) 2020 dataset contains multi-modal MRI scans of glioma patients.

### MRI Modalities

* T1
* T1ce
* T2
* FLAIR

### Segmentation Classes

| Label | Description                         |
| ----- | ----------------------------------- |
| 0     | Background                          |
| 1     | Necrotic / Non-enhancing Tumor Core |
| 2     | Peritumoral Edema                   |
| 4     | Enhancing Tumor                     |

The labels were remapped into four training classes:

| Class ID | Description     |
| -------- | --------------- |
| 0        | Background      |
| 1        | Necrotic Core   |
| 2        | Edema           |
| 3        | Enhancing Tumor |

---

## Project Pipeline

### 1. Data Loading

* NIfTI (.nii) MRI volumes loaded using NiBabel
* Four MRI modalities stacked into a 4-channel input tensor

### 2. Preprocessing

* Per-modality Z-score normalization
* Non-zero voxel normalization
* Label remapping
* Tumor-aware patch sampling

### 3. Training

* Mixed Precision Training (AMP)
* Adam Optimizer
* Dice Loss + Cross Entropy Loss
* Early Stopping
* Model Checkpointing

---

# Architectures

## Baseline 3D U-Net

A standard encoder-decoder architecture consisting of:

* 3D Convolution Blocks
* Batch Normalization
* ReLU Activations
* Skip Connections
* Transposed Convolutions for Upsampling

---

## EfficientNet-Based 3D U-Net

The encoder was replaced with EfficientNet-inspired MBConv blocks featuring:

* Depthwise Separable 3D Convolutions
* Squeeze-and-Excitation (SE) Blocks
* Residual Connections

Benefits:

* Reduced computational complexity
* Faster convergence
* Improved parameter efficiency

---

## EfficientNet + Attention U-Net

The architecture extends the EfficientNet encoder by incorporating attention mechanisms to enhance feature selection during decoding.

The goal was to investigate whether attention improves segmentation quality on volumetric MRI data.

---

# Experimental Results

## Validation Performance

| Model                          | Best Validation Dice | Best Validation IoU |
|--------------------------------|---------------------:|--------------------:|
| 3D U-Net                       | 0.8057               | 0.7349              |
| EfficientNet U-Net             | 0.8157               | 0.7427              |
| EfficientNet + Attention U-Net | **0.8256**           | **0.7575**          |

---

## Key Findings

### Baseline U-Net Achieved Highest Accuracy

The standard 3D U-Net achieved the strongest segmentation performance:

* Dice Score: 0.8057
* IoU: 0.7349

### EfficientNet Improved Training Efficiency

The EfficientNet encoder achieved performance comparable to the baseline while converging significantly faster.

### Attention Increased Complexity

Initial experiments suggest that attention mechanisms increased computational cost without providing a substantial improvement in segmentation performance.

Further investigation is ongoing.

---

# Technologies Used

## Deep Learning

* PyTorch
* Torch AMP (Mixed Precision Training)

## Medical Imaging

* NiBabel
* NumPy

## Data Science

* Pandas
* Scikit-Learn

## Visualization

* Matplotlib

---

# Repository Structure

```text
Brain-Tumor-Segmentation-BraTS2020/
│
├── notebooks/
│   ├── baseline_unet.ipynb
│   ├── efficientnet_unet.ipynb
│   └── attention_unet.ipynb
│
├── models/
│   ├── unet3d_brats2020.pth
│   └── effnet_unet3d_brats2020.pth
│
├── results/
│   ├── training_logs/
│   ├── evaluation_metrics/
│   └── sample_predictions/
│
├── README.md
├── requirements.txt
└── LICENSE
```

---

# Future Work

* Whole Tumor (WT) Evaluation
* Tumor Core (TC) Evaluation
* Enhancing Tumor (ET) Evaluation
* Advanced Data Augmentation
* Learning Rate Scheduling
* CBAM and Attention Gate Experiments
* Full BraTS Benchmark Comparison

---

# Author

**Shreyansh Shakya**

Computer Science Engineering Student
Research Interests: Medical AI, Deep Learning, Computer Vision, Agentic AI Systems

GitHub: https://github.com/ShreyanshShakya
LinkedIn: https://www.linkedin.com/in/shreyansh-shakya-3b019022a
