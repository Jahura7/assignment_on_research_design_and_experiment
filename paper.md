# Machine Learning-Based Classification of Stroke-Risk Categories: An Exploratory Study

**Author:** Jahura Begum  
**Repository:** [assignment_on_research_design_experiment_and_writing_reseaech_paper](https://github.com/Jahura7/assignment_on_research_design_experiment_and_writing_reseaech_paper)  
**Study type:** Exploratory supervised machine-learning study

## Abstract

This study evaluates machine-learning models for classifying three assigned stroke-risk categories—High, Low, and Moderate—in a structured dataset of 50,000 records and 40 columns. Exploratory data analysis (EDA) examined class distribution, missing values, and selected demographic and physiological variables. After excluding the patient identifier, recommendation text, and `Stroke_Risk_Score`, the notebook created Pulse Pressure and a glucose–BMI interaction feature. A stratified 80:20 train–test split produced 40,000 training and 10,000 test records. A rounded Linear Regression baseline was compared with a class-weighted Random Forest classifier using accuracy and macro-averaged precision, recall, and F1-score. Random Forest achieved 85.38% accuracy and a macro F1-score of 77.94%, compared with 41.35% and 28.81%, respectively, for the Linear Regression baseline. Random Forest recall was highest for Moderate-risk records (94.88%) and lower for High (66.56%) and Low (57.33%) records. These results indicate stronger reproduction of the dataset labels by Random Forest, but potential leakage and preprocessing limitations must be addressed. The dataset labels are not independently validated clinical outcomes; therefore, the model should not be interpreted as predicting actual future strokes or used for clinical decisions.

## 1. Introduction
Stroke is a major public-health concern, and identifying people who may benefit from further assessment is an important research goal. Stroke-related risk is associated with multiple demographic, physiological, behavioural, and medical factors. The World Health Organization identifies factors such as tobacco use, unhealthy diet, physical inactivity, high blood pressure, elevated blood glucose, and overweight or obesity as relevant to cardiovascular disease prevention (World Health Organization [WHO], n.d.). This provides a rationale for studying multiple health and lifestyle variables together.

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
