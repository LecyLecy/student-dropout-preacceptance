# Early Student Dropout Prediction Using Pre-Acceptance Variables

This repository contains the experiment notebook and supporting outputs for the research paper:

**Early Student Dropout Prediction Using Pre-Acceptance Variables: A Comparison of Gaussian Naive Bayes and XGBoost**

## Research Overview

This study evaluates whether student dropout risk can be predicted at a very early stage using only **pre-acceptance variables**. The main motivation is that many dropout prediction studies rely on semester-based academic performance or post-acceptance information, while those variables are not available early enough for proactive intervention.

Instead of maximizing accuracy using all available student records, this study focuses on testing whether useful dropout risk signals can already be detected before academic performance records emerge.

## Dataset

The dataset used in this study is publicly available on Kaggle:

**Higher Education Predictors of Student Retention**  
https://www.kaggle.com/datasets/thedevastator/higher-education-predictors-of-student-retention

The original target contains three classes:

- Dropout
- Graduate
- Enrolled

For this study, the task is converted into binary classification:

- Graduate = 0
- Dropout = 1
- Enrolled records are removed because their final academic outcome is still unresolved.

The dataset file is **not included** in this repository. Please download it from Kaggle and upload the CSV file when running the notebook.

## Feature Settings

Two early-stage feature settings are evaluated.

### Setting A: Background-Only Variables

Setting A includes demographic, family background, and macroeconomic variables, such as:

- Marital status
- Nacionality
- Mother's qualification
- Father's qualification
- Mother's occupation
- Father's occupation
- Displaced
- Educational special needs
- Gender
- Age at enrollment
- International
- Unemployment rate
- Inflation rate
- GDP

### Setting B: Background + Application-Related Variables

Setting B includes all variables in Setting A, plus application-related variables available during admission or enrollment:

- Application mode
- Application order
- Course
- Daytime/evening attendance
- Previous qualification

### Excluded Variables

The following variables are excluded because they are post-acceptance or semester-based variables:

- Debtor status
- Tuition fees up to date
- Scholarship holder
- All first-semester curricular unit variables
- All second-semester curricular unit variables

## Models

This study compares two classification models:

1. **Gaussian Naive Bayes**
   - Used as a lightweight probabilistic baseline model.

2. **XGBoost**
   - Used as a stronger boosting-based model for structured tabular data.

## Methodology

The experiment follows these steps:

1. Load the dataset.
2. Remove `Enrolled` records.
3. Encode the target:
   - `Graduate = 0`
   - `Dropout = 1`
4. Define Setting A and Setting B feature groups.
5. Split the dataset using an 80:20 train-test split.
6. Apply preprocessing.
7. Apply SMOTE only to the training data.
8. Train Gaussian Naive Bayes and XGBoost.
9. Evaluate models using:
   - Accuracy
   - Precision
   - Recall
   - F1-score
   - Classification report
10. Generate result files and figures automatically.

## Main Result

The best overall performance was achieved by **XGBoost under Setting B**:

| Model | Setting | Accuracy | Precision | Recall | F1-score |
|---|---|---:|---:|---:|---:|
| XGBoost | B | 0.7328 | 0.6510 | 0.6831 | 0.6667 |

Gaussian Naive Bayes achieved high recall, but its low precision indicates many false positives. XGBoost produced more balanced performance, especially when application-related variables were included.

## Repository Structure

After running the notebook, the repository is expected to contain:

```text
early-student-dropout-prediction/
│
├── README.md
├── student_dropout_preacceptance_github_ready.ipynb
│
├── results/
│   ├── dataset_transformation_summary.csv
│   ├── target_distribution_binary.csv
│   ├── feature_setting_definition.csv
│   ├── model_performance_results_full_precision.csv
│   ├── model_performance_results_rounded.csv
│   ├── confusion_matrix_table.csv
│   ├── classification_report_setting_a_gnb.txt
│   ├── classification_report_setting_a_xgboost.txt
│   ├── classification_report_setting_b_gnb.txt
│   ├── classification_report_setting_b_xgboost.txt
│   ├── feature_importance_setting_b_xgboost_encoded.csv
│   └── feature_importance_setting_b_xgboost_aggregated.csv
│
└── figures/
    ├── fig2_feature_boundary.png
    └── fig3_xgboost_setting_b_feature_importance.png
```

## How to Run

1. Download the dataset from Kaggle.
2. Open the notebook in Google Colab or Jupyter Notebook.
3. Run all cells.
4. Upload the dataset CSV file when prompted.
5. The notebook will automatically generate:
   - `results/`
   - `figures/`

## Requirements

The main Python libraries used are:

```text
pandas
numpy
scikit-learn
imbalanced-learn
xgboost
matplotlib
seaborn
```

## Notes

- SMOTE is applied only to the training data to avoid data leakage.
- The test data remains unchanged to preserve the original distribution.
- The study is intended as an early warning support approach, not as an automated decision system for accepting or rejecting students.

## Authors

- Saladin Zhalifunnas Ahfar
- Michael Hendrilie
- Valentino Allen Prasetyo
- Rifqi Alfinnur Charisma
- Henry Lucky
