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

