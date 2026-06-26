# Phishing Website Detection: Reproduction and Critical Evaluation

## Project Description

This project is a reproduction and critical evaluation of the source **“Phishing Website Detection by Machine Learning Techniques.”**

The goal is to evaluate whether the original project is reproducible, whether its methodology is reliable, and how well the reported machine learning results hold under additional analysis.

The project focuses on phishing website detection, a cybersecurity task where websites are classified as either legitimate or phishing. This is important because phishing websites may lead to credential theft, fraud, malware infection, and other user harms.

## Selected Source

Original source repository:

https://github.com/shreyagopal/Phishing-Website-Detection-by-Machine-Learning-Techniques

The selected source provides:

- A defined cybersecurity problem
- Dataset files
- Feature extraction code
- Model-training notebooks
- A final extracted dataset
- Reported model results

## Dataset

The original source uses two URL sources:

- Legitimate URLs from the University of New Brunswick URL dataset
- Phishing URLs from PhishTank

The original project selected:

- 5,000 legitimate URLs
- 5,000 phishing URLs

The final dataset contains 10,000 samples and 18 columns:

- `Domain`
- 16 handcrafted numerical features
- `Label`

The label values represent:

- `0`: legitimate website
- `1`: phishing website

## Original Data Files

The original repository contains the following relevant data files:

- `1.Benign_list_big_final.csv`: raw legitimate URLs
- `2.online-valid.csv`: raw phishing URL records
- `3.legitimate.csv`: extracted features for legitimate samples
- `4.phishing.csv`: extracted features for phishing samples
- `5.urldata.csv`: final combined dataset used for modeling

In this project, the final dataset file `5.urldata.csv` was copied into:

```text
data/raw/urldata.csv
```

The `data/` directory is not committed to this repository because datasets are ignored by `.gitignore`.

## Feature Engineering

The original source already performs feature engineering before publishing the final dataset.

The features are handcrafted and belong to three main groups:

1. Address-bar based features
2. Domain-based features
3. HTML and JavaScript based features

Examples include:

- `Have_IP`
- `Have_At`
- `URL_Length`
- `URL_Depth`
- `Redirection`
- `TinyURL`
- `Prefix/Suffix`
- `DNS_Record`
- `Web_Traffic`
- `Domain_Age`
- `iFrame`
- `Mouse_Over`
- `Right_Click`
- `Web_Forwards`

In this reproduction, no additional encoding or scaling was required because the model input features were already numerical. The `Domain` column was removed from the model input because it is a string identifier.

## Project Structure

```text
.
├── data/
│   ├── raw/
│   │   └── urldata.csv
│   └── processed/
├── notebooks/
│   └── phishing_detection_reproduction.ipynb
├── report/
│   └── outputs/
│       ├── model_results.csv
│       ├── balanced_model_results.csv
│       ├── additional_metrics_results.csv
│       └── feature_importance.csv
├── src/
├── README.md
├── requirements.txt
└── .gitignore
```

## Reproducibility Notes

The project found two levels of reproducibility.

### 1. Reproducibility from intermediate files to final dataset

This level is strong.

After renaming the column `Tiny_URL` to `TinyURL` in `4.phishing.csv`, combining `3.legitimate.csv` and `4.phishing.csv` produces a dataset exactly equal to `5.urldata.csv`.

### 2. Reproducibility from raw URLs to extracted features

This level is weaker.

Some features depend on external or time-sensitive information, including:

- WHOIS records
- Web traffic rankings
- HTTP responses
- Webpage HTML
- JavaScript behavior
- Redirect history

These values can change over time, especially because phishing websites are often temporary and may disappear quickly.

## Main Findings

The reproduction and critical evaluation found several important issues.

### Dataset duplication

The original dataset contains 10,000 rows, but after removing fully duplicated rows, only 4,374 rows remain.

The duplication is especially strong in the legitimate class.

After deduplication, the class distribution changes from:

- 50% legitimate and 50% phishing

to:

- 12.09% legitimate and 87.91% phishing

This suggests that the original class balance is strongly affected by duplicated legitimate samples.

### Model performance

On the original dataset, Random Forest performed best among the tested models:

- Accuracy: 0.8670
- Macro F1-score: 0.8664
- Balanced accuracy: 0.8670

Decision Tree performed similarly.

XGBoost achieved:

- Accuracy: 0.8290

This did not fully reproduce the original source’s reported XGBoost performance of about 86.4%.

### Deduplicated dataset behavior

On the deduplicated dataset, non-weighted models achieved very high phishing recall but very low legitimate recall. This means the models tended to predict phishing too often.

Class-weighted models improved legitimate recall but reduced phishing recall, showing a tradeoff between detecting phishing websites and avoiding false alarms.

### Feature importance

The models relied mainly on a few URL-structure features:

- `URL_Length`
- `Prefix/Suffix`
- `URL_Depth`

Several features contributed very little, such as:

- `Right_Click`
- `https_Domain`
- `Redirection`

This suggests that the models may rely heavily on simple handcrafted URL patterns that attackers could potentially evade.

## Models Used

This reproduction trained and evaluated:

- Decision Tree
- Random Forest
- XGBoost
- Class-weighted Decision Tree
- Class-weighted Random Forest

## Evaluation Metrics

The project uses multiple metrics because accuracy alone is not sufficient for phishing detection.

Metrics include:

- Accuracy
- Precision
- Recall
- F1-score
- F2-score
- Balanced accuracy
- Macro F1-score
- Matthews Correlation Coefficient
- ROC-AUC
- Confusion matrix

This is important because phishing detection involves a tradeoff between security and usability.

False negatives are especially dangerous because phishing websites classified as legitimate may expose users to credential theft, fraud, or malware.

False positives also matter because legitimate websites incorrectly flagged as phishing may reduce user trust and create alert fatigue.

## How to Run the Project

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd <your-repository-folder>
```

### 2. Create and activate a virtual environment

On Windows Git Bash:

```bash
python -m venv .venv
source .venv/Scripts/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Add the dataset

Download or copy the original final dataset:

```text
5.urldata.csv
```

Place it here:

```text
data/raw/urldata.csv
```

### 5. Run the notebook

Open the notebook:

```text
notebooks/phishing_detection_reproduction.ipynb
```

Then run all cells from top to bottom.

In VS Code:

```text
Kernel → Restart Kernel and Run All Cells
```

## Output Files

The notebook saves result tables to:

```text
report/outputs/
```

Generated output files include:

- `model_results.csv`
- `balanced_model_results.csv`
- `additional_metrics_results.csv`
- `feature_importance.csv`

## Conclusion

The selected source is useful as a learning and baseline phishing-detection project. It supports the general claim that machine learning can help detect phishing websites.

However, the results should be interpreted carefully because the evaluation is affected by:

- Dataset duplication
- Class imbalance after deduplication
- Partial raw-pipeline reproducibility
- Reliance on simple handcrafted features
- Sensitivity to metric choice

A stronger future version should use a cleaner deduplicated dataset, documented random seeds, a reproducible raw-data-to-feature pipeline, time-based or domain-aware train/test splits, and more robust features that are harder for attackers to evade.
