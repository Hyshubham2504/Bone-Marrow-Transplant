Bone Marrow Transplant in Children

Abstract
Pediatric bone marrow transplantation represents a critical treatment avenue for various
hematologic disorders, including malignant and non-malignant conditions. This research
delves into understanding the variables that influence the success of unmanipulated
allogeneic unrelated donor hematopoietic stem cell transplantation in pediatric patients.
Our investigation seeks to assess how various factors influence the overall success of
transplantation outcomes, considering a wide spectrum of patient demographics, medical
history, transplantation details, and recovery metrics.
Utilizing classification and regression techniques, this research endeavours to discern key
elements influencing both short-term and long-term outcomes in pediatric bone marrow
transplants. The findings from this investigation seek to provide valuable insights into
optimizing transplant procedures, enhancing patient care, and potentially refining predictive
strategies for pediatric hematologic conditions.

Introduction
Advancements in healthcare, particularly in pediatric medicine, have witnessed the
integration of machine learning (ML) into critical domains like prognostics and treatment
planning. An expanding area that demands precision and accuracy is the projection and
success prediction of bone marrow transplants in children. In pediatric hematologic
disorders, bone marrow transplantation is a crucial intervention for various malignant and
non-malignant conditions. The success of these procedures heavily relies on intricate
variables, as the intricate nature of these variables poses a challenge in accurately predicting
the transplantation outcome.
In this study, we delve into the realm of pediatric bone marrow transplantation, aiming to
harness the power of ML algorithms to distinguish and evaluate critical factors by
uncovering patterns and relationships among them, impacting the success of these
procedures. We aim to gauge their effectiveness in predicting and optimizing the success of
bone marrow transplants in children.

Data
The dataset retrieved from the UCI Machine Learning Repository comprises a vital
compilation defining pediatric hematologic disorders and bone marrow transplantation.
With 187 instances and 36 features, it provides an extensive understanding of pediatric
healthcare. These encompass a broad spectrum, encompassing patient and donor
demographics, comprehensive medical records, specific details on transplant procedures, and
resultant outcomes. The dataset comprises Integer, Categorical, and Binary data types,
reflecting various essential information crucial for investigating transplant success factors.
Figure 1 is a brief description of some of the important features of the dataset.

Methodology

Data Pre-processing and Analysis
Data pre-processing is a crucial step in the data analysis pipeline, encompassing various
techniques to enhance the quality of the raw data for analysis. The primary objectives of
data pre-processing include addressing issues such as missing values, outliers, and irrelevant
information, as well as preparing the data for specific analytical tasks. Python was used for
the analysis.

Null Value Handling
Missing values cannot be used in ML algorithms. Given the limited size of the dataset, the
chosen strategy involved incorporating ML models to deal with these. The count is given in
Figure 2.
• Regressor Imputation for Numerical Features: Random Forest Regressor was deployed
to impute missing values in numerical features, namely ’CD3dkgx10d8’ and
’Rbodymass’. The model predicted missing values based on the relationships observed
in the other features, providing a data-driven imputation.
• For Categorical Features:
– Mode Imputation: For categorical features with low null values, such as
’RecipientABO’, ’Antigen’, ’Allele’, ’RecipientRh’, ’ABOmatch’, and
’DonorCMV’, the mode (most frequent value) was used for imputation.
– Classifier Imputation: Random Forest Classifier is used to impute missing values
in features with high null values - ’extcGvHD’, ’CMVstatus’, and
’RecipientCMV’.

Outlier Treatment
1. Transformations help mitigate the impact of extreme values, making the data more
amenable to modelling. They are given in Figures 3-6.
’Rbodymass’: Log transformation was implemented.
’CD34kgx10d6’: Capped the two smallest values just above 1 followed by a log
transformation.
’CD3dCD34’: Cube root transformation with an upper cap at 0.97 quantile.
’CD3dkgx10d8’: Square root transformation capped at 0.95 quantile
2. ’ANCrecovery’, ’PLTrecovery’, and ’time to aGvHDIIIIV’ exhibit outlier values
ranging from 8 to 26, with a drastic spike to values in the millions. We’ve retained
these outliers to observe their impact on the model’s behaviour. Subsequently, we may
contemplate categorizing these features if they prove beneficial for our analysis.

Feature Engineering
1. Utilized one-hot encoding for nominal features such as ’DonorABO’ and
’RecipientABO’, converting them into a binary numerical representation.
2. Employed ordinal encoding for ordinal features like ’HLAmatch’, ’Antigen’, ’Allele’,
’HLAgrI’, and ’Recipientageint’, ensuring a meaningful numerical order.

4.1.4 Feature Selection
Correlation Heatmap: A correlation analysis with ’survival status’ was conducted and it
is given in Figure 7. No features were removed as all exhibited low correlation, indicating
that each feature contributes unique information to survival status.
Multicollinearity: Utilized Variance Inflation Factor (VIF) with a threshold of 10 to detect
multicollinearity. Identified ’CD3dCD34’, ’Recipientage’, and ’Rbodymass’ with VIF values
exceeding the threshold. Based on multicollinearity concerns, ’CD3dCD34’ was removed,
while ’Recipientage’ and ’Rbodymass’ were retained due to their theoretical importance.
Redundant Feature Removal: Based on properties and relevance, we removed redundant
features namely, ’DonorABO’, ’RecipientABO’, ’HLAmismatch’, ’Recipientage10’,
’Recipientageint’, ’aGvHDIIIIV’, ’DonorABO’, ’RecipientABO’ and ’Donorage35’.

4.2 Modeling Techniques
Classification models (Logistic Regression, Decision Tree, SVM) and XGBoost were used to
analyze the impact of explanatory variables on survival status. Categorical/binary columns
were standardized before splitting the dataset into training and testing sets. Additionally,
we created learning curves to visualize each model’s performance. (Figures 15-23).

4.2.1 Logistic Regression
Logistic Regression is a classification algorithm used to predict binary variables from the
relationship it models between the features and the binary target variable using the logistic
function. The logistic function maps the input features to a probability score of the positive
class. A logistic regression model was created using 26 features, considering survival status
as the target variable. A significance threshold of 0.05 was set to identify features
significantly associated with the target variable using Wald’s test. Backward feature
elimination was employed for feature selection, and models were designated as logistic
regression 1, 2, and 3, representing different selection methods.

4.2.2 Decision Tree
The Decision Tree, a supervised learning algorithm used for classification tasks, utilizes data
labels to build a tree structure. Internal nodes represent features, while leaf nodes hold class
labels. The tree splits data based on feature values, maximizing information gain or
minimizing Gini impurity. [1]. Hyperparameter optimization was done via Grid Search in
Sci-kit Learn, configuring parameters like minimum samples for node splitting, samples for
leaf nodes, and maximum depth. Subsequent models refer to the original and
feature-selected versions as model 1 and model 2, respectively. The visualizations are given
as Figures 13 and 14.
4.2.3 Support Vector Machine
SVM constructs a hyperplane to separate classes, minimizing error. Support vectors, the
closest points to the hyperplane, determine its position. The goal is to maximize margins,
ensuring a clear class boundary. Kernel methods map data to higher dimensions for an
optimal hyperplane. Optimized parameters include trade-off control, kernel choice, and
coefficients influencing the decision boundary. [2]
4.2.4 XGBoost
XGBoost, an ensemble method, combines decision trees to enhance accuracy. It uses
gradient boosting to build sequential, interpretable trees, correcting errors and updating
residuals. Optimal parameters, including maximum tree depth, learning rate, and training
instance fraction, were selected to prevent overfitting. The final prediction is derived from
the sum of all tree predictions.

5 Results and Inferences
A classification report summarizes the performance of a classification model by providing
metrics such as precision (accuracy of positive predictions), recall (ability to identify positive
instances), F1 score (a balanced measure of precision and recall), and accuracy. The
formulae are given in the Appendix as Figures 9 and 10. For the logistic regression, the
likelihood ratio test for each model against the null models was performed. However, the
p-value was 1 at all times and the maximum number of iterations was set very high for
feature selection. The models were termed the best depending on the highest accuracy
followed by a better precision, recall, and F1 score and considering the lesser difference in
the training and testing accuracy if the former is higher. The best outputs from each model
are given in Table 1.
<img width="207" alt="image" src="https://github.com/Hyshubham2504/Bone-Marrow-Transplant/assets/113291998/ab88f7a2-78e7-46ce-91a0-1a6dc4f8e6bb">
Table 1: Best Evaluation Metrics for Classification
For the Logistic Regression, Decision tree, and XGBoost, all features were selected but for
SVM - ’ABOmatch’, ’DonorCMV’, ’RecipientCMV’, ’Allele’, ’Recipientage’, ’extcGvHD’,
’CD34kgx10d6’, ’CD3dkgx10d8’, ’Rbodymass’, ’PLTrecovery’, ’Stemcellsource 1’,
’Relapse 1’, and ’aGvHDIIIIV’ features were selected. The outputs of the confusion matrix
and Wald’s test for logistic regression are given in Table 2 and Figure 8. The logistic
regression models didn’t show signs of overfitting, yet encountered convergence issues.
However, the other models demonstrated slight overfitting, with XGBoost exhibiting a wider
margin in comparison. The training accuracy of both XGBoost models was perfect. The
XGBoost model achieved good accuracy, the satisfactory precision indicates that the
predicted positive cases are true positives, the high recall suggests a lower likelihood of
missing actual positives, and the balanced F1 score indicates false positives and negatives
are minimal. The feature importance from the best XGBoost model are given in Figure 11
(for those of model-1 refer to Figure 12).

6 Impact of CD34 Cells
Our investigation also aimed to validate the hypothesis that an increased dosage of CD34+
cells per kilogram extends overall survival time while avoiding concurrent adverse events
impacting patients’ quality of life. Rigorous analysis focused on specific features—Relapse,
aGvHDIIIIV, and extcGvHD—each representing their absence. The primary objective was
to construct a predictive model for Survival time, emphasizing the augmentation of CD34+
cell dosage. Our findings unveiled a noteworthy trend in the relationship between CD34+
cells and disease types. Notably, a significant linear correlation was evident between CD34+
cell dosage and Non-Malignant disease cases (Figure 24). However, in Malignant disease
cases, the relationship exhibited less linearity (Figure 25). Specifically, a higher dose of
infused CD34+ cells demonstrated an association with improved outcomes in the low-risk
disease subgroup. This analysis is in line with the research conducted in this paper [4].
To evaluate the model’s performance excluding undesirable events, such as the recipient
developing acute or chronic graft versus host disease, a subset of test values meeting this
criterion was isolated. Subsequently, a linear regression model was employed to establish the
slope and intercept of the best-fit line for the data points. This analysis provided insights
into the relationship between CD34+ cell dose and survival time, distinctly for malignant
and non-malignant diseases.

7 Conclusion
In essence, our exploration into pediatric bone marrow transplantation uncovered pivotal
factors influencing transplant success. The investigation scrutinized many variables and
their profound impact on transplantation outcomes. Noteworthy among our findings was the
discernible influence of CD34+ cell dosage on treatment efficacy, particularly in different
disease subgroups. This highlighted the nuanced relationship between dosage escalation and
improved outcomes, underscoring its potential in specific patient cohorts. Despite minor
model convergence challenges, limited samples, and mild overfitting, our analysis yields
valuable insights for predicting and optimizing bone marrow transplant outcomes in
children. These insights promise to refine prognostic approaches, elevate patient care
standards, and contribute to enhancing the well-being of pediatric patients undergoing
hematologic treatments.
