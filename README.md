# Financial-Fraud-Detection-Model

Business Context:

This project aims to develop a model for predicting fraudulent transactions for a financial company. The goal is to use insights from the model to develop an actionable prevention plan. The provided dataset contains over 6 million rows and 10 columns of transactional data.

My Approach:

I followed a standard machine learning workflow, starting with extensive data exploration and cleaning, followed by model development, and finally, analysis and strategic recommendations.

#1. Data Cleaning and Preprocessing:
   
I began by analyzing the dataset for issues like missing values, outliers, and multicollinearity.


Missing Values: The data had no explicit NaN values, but the dictionary indicated that balance information was not available for merchant transactions. To handle this structured missingness, I created a binary feature, is_merchant_dest, and verified that the placeholder for these balances was 0.

Outliers: I used visualizations like box plots and scatter plots to identify outliers. I discovered that extreme values in the amount and oldbalanceOrg columns were highly correlated with fraudulent transactions, so I chose not to remove them. Instead, I applied log and Yeo-Johnson transformations to normalize the data's distribution without losing this critical information.

Multicollinearity: To address high correlation between balance columns, I engineered new features (balance_change_orig and balance_change_dest) to represent the change in balance for both the originating and destination accounts. I also transformed the 

step column into cyclical features (sin_hour, cos_hour, etc.) to capture time-based patterns in the data.


2. Fraud Detection Model:
   
Model Selection: I chose an XGBoost classifier for its high performance on tabular data and its ability to handle imbalanced datasets effectively.

Data Imbalance: Since fraudulent transactions were a small minority of the data, I used the SMOTE oversampling technique on the training data to create a balanced dataset for the model to learn from.

Feature Selection: I relied on both data exploration and the model's feature importance to select the most predictive variables.

3. Model Performance:

To demonstrate the model's performance, I used a set of evaluation metrics suitable for imbalanced data.

Key Metrics: The model achieved a precision of 90.04% and a recall of 86.91%, with an overall F1-score of 88.45%.

Confusion Matrix: The confusion matrix shows that the model successfully identified 1,428 true positives (correctly predicted fraud) while only having 158 false positives.

ROC Curve: The ROC curve with an AUC of 1.00 indicates an excellent ability to distinguish between the two classes.

4. Key Factors and Justification:
My feature importance analysis identified the top factors for predicting a fraudulent transaction:

balance_change_orig_yeo: This feature, which represents the change in the originating account's balance, was the most significant predictor.

ratio_amount_to_oldbalance: The ratio of the transaction amount to the old balance was the second most important feature.

type_TRANSFER: The transaction type, specifically TRANSFER, was also a key factor.

These factors make logical sense because the data dictionary explains that fraudulent agents attempt to "empty the funds by transferring to another account and then cashing out". Therefore, a significant change in a customer's balance and the type of transaction being a transfer are direct indicators of the described fraudulent behavior.


5. Prevention Strategy:
   
Based on the model's insights, I recommend the following prevention strategies for the company's infrastructure:

Real-time Monitoring: Implement a real-time system to flag transactions that exhibit high-risk patterns based on the top features identified by the model.

Enhanced Authentication: For flagged transactions, a second layer of authentication (e.g., OTP or biometric verification) should be required.

Transaction Limits: Set dynamic limits on transactions that involve unusual balance changes or high-risk transaction types.

6. Measuring Success:
   
To determine the effectiveness of these actions, I would establish key performance indicators (KPIs):

Reduction in Fraud: The primary measure of success would be a measurable decrease in the number of actual fraudulent transactions over time.

False Positive Rate: A low false-positive rate would be monitored to ensure the new system does not disrupt legitimate customer activity.

Cost-Benefit Analysis: A financial analysis would compare the cost of implementing the new system against the savings from prevented fraud.
