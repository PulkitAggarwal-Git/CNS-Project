# Enhanced NeuroScope: Dual-Branch FeatureNet for DNN Operator & Attribute Recovery

An enhanced implementation of the **FeatureNet-based operator recovery framework** proposed in the paper:

**"NeuroScope: Reverse Engineering Deep Neural Network on Edge Devices using Dynamic Analysis"** 

This project focuses on improving the original FeatureNet architecture used for recovering Deep Neural Network (DNN) operator semantics from input/output tensor behaviors.

---

## Overview

Modern edge AI deployments often execute DNN inference through compiled binaries or interpreter-based runtimes, making reverse engineering difficult using traditional static analysis approaches.

The original NeuroScope framework proposes a **data-driven dynamic analysis pipeline** that:

1. Collects operator input/output tensor pairs from DNN binaries
2. Extracts statistical features from these tensors
3. Uses a FeatureNet-based neural architecture to:
   - Predict operator types
   - Recover operator attributes

The original architecture combines:

- Seq2Seq LSTM encoding
- Statistical tensor features
- Fully connected prediction layers

for recovering DNN semantics from runtime behavior.

---

# Our Contribution

In this work, we extend the original FeatureNet architecture by introducing a **Dual-Branch FeatureNet** that separately captures:

- **Input tensor statistical characteristics**
- **Output tensor statistical characteristics**

before fusing them for final operator and attribute prediction.

## Motivation

The original architecture processes statistical information jointly after sequence encoding. However, input and output tensors often exhibit different semantic behaviors:

- Input tensors preserve structural and distributional information
- Output tensors encode transformed activation patterns
- Certain operators introduce distinctive output-side sparsity or compression effects

By learning these representations independently, the enhanced model can:

- Better capture operator-specific transformations
- Improve feature disentanglement
- Increase robustness across operator categories
- Improve attribute recovery accuracy

---

# Enhanced Architecture

## Original FeatureNet

The original FeatureNet architecture proposed in NeuroScope uses:

- Sequential LSTM encoding of input/output tensors
- Extracted statistical features:
  - Mean
  - Min/Max
  - Zero counts
  - Length ratios
  - Range ratios
- Fully connected layers for classification/regression

---

## Proposed Dual-Branch FeatureNet

Our enhanced architecture introduces two independent statistical processing branches:

### Branch 1 — Input Feature Branch
Processes statistics extracted only from input tensors:

- Input mean
- Input variance
- Input sparsity
- Input range
- Tensor length information

### Branch 2 — Output Feature Branch
Processes statistics extracted only from output tensors:

- Output mean
- Output variance
- Output sparsity
- Activation compression patterns
- Output range information

### Fusion Layer
The representations learned from both branches are fused together with sequence encodings to produce:

- Operator type predictions
- Operator attribute predictions

This allows the model to learn richer semantic relationships between operator inputs and outputs.

---

# Offline Phase

The complete recovery pipeline is executed through the offline module, which includes:

- Dataset synthesis
- Model training
- Model recovery

## Dataset Synthesis

Go into the `offline` folder and run:

```bash
python dataset_synthesizer.py
```

You can configure:

- `SIZE` → number of synthesized datapoints
- `PATH` → output path for the synthesized dataset

Both parameters can be modified using the global variables defined inside `dataset_synthesizer.py`.

---

## Model Training

Go into the `offline` folder and run:

```bash
python train.py
```

The trained models will be saved in:

```text
./offline/saved_models
```

---

## Model Recovery

Go into the `offline` folder and run:

```bash
python recover.py
```

The recovery pipeline:

- Infers operator and attribute information from dumped I/O data
- Uses the trained models for semantic prediction
- Reconstructs the DNN architecture
- Generates the recovered network in ONNX format

The recovered model will be generated as:

```text
recovered_model.onnx
```

---

# Experimental Results

We compare:

1. The original FeatureNet implementation described in the NeuroScope paper
2. Our enhanced Dual-Branch FeatureNet

## Result Files

- `paper results.log`
  - Results reproduced from the original FeatureNet architecture

- `our model results.txt`
  - Results obtained using the enhanced dual-branch architecture

---

# Key Improvements

Our enhanced model demonstrates improvements in:

- Operator classification accuracy
- Attribute prediction stability
- Feature representation quality
- Generalization across operator types

The dual-branch design enables the model to better learn semantic transformations between input and output tensors.

---

# Supported Operators

The framework supports recovery of common DNN operators including:

- Convolution (Conv)
- Conv + ReLU
- Fully Connected (FC)
- MaxPool
- AvgPool
- Add
- ReLU
- Softmax
- Concat
- Transpose
- RNN
- LSTM

as described in the original NeuroScope framework. fileciteturn0file0

---

# Training Pipeline

## 1. Dataset Synthesis

Synthetic operator input/output tensor pairs are generated using randomized:

- Tensor dimensions
- Operator parameters
- Operator attributes

following the methodology proposed in NeuroScope. fileciteturn0file0

## 2. Feature Extraction

Statistical and sequential features are extracted independently from:

- Input tensors
- Output tensors

## 3. Model Training

The enhanced Dual-Branch FeatureNet is trained for:

- Operator classification
- Attribute regression

using PyTorch.

---

# Requirements

Example dependencies:

```bash
Python >= 3.9
PyTorch
NumPy
scikit-learn
matplotlib
pandas
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

# Running the Project

## Model Training

Go into the folder `offline`, and invoke:

```bash
python train.py
```

The trained models will be placed in:

```text
./offline/saved_models
```

---

## Model Recovery

Go into the folder `offline`, and invoke:

```bash
python recover.py
```

The recovery pipeline will:

- Infer network information from dumped I/O data
- Use the trained models for operator and attribute prediction
- Generate a DNN model description file in ONNX format

The recovered model will be generated as:

```text
recovered_model.onnx
```

---

# Research Context

This work builds upon the ideas introduced in NeuroScope:

- Dynamic-analysis-based DNN reverse engineering
- I/O semantic inference
- FeatureNet-style operator recovery
- Statistical tensor feature learning

while improving the FeatureNet architecture through a dual-branch statistical learning design.

---

# Citation

If you use this project, please cite the original paper:

```bibtex
@inproceedings{wu2025neuroscope,
  title={NeuroScope: Reverse Engineering Deep Neural Network on Edge Devices using Dynamic Analysis},
  author={Wu, Ruoyu and Zou, Muqi and Khan, Arslan and Kim, Taegyu and Xu, Dongyan and Tian, Dave and Bianchi, Antonio},
  booktitle={USENIX Security Symposium},
  year={2025}
}
```

---

# Acknowledgement

This repository is inspired by the NeuroScope framework and extends its FeatureNet-based learning methodology with a dual-branch feature extraction architecture for improved DNN operator and attribute recovery.

