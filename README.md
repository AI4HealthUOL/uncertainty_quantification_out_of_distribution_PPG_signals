# Uncertainty Quantification under Domain Shift for PPG-Based Blood Pressure Estimation

This repository contains the code for the paper:

**Uncertainty Reliability Under Domain Shift: An Investigation for Data-Driven Blood Pressure Estimation in Photoplethysmography**

Mohammad Moulaeifard, Ciaran Bench, Philip J. Aston, and Nils Strodthoff

---

## Overview

Uncertainty quantification (UQ) is essential for trustworthy machine learning in healthcare, especially when models are applied to data that differ from the training distribution. In this project, we investigate the reliability of uncertainty estimates for deep learning-based blood pressure (BP) estimation from photoplethysmography (PPG) signals under both in-distribution (ID) and out-of-distribution (OOD) settings.

The study evaluates predictive accuracy and uncertainty reliability for systolic blood pressure (SBP) and diastolic blood pressure (DBP) estimation using deep learning models trained on PPG signals.

---

## Main Objectives

This repository supports experiments for:

- PPG-based SBP and DBP estimation
- In-distribution and out-of-distribution evaluation
- Uncertainty quantification for regression models
- Comparison of Monte Carlo Dropout and Deep Ensembles
- Comparison of MSE and Gaussian negative log-likelihood losses
- Post-hoc uncertainty recalibration
- Evaluation of predictive accuracy and uncertainty reliability

---

## Methods

The experiments are based on an uncertainty-aware one-dimensional deep learning architecture for PPG signal analysis.

### Model Architecture

The main prediction model is based on:

- `XResNet1D-50`
- One-dimensional convolutional residual blocks
- Global average pooling for variable-length input signals
- Optional dropout inside residual blocks for Monte Carlo Dropout inference

Because the model uses global average pooling, it can process PPG segments with different temporal lengths during inference.

---

## Uncertainty Quantification Strategies

The following uncertainty estimation strategies are evaluated.

### 1. Monte Carlo Dropout

Monte Carlo Dropout keeps dropout active during inference and performs multiple stochastic forward passes. The variability across predictions is used as an estimate of epistemic uncertainty.

Evaluated dropout rates:

- `0%`
- `4%`
- `40%`

Each configuration is trained using multiple random seeds to assess robustness under underspecification.

### 2. Deep Ensembles

Deep Ensembles combine predictions from multiple independently trained models. In this project, ensembles are constructed using five independently trained models with different random initializations.

The ensemble mean is used as the final BP prediction, and the variance across ensemble members is used as an estimate of epistemic uncertainty.

### 3. Probabilistic Regression with GNLL

Gaussian negative log-likelihood loss is used to train models that predict both:

- the BP mean
- the input-dependent predictive variance

This allows the model to estimate aleatoric uncertainty.

### 4. MSE-Based Regression

Models trained with mean squared error predict only a point estimate. For these models, uncertainty is derived from stochastic inference or ensemble variability and is therefore mainly epistemic unless recalibration is applied.

---

## Post-hoc Recalibration

The repository includes post-hoc recalibration methods to improve uncertainty reliability after model training.

The evaluated recalibration methods are:

- Conformal Prediction
- Temperature Scaling
- Isotonic Regression

These methods use a separate calibration set and do not modify the trained neural network parameters.

---

## Datasets

The experiments use one in-distribution training dataset and several external OOD test datasets.

### In-Distribution Dataset

The in-distribution training data are based on the CalibFree-VitalDB subset of PulseDB.

The ID data are split into:

- training set
- validation set
- calibration set
- test set

### Out-of-Distribution Datasets

The external OOD datasets include:

- Sensors
- UCI / Cuff-Less Blood Pressure Estimation dataset
- BCG
- PPGBP

These datasets differ in acquisition setting, subject population, recording duration, and signal length.

For detailed preprocessing steps and dataset preparation, we refer readers to our previous project and paper:

> Moulaeifard M, Charlton PH, Strodthoff N. Generalizable deep learning for photoplethysmography-based blood pressure estimation—A benchmarking study. Machine Learning: Health. 2025 Dec 1;1(1):010501.

---

## Evaluation Metrics

### Predictive Accuracy

Predictive accuracy is evaluated using:

- Mean Absolute Error (MAE)

MAE is reported separately for SBP and DBP.

### Uncertainty Reliability

Uncertainty reliability is evaluated using:

- Winkler score

The Winkler score evaluates the quality of prediction intervals by combining interval width and coverage. Lower Winkler scores indicate better uncertainty estimates.

Prediction intervals are evaluated at:

- 1σ level
- 2σ level

A combined calibration summary score is computed from both interval levels.

### Statistical Analysis

Bootstrap resampling is used to estimate confidence intervals and compare methods statistically. The experiments use segment-level bootstrapping to compute confidence intervals for MAE and uncertainty calibration scores.

---

## Main Findings

The main findings of the paper are:

1. Deep Ensembles provide stronger predictive robustness under domain shift than Monte Carlo Dropout.
2. GNLL-based models provide more meaningful native uncertainty estimates than MSE-based models.
3. MSE-based uncertainty requires post-hoc recalibration to become practically useful.
4. Conformal Prediction and Temperature Scaling provide the most consistent recalibration improvements.
5. Predictive performance and uncertainty reliability both degrade under OOD conditions, showing that ID evaluation alone is not sufficient for trustworthy cuffless BP estimation.

---

## Repository Structure

A typical structure of this repository is expected to be:

```text
.
├── data/
│   └── README.md
├── models/
│   └── xresnet1d_mcd.py
├── training/
│   └── train.py
├── evaluation/
│   └── evaluate.py
├── calibration/
│   ├── conformal_prediction.py
│   ├── temperature_scaling.py
│   └── isotonic_regression.py
├── utils/
│   └── metrics.py
├── requirements.txt
└── README.md
```

Please adapt the folder names if your local repository uses a different structure.

---

## Installation

Clone the repository:

```bash
git clone https://github.com/AI4HealthUOL/uncertainty_quantification_out_of_distribution_PPG_signals.git
cd uncertainty_quantification_out_of_distribution_PPG_signals
```

Create a Python environment:

```bash
conda create -n uq_ood_ppg python=3.10
conda activate uq_ood_ppg
```

Install the required packages:

```bash
pip install -r requirements.txt
```

If `requirements.txt` is not yet available, install the main dependencies manually:

```bash
pip install numpy pandas scipy scikit-learn torch pytorch-lightning matplotlib tqdm
```

---

## Data Preparation

The raw datasets are not included in this repository. Users should download the required datasets from their original sources and follow the preprocessing pipeline described in the related publications.

This project uses preprocessed PPG segments and BP labels. The expected input format is typically:

- PPG signal array
- SBP label
- DBP label
- dataset split information
- subject or patient identifier
- optional metadata

For preprocessing details, please refer to:

> Moulaeifard, Mohammad, et al.  
> “Machine-learning for photoplethysmography analysis: Benchmarking feature, image, and signal-based approaches.”  
> *Biomedical Signal Processing and Control*, 120, 109831, 2026.

---

## Training

Example command for training a model:

```bash
python train.py \
    --data_dir path/to/preprocessed/data \
    --model xresnet1d50_mcd \
    --loss gnll \
    --dropout_rate 0.04 \
    --batch_size 512 \
    --epochs 50 \
    --lr 1e-3 \
    --seed 42
```

For MSE-based training:

```bash
python train.py \
    --data_dir path/to/preprocessed/data \
    --model xresnet1d50_mcd \
    --loss mse \
    --dropout_rate 0.04 \
    --batch_size 512 \
    --epochs 50 \
    --lr 1e-3 \
    --seed 42
```

---

## Evaluation

Example command for evaluating a trained model:

```bash
python evaluate.py \
    --checkpoint path/to/checkpoint.ckpt \
    --test_data path/to/test/data \
    --output_dir results/
```

For OOD evaluation:

```bash
python evaluate.py \
    --checkpoint path/to/checkpoint.ckpt \
    --test_data path/to/ood_dataset \
    --dataset_name UCI \
    --output_dir results/ood/
```

---

## Recalibration

Example command for post-hoc recalibration:

```bash
python recalibrate.py \
    --method conformal_prediction \
    --calibration_data path/to/calibration/data \
    --test_predictions path/to/predictions.csv \
    --output_dir results/recalibrated/
```

Supported recalibration methods:

```text
conformal_prediction
temperature_scaling
isotonic_regression
```

---

## Results

The experiments compare:

- MSE vs. GNLL loss
- Monte Carlo Dropout vs. Deep Ensembles
- Different dropout rates
- Native uncertainty vs. recalibrated uncertainty
- ID vs. OOD performance

The main reported metrics are:

- SBP MAE
- DBP MAE
- SBP Winkler score
- DBP Winkler score
- Bootstrap confidence intervals

---

## Citation

If you use this code or build on this work, please cite:

```bibtex
@article{moulaeifard2026uncertainty,
  title={Uncertainty Reliability Under Domain Shift: An Investigation for Data-Driven Blood Pressure Estimation in Photoplethysmography},
  author={Moulaeifard, Mohammad and Bench, Ciaran and Aston, Philip J. and Strodthoff, Nils},
  year={2026}
}
```

Please also cite the related preprocessing and benchmarking paper:

```bibtex
@article{moulaeifard2026machine,
  title={Machine-learning for photoplethysmography analysis: Benchmarking feature, image, and signal-based approaches},
  author={Moulaeifard, Mohammad and Coquelin, Loic and Rinkevicius, Mantas and Sološenko, Andrius and Pfeffer, Oskar and Bench, Ciaran and Hegemann, Nando and Vardanega, Sara and Nandi, Manasi and Alastruey, Jordi and others},
  journal={Biomedical Signal Processing and Control},
  volume={120},
  pages={109831},
  year={2026}
}
```

---

## Acknowledgments

This work was supported by the project 22HLT01 QUMPHY, which received funding from the European Partnership on Metrology, co-financed by the European Union’s Horizon Europe Research and Innovation Programme and by the Participating States.

---

## Contact

For questions about the code or paper, please contact:

**Mohammad Moulaeifard**  
AI4Health Department  
University of Oldenburg  
GitHub: [AI4HealthUOL](https://github.com/AI4HealthUOL)

Corresponding author:  
**Nils Strodthoff**  
University of Oldenburg
