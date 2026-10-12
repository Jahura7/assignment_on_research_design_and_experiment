# Methodology: Machine Learning-Based Stroke Risk Classification

## 1. Refined Research Question

How effectively can machine learning models classify the dataset's assigned stroke-risk categories (`Low`, `Moderate`, and `High`) using demographic, physiological, lifestyle, and medical-history features? In particular, how does a Random Forest Classifier compare with a Linear Regression baseline when evaluated on the same held-out test set?

## 2. Dataset Description

- **Dataset:** `stroke_risk_prediction_dataset.csv`
- **Size:** 50,000 records and 40 columns.
- **Target variable:** `Stroke_Risk`, with three categories: `Low`, `Moderate`, and `High`.
- **Feature groups:** demographic information (`Age`, `Gender`, `Country`); body measurements (`Height_cm`, `Weight_kg`, `BMI`); physiological and laboratory measurements (blood pressure, heart rate, glucose, HbA1c, cholesterol, HDL, LDL, and triglycerides); lifestyle variables (smoking, alcohol consumption, physical activity, sleep, stress, diet, exercise, and walking); medical history and conditions (family history, diabetes, hypertension, heart disease, previous TIA, atrial fibrillation, and chronic kidney disease); and other fields such as medication adherence, work type, residence type, air-pollution exposure, and consultation need.
- **Excluded fields:** `Patient_ID` is an identifier and `AI_Health_Recommendation` is recommendation text. `Stroke_Risk_Score` is excluded from model predictors because it may have been used to construct the target and could cause target leakage. It should only be reconsidered if its relationship to the target-generation process is documented and justified.
- **Limitations:** The target classes are imbalanced, some feature values are missing, and the dataset's label-generation process and clinical validity are not independently established. Therefore, results measure agreement with the dataset's assigned labels, not clinical ability to predict future stroke events.

## 3. Data Cleaning and Preprocessing

1. Inspect data types, dataset dimensions, duplicate records, target labels, and missing-value counts.
2. Remove identifier and non-predictive recommendation fields, as well as the target and potential leakage feature, from the predictor matrix.
3. Impute missing numerical values with the median and missing categorical values with the most frequent category.
4. Encode the target categories consistently for model training and reporting.
5. Encode categorical predictors. The current notebook uses `LabelEncoder`; because many predictors are nominal categories, a stronger version should use one-hot encoding (or another justified encoding) to avoid implying an artificial order between categories.
6. Split the data into training and test sets using a stratified split so each risk category is represented proportionally in both sets.
7. Fit imputers, encoders, and any scaler on the training data only, then apply the fitted transformations to the test data. This prevents preprocessing information from leaking from the test set into training.
8. Standardize numerical predictors for Linear Regression. Random Forest does not require feature scaling, so it uses the unscaled predictors.

## 4. Feature Engineering

Two derived predictors are created from existing measurements:

- **Pulse Pressure:** `Blood_Pressure_Systolic - Blood_Pressure_Diastolic`. This represents the difference between systolic and diastolic blood pressure.
- **Metabolic Risk Index:** `Blood_Glucose * BMI`. This is an exploratory interaction feature intended to represent a combined glucose–BMI signal; it is not a clinically validated risk score.

The engineered features are added before model fitting. Their contribution should be evaluated on held-out data rather than assumed to improve prediction.

## 5. Models and Rationale

### Linear Regression baseline

Linear Regression is included as the requested baseline. It predicts a continuous numeric value, so the notebook rounds and clips its predictions to the encoded target-class indices before calculating classification metrics. Because the numeric codes for `Low`, `Moderate`, and `High` are categorical labels rather than a validated continuous outcome, this is a simple comparison baseline and is not an ideal classification method.

### Random Forest Classifier

A Random Forest Classifier is used as the primary classification model. The current configuration uses 200 trees, `class_weight='balanced'`, `min_samples_leaf=2`, and `random_state=42`. Random Forest can capture nonlinear relationships and feature interactions, does not require standardization, and provides feature-importance estimates. Class weighting is included to help account for the imbalanced target distribution.

## 6. Evaluation Metrics and Visualizations

Both models are evaluated on the same stratified test set using:

- **Accuracy:** the overall proportion of correctly classified records; useful as a summary, but can be misleading when classes are imbalanced.
- **Macro precision:** computes precision separately for each class and gives each class equal weight.
- **Macro recall:** gives equal weight to each class's ability to be identified, making it useful for assessing missed risk categories.
- **Macro F1-score:** summarizes precision and recall with equal weight across classes and is the main metric for comparing performance under class imbalance.
- **Confusion matrix:** shows correct and incorrect predictions for each risk category and helps identify which classes are most often confused.

The notebook visualizes the metrics side by side, plots confusion matrices for both models, and displays the 15 highest Random Forest feature-importance values. Linear Regression MAE, RMSE, and R² are also reported for its continuous predictions, but they should be interpreted cautiously because the encoded target values represent categories rather than a true continuous scale.

## 7. Scope and Ethical Considerations

This is an educational machine-learning project using a structured dataset. Predictions refer to the dataset's existing `Stroke_Risk` labels and should not be interpreted as diagnosis or as validated predictions of future stroke. Independent validation, documentation of how the labels were generated, assessment for bias, and review by qualified clinical experts would be required before any real-world healthcare use.
