# ECG-Age for Cardiovascular Outcome Prediction: Evaluation in an Adult Congenital Heart Disease Cohort

[![DOI](https://img.shields.io/badge/DOI-10.xxxx%2Fxxxxx-blue)](https://doi.org/10.xxxx/xxxxx)
[![Python](https://img.shields.io/badge/Python-3.11-3776AB?logo=python&logoColor=white)](https://www.python.org/)
[![MATLAB](https://img.shields.io/badge/MATLAB-R2025b-orange?logo=mathworks&logoColor=white)](https://www.mathworks.com/products/matlab.html)
[![Computing in Cardiology Conference 2026, Spain](https://img.shields.io/badge/Computing%20in%20Cardiology%20Conference-2026%2C%20Spain-purple)](https://cinc.org/)

[Seyedeh Somayyeh Mousavi](https://scholar.google.com/citations?user=gk99WMsAAAAJ&hl=en)<sup>1</sup>, [Cheryl L. Raskind-Hood](https://scholar.google.com/citations?hl=en&user=GtjhI4oAAAAJ&view_op=list_works&sortby=pubdate)<sup>2</sup>, Alexandra Haffner<sup>3</sup>, [Gari D. Clifford](https://scholar.google.com/citations?user=VwYoZ6gAAAAJ&hl=en)<sup>1,4,5,6</sup>, Wendy M. Book<sup>2,3</sup>, [Reza Sameni](https://scholar.google.com/citations?user=MkoXtWwAAAAJ&hl=en)<sup>1,4</sup>

<sup>1</sup> Dept. of Biomedical Informatics, Emory University, Atlanta, GA, USA<br>
<sup>2</sup> Dept. of Epidemiology, Rollins School of Public Health, Emory University, Atlanta, GA, USA<br>
<sup>3</sup> Dept. of Medicine, Emory University, Atlanta, GA, USA<br>
<sup>4</sup> Dept. of Biomedical Engineering, Georgia Institute of Technology, Atlanta, GA, USA<br>
<sup>5</sup> Dept. of Biomedical Engineering, Johns Hopkins University, Baltimore, MD, USA<br>
<sup>6</sup> Dept. of Anesthesia & Critical Care Medicine, Johns Hopkins University, Baltimore, MD, USA

## 🔗 Links

- [Paper (PDF)](https://scholar.google.com/scholar?hl=en&as_sdt=0%2C11&q=ECG-Age+for+Cardiovascular+Outcome+Prediction%3A+Evaluation+in+an+Adult+Congenital+Heart+Disease+Cohort&btnG=)
- [CODE15 extracted features]() (Will be published very soon)
- [Pretrained ECG-Age estimation model](./model_and_metadata/): best_model_catboost_ml_cinc2026.pkl
- [Poster](https://docs.google.com/presentation/d/1S39B-Ud01aSYuqqFkjkU77fmmsMqP9ROKIpyHA9B8yM/edit?usp=sharing)

## 📌 Overview
ECG-Age has become popular because AI models can look at an ECG and predict an apparent age (how healthy or sick a subject's heart is). Differences between ECG-Age and chronological age have been linked to cardiovascular risk, but there is an important interpretability question: is ECG-Age really a physiological biomarker, or is it a convenient summary of age-related ECG characteristics, or simply a regression to age?

We approached this by building an interpretable ECG-Age model based on engineered ECG features rather than a black-box deep network, and then applied it to a large adult congenital heart disease cohort. Our ECG-Age model performs very closely to the state-of-the-art deep models, while it is explainable and we can identify which characteristics of the ECG the model is inferring from. There are three main take-home messages:

- **First,** ECG-Age is strongly model dependent. Different models can perform similarly at the population level while giving substantially different age estimates for the same individual. So an ECG-Age difference of, say, 50 or 60 years should not automatically be interpreted as a unique biological measurement of cardiac aging vs. a subject's actual age.

- **Second,** in congenital heart disease, the ECG clearly contains information related to disease burden. On average, ECG-Age tends to look older than chronological age, particularly in younger patients, and the pattern differs across anatomical groups, with the single-ventricle population showing the most distinct pattern (they tend to have shorter lives in clinical studies).

- **Third,** for predicting future adverse outcomes, the original ECG features are more informative than summarizing all of that information into a single ECG-Age number. ECG-Age adds information beyond demographics alone, but once the ECG features are available to a machine learning model, it adds essentially no additional information.

So our overall conclusion is: **ECG-Age can be a useful summary of ECG abnormalities, but we should be cautious about treating it as a biomarker or an independent measure of biological cardiac age. For clinical prediction, the underlying ECG characteristics may be considerably more valuable than a single regression to age.**

## 📁 Structure of the Repository
This repository contains the best-performing [pretrained ECG-Age estimation model](./model_and_metadata/), [sample extracted ECG features](./features), and [codes](./codes) to run the model and generate age predictions.

```
.
├── codes/
│   └── matlab_scripts/
│   │   └── test_script_extract_features.m                    # Extract ECG features
│   └── python_scripts/
│       ├── merging_feature_csv_files.ipynb                   # Merge ECG features and add demographic info (age and sex) to the feature set
│       └── predict_ecg_age_and_evaluating.ipynb              # Predict ECG-age and evaluate the results                         
│
├── features/
│   ├── leadwise_features/                                    # Lead-wise feature CSV files (sample dataset)
│   └── multilead_features/                                   # Multi-lead feature CSV files (sample dataset)
│
├── model_and_metadata/
│   ├── best_model_catboost_ml_cinc2026.pkl                   # Best-performing pretrained ECG-age model
│   └── inference_mode_pretrained_model_outputs_cinc2026.csv  # Model outputs
│
└── requirements.txt                                          # Python package requirements
│
├── sample_data/                                              # Sample test data and .csv file includes demographic features of the records (age and gender)
│
└── README.md
```

## ⚙️ Requirements

This project requires:
- **MATLAB** for ECG feature extraction
- **Python 3.11 or later** for running the model

### Python setup

1. Open a terminal and check that Python 3.11 or later is installed:
```bash
   python --version
```

2. Create a virtual environment:
```bash
   python3.11 -m venv venv_cinc2026
```

3. Activate the virtual environment:
```bash
   # macOS / Linux
   source venv_cinc2026/bin/activate

   # Windows
   venv_cinc2026\codes\activate
```

4. Install the required packages:
```bash
   pip install -r requirements.txt
```

5. Register the environment as a Jupyter kernel, so you can select it in your notebooks:
```bash
   python -m ipykernel install --user --name cinc2026
```

### MATLAB setup

Clone the [Open-Source Electrophysiological Toolbox (OSET)](https://github.com/alphanumericslab/OSET) and add it to your MATLAB path to extract the ECG features:

```bash
git clone https://github.com/alphanumericslab/OSET.git
```

Then, in MATLAB:
```matlab
addpath(genpath('path/to/OSET'));
```

## Running the pretrained model

1. **Prepare your dataset:** All ECG records must be in WFDB format and stored in a single folder (for example: [sample_data](./sample_data)). 
2. **Consider an output folder:** Consider a folder to save the extracted .csv files (for example: [features](./features)).
3. **Extract features:** Run the feature extraction script [`extract_features.m`](codes/matlab_scripts/test_script_extract_features.m) in MATLAB.
4. **Merge feature files:** Run [`merge_features.py`](codes/python_scripts/1_Merging_feature_csv_files.ipynb) to combine all extracted feature CSV files into a single file and add sex information to it.
5. **Predict ECG-Age and evaluate the results:** Run [`predict_and_evaluate_ECG_age_.py`](codes/python_scripts/2_Predict_ecg_age_and_evaluating.ipynb) to generate age predictions.

> **Note**
> - The ECG-Age model is **sex-aware**: sex is a required input to the model, so it must be provided for each record.

## Corresponding authors:
For questions about this project, please contact [Seyedeh Somayyeh Mousavi](https://scholar.google.com/citations?user=gk99WMsAAAAJ&hl=en) (bmemousavi@gmail.com) and [Reza Sameni](https://scholar.google.com/citations?user=MkoXtWwAAAAJ&hl=en) (rsameni@dbmi.emory.edu).

## Citation
If you use this code in your research, please cite our paper:
```
Mousavi SS, Raskind-Hood CL, Haffner A, Robichaux C, Ivey LC, Clifford GD, M Book W, and Sameni R. "ECG-Age for Cardiovascular Outcome Prediction: Evaluation in an Adult Congenital Heart Disease Cohort." In Computing in Cardiology Conference (CinC), Sep 2026.
```






