# 📊 Benchmarking Modern Deep Learning Architectures for ECG Diagnosis Using PTB-XL  
### Boston University — BA878 Final Project  
**Authors:** Bhuvan S. Gowda, Sumanth H. Kamath, Rishabh R. Suravaram, Hung T. Tran  
**Instructor:** Prof. Ned Mccague 

---

## 🌟 Project Overview  
Electrocardiograms (ECGs) are essential tools for diagnosing cardiac abnormalities, yet interpretation often varies between clinicians. This motivates the development of automated, consistent, and reliable ECG classification systems.  
In this project, we benchmark five modern deep learning architectures on the PTB-XL dataset to evaluate their effectiveness in multi-label ECG classification. Our goal is to understand how different architectural families—convolutional, multi-resolution, transformer-based, wavelet-enhanced, and graph-based models—perform under a unified and reproducible pipeline.

---

## 📁 Repository Structure

BA878_project/  
│
├── EDA and baseline model.ipynb       # Exploratory analysis + ResNet1D baseline  
├── transformer_final.ipynb            # TransformerECG implementation  
├── BA878_FINAL_CODE_1.ipynb           # Additional models (MRMT-GNN, MultiResCNN, WaveletAttention)  
├── README.md                          # Project documentation  
└── data/                              # (Not included) PTB-XL dataset files go here  


---

## 🧠 Dataset — PTB-XL  
- PTB-XL contains **27,765** clinical 12-lead ECGs (10-second recordings).  
- We used **100 Hz** low-resolution waveforms.  
- Each ECG includes **71 SCP-ECG diagnostic statements**, mapped into **five diagnostic superclasses**:

| Abbreviation | Description |
|--------------|-------------|
| NORM         | Normal ECG |
| MI           | Myocardial Infarction |
| STTC         | ST/T Abnormalities |
| CD           | Conduction Disorders |
| HYP          | Hypertrophy |

Labels were binarized using `MultiLabelBinarizer` to support multi-label classification.

### Demographic Metadata  
We used patient-level metadata:
- Age (grouped: <40, 40–59, 60–79, ≥80)
- Sex  
- Recording site  

These features were encoded as integers and used as additional model inputs.

---

## 🛠 Preprocessing Pipeline  
- Load ECG waveforms using the **WFDB** library  
- Resample to **100 Hz** if needed  
- Store signals as tensors of shape `(samples, 12_leads, 1000_timepoints)`  
- Create a custom PyTorch `Dataset` returning:  
  - ECG waveform  
  - Multi-label targets  
  - Encoded demographic variables  
- Use `DataLoader(batch_size=64)` for efficient batching and GPU training

---

## 🏗 Model Architectures  

We evaluated five models:

| Model | Description |
|-------|-------------|
| **ResNet1D** | Baseline CNN with residual connections |
| **MultiResCNN** | Multi-kernel CNN capturing multiple scales |
| **TransformerECG** | Attention-based model learning long-range dependencies |
| **WaveletAttention** | Wavelet-enhanced temporal attention model |
| **MRMT-GNN** | Hybrid CNN + Transformer + Graph Neural Network |


---

## 📏 Evaluation Metrics  

We assessed model performance using:

- **Macro AUC (primary metric)**  
- **Macro F1-score**  
- **Validation Loss**  
- **Label Accuracy**  

Macro-averaging ensures each diagnostic class contributes equally despite dataset imbalance.

---

## 📈 Results Summary  

### Key Findings  
- **TransformerECG achieved the highest AUC**, showing strong discriminative ability.  
- **ResNet1D produced the best F1-score and label accuracy**, making it the most reliable thresholded predictor.  
- **MultiResCNN performed competitively**, validating multiscale convolution.  
- **WaveletAttention and MRMT-GNN underperformed**, likely due to higher tuning demands and shorter training schedules.  

### Performance Table

| Model | AUC (Macro) | F1 (Macro) | Loss | Label Accuracy |
|-------|-------------|------------|------|----------------|
| ResNet1D | 0.8227 | 0.7396 | 0.3132 | 0.8877 |
| TransformerECG | 0.8850 | 0.6795 | 0.2774 | 0.8592 |
| MultiResCNN | 0.7936 | 0.6897 | 0.3145 | 0.8756 |
| WaveletAttention | 0.7124 | 0.5791 | 0.3857 | 0.8359 |
| MRMT-GNN | 0.7428 | 0.6434 | 0.3379 | 0.8574 |

---

## 🚧 Limitations  
- Only **20 epochs** of training; complex models require longer optimization  
- No interpretability methods (Grad-CAM, saliency maps) included  
- Only **five superclasses** used instead of full 44-label PTB-XL taxonomy  
- No demographic fairness or subgroup performance analysis conducted

---

## 🔮 Future Work  
- Train models with **longer schedules and tuned hyperparameters**  
- Incorporate **explainability tools** for clinical validation   
- Perform **subgroup analyses** for fairness evaluation  
- Explore multimodal architectures using richer clinical metadata  

---

## ▶️ Running the Code  

### 1. Download PTB-XL Dataset  
Place the files into:


### 2. Install Required Dependencies


### 3. Run the Notebooks  


---

## 📜 Citation  

If using this work, please cite our project:

**Benchmarking Modern Deep Learning Architectures for ECG Diagnosis Using PTB-XL**  
Bhuvan S. Gowda, Sumanth H. Kamath, Rishabh R. Suravaram, Hung T. Tran
Boston University, 2025

---

## 📧 Contact  
For questions or collaboration, contact:  
**Sumanth Kamath** — sumanth1810@bu.edu  
**Bhuvan Gowda** — bhuvan1201@bu.edu  
**Rishabh Reddy** - rishabh1@bu.edu  
**Hung Tran** - khungt@bu.edu
