# Artificial-Intelligence-in-Medicine-Competition

**Short Description (≈350 characters):**  
This project presents a complete EEG-based seizure detection framework integrating advanced signal preprocessing, spectral and temporal feature extraction, and a hybrid CNN–LSTM model with an attention mechanism to provide accurate seizure classification and interpretable onset estimation.

---

# 1. Introduction

This repository contains the complete implementation developed for the **Artificial Intelligence in Medicine Competition**, focusing on automated seizure detection using multichannel EEG signals. The project aims to design a robust, interpretable, and computationally efficient pipeline capable of identifying seizure events and estimating seizure onset and offset with high precision.

The workflow spans three phases:
1. **Data extraction and preprocessing**
2. **Deep learning model training**
3. **Visualization and explainability**

The project demonstrates the integration of classical biomedical signal processing methods with modern deep learning architectures tailored for temporal physiological data.

---

# 2. Key Features

### **Signal Processing & Data Standardization**
- Removal of 50 Hz electrical interference using **IIR notch filtering**
- **FIR bandpass filtering (0.5–70 Hz)** to preserve physiological EEG frequencies
- Resampling EEG signals to **250 Hz** for uniformity
- Extraction of fixed-length segments (first 300 seconds)
- Storage of processed arrays and annotation labels in `.npy` format

### **Feature Engineering**
- **Power Spectral Density (PSD)** using Welch’s method
- **Bandpower extraction** across clinically relevant EEG bands:
  - Delta (0.5–4 Hz)
  - Theta (4–8 Hz)
  - Alpha (8–12 Hz)
  - Beta (12–30 Hz)
  - Gamma (30–70 Hz)
- Time-domain metrics:
  - Mean, Standard Deviation, RMS  
  - Skewness, Kurtosis

### **Deep Learning Model**
A hybrid seizure detection architecture:
- **CNN layers**: capture localized temporal patterns
- **LSTM layers**: learn long-term dependencies in EEG
- **Attention mechanism**: highlight critical time steps contributing to seizure prediction
- Multi-task loss:
  - Binary Cross-Entropy for seizure classification  
  - Weighted MSE for onset/offset regression  
  - Combined total loss = BCE + 0.5 × MSE

### **Explainable AI (XAI)**
- Extraction of attention weight vectors
- Upsampling attention to original EEG length
- Visualization of EEG waveforms with heatmap overlays
- Identification of EEG regions most relevant to model decision-making

---

# 3. Installation

Ensure **Python 3.x** is installed.

Install required dependencies:

```bash
pip install numpy==1.24.0 \
             pandas==1.5.3 \
             scipy==1.9.3 \
             tqdm==4.64.0 \
             torch>=1.10.0 \
             matplotlib==3.6.3 \
             seaborn==0.11.2 \
             scikit-learn==1.0.2 \
             mne==1.3.1

## 4. File Overview

### 4.1. visualization.ipynb
This notebook provides comprehensive visualization tools to interpret EEG signals and model outputs:

- Visualization of raw and filtered EEG signals  
- PSD plots to analyze power distribution across frequencies  
- Overlay of attention heatmaps on EEG signals for XAI  
- Visual comparison of seizure vs. non-seizure patterns  

It is the final diagnostic tool in the workflow, allowing users to inspect how the model interprets EEG patterns.

---

### 4.2. seizure_nonseizure_data_extraction.ipynb
This notebook performs all preprocessing required to prepare the dataset for modeling:

- Loading raw EEG signals  
- Removing powerline interference using a 50 Hz notch filter  
- Applying a 0.5–70 Hz FIR bandpass filter  
- Selecting seizure intervals recorded at 250–256 Hz  
- Resampling all signals to a unified 250 Hz format  
- Extracting the first 300 seconds of each signal  
- Saving processed signals and seizure onset/offset labels as `.npy` files  

This notebook must be executed before training the model.

---

### 4.3. model_train.ipynb
This notebook implements the complete training workflow:

- CNN + LSTM + Attention model definition  
- Train/validation split and data loading  
- Weighted multi-task loss computation (classification + regression)  
- Performance tracking (loss curves, accuracy)  
- Saving trained model weights for inference  

The architecture is optimized for temporal pattern recognition and onset/offset prediction.

---

## 5. Usage

Clone the repository and open each notebook in order:

1. **seizure_nonseizure_data_extraction.ipynb** – preprocess the data  
2. **model_train.ipynb** – train the deep learning model  
3. **visualization.ipynb** – visualize results and model explanations  

Each notebook contains sequential, executable workflow blocks.  
No additional example code is required.
