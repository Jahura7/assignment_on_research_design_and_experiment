# Machine Learning-Based Classification of Stroke-Risk Categories: An Exploratory Study

**Author:** Jahura Begum  
**Repository:** [assignment_on_research_design_experiment_and_writing_reseaech_paper](https://github.com/Jahura7/assignment_on_research_design_experiment_and_writing_reseaech_paper)  
**Study type:** Exploratory supervised machine-learning study

## Abstract

This study evaluates machine-learning models for classifying three assigned stroke-risk categories—High, Low, and Moderate—in a structured dataset of 50,000 records and 40 columns. Exploratory data analysis (EDA) examined class distribution, missing values, and selected demographic and physiological variables. After excluding the patient identifier, recommendation text, and `Stroke_Risk_Score`, the notebook created Pulse Pressure and a glucose–BMI interaction feature. A stratified 80:20 train–test split produced 40,000 training and 10,000 test records. A rounded Linear Regression baseline was compared with a class-weighted Random Forest classifier using accuracy and macro-averaged precision, recall, and F1-score. Random Forest achieved 85.38% accuracy and a macro F1-score of 77.94%, compared with 41.35% and 28.81%, respectively, for the Linear Regression baseline. Random Forest recall was highest for Moderate-risk records (94.88%) and lower for High (66.56%) and Low (57.33%) records. These results indicate stronger reproduction of the dataset labels by Random Forest, but potential leakage and preprocessing limitations must be addressed. The dataset labels are not independently validated clinical outcomes; therefore, the model should not be interpreted as predicting actual future strokes or used for clinical decisions.

## 1. Introduction
Stroke is a major public-health concern, and identifying people who may benefit from further assessment is an important research goal. Stroke-related risk is associated with multiple demographic, physiological, behavioural, and medical factors. The World Health Organization identifies factors such as tobacco use, unhealthy diet, physical inactivity, high blood pressure, elevated blood glucose, and overweight or obesity as relevant to cardiovascular disease prevention (World Health Organization [WHO], n.d.) (Cardiovascular Diseases (CVDs), n.d.). This provides a rationale for studying multiple health and lifestyle variables together.

Machine learning has been investigated for stroke-related prediction because it can model interactions and nonlinear patterns. In a systematic review, Asadi et al. (2024) found that Random Forest was reported as a best-performing model in several included studies, while also noting variation in performance across datasets and experimental designs. Orfanoudaki et al. (2020) explored nonlinear modelling of stroke risk, supporting the broader rationale for comparing approaches rather than assuming that relationships are purely linear. A systematic review and meta-analysis by Asowata et al. (2024) also examined the evidence base for stroke-risk prediction models, reinforcing the importance of evaluating model performance in the context of the data and validation design. However, results from published clinical studies cannot be transferred directly to a different dataset without validation.

This project uses a structured dataset with 50,000 records and 40 columns. Its target, `Stroke_Risk`, contains three assigned labels: High, Low, and Moderate. Available fields cover demographics, body measurements, blood pressure, laboratory measurements, lifestyle indicators, and medical history. The label-generation process and clinical validity of the dataset have not been independently established. Accordingly, this study measures how well models reproduce the dataset’s existing labels, not whether they predict a future stroke event in clinical practice.

**Research question:** How effectively can the dataset’s assigned High, Low, and Moderate stroke-risk categories be classified from the available predictors, and how does a Random Forest classifier compare with a rounded Linear Regression baseline on a held-out test set?

## 2. Methodology

### 2.1 Study design and dataset

This is a quantitative supervised-learning experiment using the file `stroke_risk_prediction_dataset.csv`. The notebook reports 50,000 rows and 40 columns. The target variable is `Stroke_Risk`. The class distribution is shown in Figure 1 and summarized in Table 1.

Candidate predictors include age, gender, country, height, weight, BMI, systolic and diastolic blood pressure, heart rate, glucose, HbA1c, cholesterol and lipid measurements, smoking and alcohol status, physical activity, sleep, stress, diet, family history, and medical-history indicators.

`Patient_ID` and `AI_Health_Recommendation` were removed before modelling. `Stroke_Risk_Score` was excluded from the final predictor matrix because it could be closely related to how the target label was assigned and could introduce target leakage.

### 2.2 Data inspection and preprocessing

Two additional features were created:

- **Pulse Pressure:** systolic blood pressure minus diastolic blood pressure.
- **Metabolic Risk Index:** blood glucose multiplied by BMI.

These are exploratory engineered features; the glucose–BMI product is not a clinically validated risk score. The final predictor matrix contained 38 columns. `Stroke_Risk_Score` was excluded from this matrix, along with the identifier, recommendation text, and target.

One additional caution concerns `Doctor_Consultation_Needed`, which remains among the predictors and is the most important feature in the Random Forest plot. If this field was derived from the same rules or variables used to assign `Stroke_Risk`, it may act as a proxy for the target and inflate apparent performance. Its provenance should be checked, and a sensitivity analysis should compare results with and without this field.

EDA included inspection of the dataset dimensions, data types, descriptive statistics, target counts, missing-value counts, and selected correlations. The notebook found missing observations in 13 columns. The largest counts were for `Height_cm` (1,525; 3.05%) and `Weight_kg` (1,523; 3.05%). Missing values were handled using median imputation for numerical columns and most-frequent imputation for categorical columns. Categorical predictors were encoded using `LabelEncoder`.

### 2.3 Feature engineering and leakage controls

### 2.4 Train–test split and models

The notebook used `train_test_split` with a 20% test fraction, `random_state=42`, and stratification by target. This resulted in 40,000 training records and 10,000 test records. The encoded class order was High, Low, and Moderate.

Two models were compared:

1. **Linear Regression baseline:** Linear Regression produces continuous predictions, which the notebook rounded and clipped to the available encoded class indices to calculate classification metrics. This is a weak baseline for multiclass classification because the encoded class numbers are nominal labels rather than a meaningful continuous scale.
2. **Random Forest Classifier:** the model used 200 trees, `class_weight='balanced'`, `min_samples_leaf=2`, `random_state=42`, and `n_jobs=-1`.

The notebook standardised the predictors for Linear Regression and used unscaled predictors for Random Forest.

### 2.5 Evaluation metrics

Both models were evaluated on the same held-out test set using accuracy, macro precision, macro recall, and macro F1-score. Macro averages weight the three classes equally and are useful when class proportions are uneven. Class-level precision, recall, F1-score, and confusion matrices were also examined. Random Forest feature importance was plotted to describe which input features contributed most to the fitted model; importance does not establish causality.

## 3. Results

### 3.1 Target-class distribution

The dataset contains 34,375 Moderate-risk records (68.75%), 12,112 High-risk records (24.22%), and 3,513 Low-risk records (7.03%). Moderate is the majority class, while Low is substantially underrepresented.


**Figure 1. Distribution of the assigned stroke-risk categories**

<img width="558" height="393" alt="target_distribution" src="https://github.com/user-attachments/assets/4507ab02-212d-48a3-b7b3-6b27e2bfb295" />


**Table 1. Target-class distribution**

| Assigned class | Records | Percentage |
|---|---:|---:|
| Moderate | 34,375 | 68.75% |
| High | 12,112 | 24.22% |
| Low | 3,513 | 7.03% |
| **Total** | **50,000** | **100.00%** |

### 3.2 Missing data and descriptive patterns

Thirteen columns contained missing observations. The largest gaps were in height and weight (approximately 3.05% of records each); other listed variables had missingness between 0.44% for age and 2.11% for exercise hours per week.

**Figure 2. Missing values in dataset features**

<img width="1601" height="985" alt="missing_values" src="https://github.com/user-attachments/assets/894a410e-1e71-4680-9fa0-0f2d218ceb5a" />



The notebook’s selected-variable correlation matrix showed that `Stroke_Risk_Score` had positive correlations with age (r = 0.36), systolic blood pressure (r = 0.31), blood glucose (r = 0.27), and BMI (r = 0.11). The score was excluded from the final model predictors because it may be related to the target-label construction. These correlations are descriptive and do not establish causality.

**Figure 3. Correlation matrix of selected quantitative indicators**

<img width="777" height="680" alt="correlation_matrix" src="https://github.com/user-attachments/assets/b5941cc9-39f7-46f2-9a9b-9fd8966f18d5" />

The notebook’s group comparisons found a mean age of 63.4 years in the High-risk group and 52.1 years in the Moderate-risk group. Mean blood glucose was 160.00 mg/dL in the High-risk group and 142.14 mg/dL in the Moderate-risk group. These are descriptive comparisons across pre-assigned categories; they do not establish causality, statistical significance, or validated clinical risk.

### 3.3 Comparative model performance

The models were evaluated on 10,000 test records. Random Forest outperformed the rounded Linear Regression baseline on all four reported metrics.

**Table 2. Test-set model performance**

| Model | Accuracy | Macro precision | Macro recall | Macro F1-score |
|---|---:|---:|---:|---:|
| Linear Regression (rounded predictions) | 0.4135 (41.35%) | 0.6096 (60.96%) | 0.2230 (22.30%) | 0.2881 (28.81%) |
| Random Forest Classifier | 0.8538 (85.38%) | 0.8601 (86.01%) | 0.7292 (72.92%) | 0.7794 (77.94%) |

**Figure 4. Model performance comparison on the test set**

<img width="1087" height="590" alt="model_performance_comparison" src="https://github.com/user-attachments/assets/4eba4bed-fb95-4366-8119-33571e4742b3" />

Random Forest improved accuracy by 44.03 percentage points and macro F1-score by 49.13 percentage points relative to the rounded Linear Regression baseline. Because Linear Regression is not designed for nominal multiclass classification, this comparison should be interpreted as a requested baseline comparison, not a fair comparison between two equally suitable classifiers.

### 3.4 Class-level performance and confusion matrices

The Random Forest classification report showed the following class-level results:

**Table 3. Random Forest class-level performance**

| Class | Precision | Recall | F1-score | Test support |
|---|---:|---:|---:|---:|
| High | 0.84 | 0.67 | 0.74 | 2,422 |
| Low | 0.88 | 0.57 | 0.69 | 703 |
| Moderate | 0.85 | 0.95 | 0.90 | 6,875 |
| **Macro average** | **0.86** | **0.73** | **0.78** | **10,000** |
| Weighted average | 0.85 | 0.85 | 0.85 | 10,000 |

**Figure 5. Confusion-matrix comparison**

<img width="1290" height="515" alt="confusion_matrix_comparison" src="https://github.com/user-attachments/assets/07b70132-3447-4b89-a2a3-c04be474f726" />


### 3.5 Random Forest feature importance

The notebook’s feature-importance plot ranked `Doctor_Consultation_Needed` as the most important feature, followed by `Age`, `Blood_Pressure_Systolic`, `HbA1c`, `Blood_Glucose`, and `Metabolic_Risk_Index`. Other prominent features included total cholesterol, LDL, diastolic blood pressure, pulse pressure, triglycerides, HDL, BMI, daily walking minutes, and weight.

**Figure 6. Top 15 Random Forest feature importances**

<img width="989" height="590" alt="random_forest_feature_importance" src="https://github.com/user-attachments/assets/6305980c-f216-4d33-bf3d-5d9beaa85073" />


These rankings describe how the fitted Random Forest used the available predictors. They are not evidence that a feature causes stroke risk. The high ranking of `Doctor_Consultation_Needed` warrants particular investigation for possible target leakage or a proxy relationship with the target labels.

Random Forest correctly classified 6,523 of 6,875 Moderate-risk records, 1,612 of 2,422 High-risk records, and 403 of 703 Low-risk records. Its recall was highest for Moderate (94.88%), followed by High (66.56%) and Low (57.33%). Thus, the model missed or confused a meaningful share of the High- and Low-risk records, despite its overall accuracy of 85.38%.

The rounded Linear Regression baseline had especially weak class-level performance: its recall was approximately 10% for High and 0% for Low, and it frequently assigned records to the Low or Moderate encoded classes. Its macro F1-score of 28.81% reflects this poor balance across categories.

## 4. Discussion

The main quantitative finding is that Random Forest reproduced the dataset’s assigned risk labels more successfully than the rounded Linear Regression baseline. Its test accuracy was 85.38% and macro F1-score was 77.94%, compared with 41.35% and 28.81% for Linear Regression. The class-level report gives a more qualified picture: performance was strongest for Moderate, the majority class, while recall was lower for High and especially Low. This is important because accuracy alone can obscure weak performance on less frequent classes.

The exploratory analysis also found that the High-risk group had higher mean age and blood glucose than the Moderate-risk group. These observations are consistent with the general rationale for studying demographic and physiological variables, but the dataset’s label-generation process is not independently validated. They should therefore be treated as descriptive properties of this dataset, not as evidence of causal or clinical relationships.

Several methodological limitations reduce confidence in the reported test metrics. First, imputation and label encoding were fitted before the train–test split. This means preprocessing was not fully isolated from the test set; the experiment should be rerun using a train-only preprocessing pipeline. Second, `Doctor_Consultation_Needed` was retained and ranked as the most important predictor. Its relationship to the target should be investigated because it may encode a decision that is itself based on the risk category or closely related criteria. Excluding `Stroke_Risk_Score` is a useful safeguard, but it does not eliminate every possible leakage pathway.

Third, the target is imbalanced: only 7.03% of records are Low risk. Although `class_weight='balanced'` was used for Random Forest, Low-class recall was still 57.33%. Further work should examine per-class precision–recall trade-offs and consider additional classifiers and carefully applied resampling or class-weighting approaches. Fourth, the Linear Regression baseline is not an appropriate primary model for nominal classes; Logistic Regression or another classification algorithm would be a more defensible baseline. Finally, there is no independent external validation, and feature importance does not establish causation.

These limitations mean that the reported performance should be interpreted as an initial experiment in reproducing labels within this dataset. It does not demonstrate that the model predicts future strokes or is ready for clinical use. Before any healthcare application, data provenance and label definitions must be verified, leakage risks addressed, preprocessing corrected, and the model evaluated on independent, clinically reliable data from representative populations.

## 5. Conclusion

In this experiment, the Random Forest Classifier outperformed a rounded Linear Regression baseline when classifying the dataset’s assigned High, Low, and Moderate stroke-risk categories. Random Forest achieved 85.38% accuracy and a macro F1-score of 77.94%, while the baseline achieved 41.35% accuracy and a macro F1-score of 28.81%. However, Random Forest recall was lower for Low (57.33%) and High (66.56%) than for Moderate (94.88%), showing that overall accuracy does not fully describe class-level performance. Potential preprocessing leakage and the prominence of `Doctor_Consultation_Needed` must be investigated, and the labels require independent validation. The findings are therefore preliminary evidence of classification performance on this dataset, not validated prediction of future stroke events or a basis for clinical decisions.

## References

Asadi, F., Rahimi, M., Daeechini, A. H., & Paghe, A. (2024). The most efficient machine learning algorithms in stroke prediction: A systematic review. Health Science Reports, 7(10), e70062. https://doi.org/10.1002/hsr2.70062

Asowata, O. J., Okekunle, A. P., Olaiya, M. T., Akinyemi, J., Owolabi, M., & Akpa, O. M. (2024). Stroke risk prediction models: A systematic review and meta-analysis. Journal of the Neurological Sciences, 460. https://doi.org/10.1016/j.jns.2024.122997

Cardiovascular diseases (CVDs). (n.d.). Retrieved October 17, 2026, from https://www.who.int/news-room/fact-sheets/detail/cardiovascular-diseases-(cvds)

Orfanoudaki, A., Chesley, E., Cadisch, C., Stein, B., Nouh, A., Alberts, M. J., & Bertsimas, D. (2020). Machine learning provides evidence that stroke risk is not linear: The non-linear Framingham stroke risk score. PLOS ONE, 15(5), e0232414. https://doi.org/10.1371/journal.pone.0232414
