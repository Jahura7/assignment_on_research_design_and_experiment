# Literature Review: Stroke Risk Prediction Using Machine Learning

This review summarizes selected research and public-health sources relevant to stroke-risk prediction, machine-learning classification, risk factors, and model validation. These sources provide background for the project; they do not independently validate the labels or clinical reliability of the Kaggle dataset used in this repository.

## 1. Asadi et al. (2024) — *The most efficient machine learning algorithms in stroke prediction: A systematic review*

This systematic review examined studies published from 2019 to August 2023 and compared machine-learning methods used for stroke prediction. Random Forest was identified as the best-performing method in a number of included studies, although the review notes that results vary with the dataset, sample size, and data characteristics; this supports comparing models rather than assuming one algorithm is always best.

**Citation:** (Asadi et al., 2024)

## 2. Asowata et al. (2024) — *Stroke risk prediction models: A systematic review and meta-analysis*

This review assessed existing stroke risk-score models and reported an overall pooled C-statistic of approximately 0.78, suggesting fair rather than perfect predictive performance across the included studies. The authors also highlighted limited external validation and shortcomings in how some models accounted for sex, emphasizing that model evaluation and population-specific validation are important before any clinical use.

**Citation:** (Asowata et al., 2024)


## 3. Orfanoudaki et al. (2020) 

This study used machine learning to investigate the relationship between risk factors and stroke risk. The study found that stroke risk does not always follow a simple linear pattern, as traditional risk scores may suggest. Their machine learning approach identified complex, non-linear relationships between different risk factors and stroke occurrence. The findings highlight the potential of machine learning to improve stroke risk prediction. This study supports the use of advanced predictive models in healthcare research.

**Citation:** (Orfanoudaki et al., 2020)

## 4. World Health Organization — *Cardiovascular diseases* (fact sheet)

The WHO describes major behavioural and physiological risk factors for cardiovascular disease, including tobacco use, unhealthy diet, physical inactivity, harmful alcohol use, high blood pressure, high blood glucose, and overweight or obesity. This provides public-health context for why variables related to health measurements and lifestyle may be relevant to stroke-risk research, but it does not establish that every variable in the project dataset is a clinically valid predictor.

**Citation:** (Cardiovascular Diseases (CVDs), n.d.)

## Relevance to This Project

The reviewed literature supports investigating machine-learning models such as Random Forest, carefully evaluating model performance, and acknowledging limitations in validation and interpretability. In this project, the models classify the `Stroke_Risk` categories provided by the Kaggle dataset; unless the dataset labels and their generation process are independently verified, the results should be interpreted as agreement with those supplied labels rather than proof of predicting real-world stroke events.

## References

1. Asadi, F., Rahimi, M., Daeechini, A. H., & Paghe, A. (2024). The most efficient machine learning algorithms in stroke prediction: A systematic review. Health Science Reports, 7(10), e70062. https://doi.org/10.1002/hsr2.70062

2. Asowata, O. J., Okekunle, A. P., Olaiya, M. T., Akinyemi, J., Owolabi, M., & Akpa, O. M. (2024). Stroke risk prediction models: A systematic review and meta-analysis. Journal of the Neurological Sciences, 460. https://doi.org/10.1016/j.jns.2024.122997
   
3. Orfanoudaki, A., Chesley, E., Cadisch, C., Stein, B., Nouh, A., Alberts, M. J., & Bertsimas, D. (2020). Machine learning provides evidence that stroke risk is not linear: The non-linear Framingham stroke risk score. PLOS ONE, 15(5), e0232414. https://doi.org/10.1371/journal.pone.0232414
   
4. World Health Organization. *Cardiovascular diseases*. Cardiovascular diseases (CVDs). (n.d.). Retrieved October 17, 2026, from https://www.who.int/news-room/fact-sheets/detail/cardiovascular-diseases-(cvds)

   


