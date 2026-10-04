# Network Intrusion Detection using Machine Learning (CICIDS2017)

## Overview
This project applies machine learning to detect and classify malicious network traffic. Using the CICIDS2017 intrusion detection dataset, we compare five algorithms combined with LDA (Linear Discriminant Analysis) dimensionality reduction, across different numbers of components and train/test splits, to see how each setting affects performance.

## Dataset
**CICIDS2017** (Canadian Institute for Cybersecurity, University of New Brunswick). We used three of its daily traffic capture files:

- `Tuesday-WorkingHours.pcap_ISCX.csv`
- `Wednesday-workingHours.pcap_ISCX.csv`
- `Thursday-WorkingHours-Morning-WebAttacks.pcap_ISCX.csv`

Each record is a network flow described by numerical features (flow duration, packet counts, byte rates, flag counts, etc.). The last column is the label: normal traffic (BENIGN) or an attack type such as FTP-Patator, SSH-Patator, DoS variants, or web attacks (Brute Force, XSS, SQL Injection).

> The dataset files are too large for GitHub. Download them from the official CICIDS2017 page and place them in the project folder before running the code.

## Methodology
1. **Data loading:** the three CSV files are merged into one dataset.
2. **Cleaning:** column names are stripped of whitespace, and values are converted to numeric. Infinite values are replaced with NaN and imputed using the column mean (`SimpleImputer`).
3. **Label encoding:** class labels are converted to integers with `LabelEncoder`.
4. **Train/test split:** stratified split with `random_state=0`.
5. **Dimensionality reduction:** LDA is applied to the training data, and the transform is then applied to the test data.
6. **Model training and prediction:** each of the five algorithms below is trained and evaluated.
7. **Evaluation:** confusion matrix, accuracy, precision, recall, F1-score (weighted), and a full classification report.

## Algorithms Used
| # | Algorithm | Key settings |
|---|-----------|--------------|
| 1 | Linear Regression | default |
| 2 | Ridge Regression | alpha = 1.0 |
| 3 | Elastic Net | alpha = 0.1, l1_ratio = 0.5 |
| 4 | Random Forest | n_estimators = 200, criterion = entropy |
| 5 | XGBoost | default |

## Experiment Parameters
| Parameter | Values |
|-----------|--------|
| LDA n_components | 3, 5, 10 |
| Test size | 0.2, 0.4, 0.6 |

5 algorithms × 3 LDA settings × 3 test sizes = **45 experiments**. Each experiment has its own screenshot of the output, and all results are compiled in `results/ML_results.xlsx`.

## Repository Structure
```
ML-Project/
├── README.md
├── requirements.txt
├── results/ML_results.xlsx
├── 01_Linear_Regression/   (code + 9 screenshots)
├── 02_Ridge_Regression/    (code + 9 screenshots)
├── 03_Elastic_Net/         (code + 9 screenshots)
├── 04_Random_Forest/       (code + 9 screenshots)
└── 05_XGBoost/             (code + 9 screenshots)
```
Screenshots are named `lda<N>_test<size>.png`, e.g. `lda5_test0.4.png`.

## How to Run
```bash
pip install -r requirements.txt
python 04_Random_Forest/random_forest.py
```
Change `n_components` and `test_size` at the top of the script to reproduce each experiment.

## Tools & Libraries
Python, Spyder IDE, NumPy, Pandas, Scikit-learn, XGBoost

## Results
See `results/ML_results.xlsx` for the full comparison of all 45 experiments, and the screenshots in each algorithm folder for the raw outputs.

## Team Members
- Vihaan Gowda
- Vellogi Prem Sai
- Ajaay Subramanian

## Acknowledgement
Project done as part of the Python / Machine Learning course under Prof.Suriya Prakash J.
