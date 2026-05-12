# **Deriving Health Metrics from the Photoplethysmogram: Benchmarks and Insights from MIMIC-III-Ext-PPG**

This is the official code repository for the paper 
"Deriving Health Metrics from the Photoplethysmogram: Benchmarks and Insights from MIMIC-III-Ext-PPG"
by **Mohammad Moulaeifard, Philip J. Aston, Peter Charlton, and Nils Strodthoff**.

@misc{moulaeifard2026derivinghealthmetricsphotoplethysmogram,
      title={Deriving Health Metrics from the Photoplethysmogram: Benchmarks and Insights from MIMIC-III-Ext-PPG}, 
      author={Mohammad Moulaeifard and Philip J. Aston and Peter H. Charlton and Nils Strodthoff},
      year={2026},
      eprint={2603.21832},
      archivePrefix={arXiv},
      primaryClass={cs.LG},
      url={[https://arxiv.org/abs/2603.21832](https://arxiv.org/abs/2603.21832)}, 
}


---

## **📌 Overview**
This repository contains scripts for training and evaluating AI models on **MIMIC-III-Ext-PPG** dataset. The key objectives of this project include:


---

## **🛠 Step-by-Step Procedure**

### **1. Preprocessing Dataset**
   For each dataset, first generate the corresponding signals.npy and metadata.csv files. You need to download the signals and metadata directly from PhysioNet. Please note that the PhysioNet WFDB signals contain multiple channels, including PPG, ECG, ABP, and RESP. Since this project only required PPG, we extracted the PPG channel from the downloaded WFDB files and converted it into .npy format.

 Summary of the datasets utilized in this study
 
![MIMIC-III-Ext-PPG Dataset](images/d_dataset.png)
---

### **2. Training Models**

📌 **Follow the instructions in the source section of the repository:**  

### **3. If you use our dataset, please cite our benchmarking paper**

[https://arxiv.org/abs/2603.21832
](https://arxiv.org/abs/2603.21832)
