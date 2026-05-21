# Uncertainty Quantification under Domain Shift for PPG-Based Blood Pressure Estimation

This repository contains the code for the paper:

https://arxiv.org/abs/2605.18008

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



## Citation

If you use this code or build on this work, please cite:



Please also cite the related preprocessing and benchmarking paper:

```bibtex
@article{moulaeifard2025generalizable,
  title={Generalizable deep learning for photoplethysmography-based blood pressure estimation—A benchmarking study},
  author={Moulaeifard, Mohammad and Charlton, Peter H and Strodthoff, Nils},
  journal={Machine Learning: Health},
  volume={1},
  number={1},
  pages={010501},
  year={2025},
  publisher={IOP Publishing}
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
