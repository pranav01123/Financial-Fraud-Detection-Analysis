🛡️ Financial Fraud Detection Analysis
An end-to-end machine learning project that detects fraudulent financial transactions. This analysis moves from initial exploratory data analysis on a sample to building and validating a robust RandomForestClassifier on a full dataset of over 6.3 million records, with a special focus on handling severe class imbalance.

✨ Key Features
Strategic EDA: Initial analysis performed on a 10,000-row sample to rapidly prototype and identify key fraud patterns without high computational cost.

Intelligent Feature Engineering: Creation of a powerful funds_emptied feature that directly models the primary behavior of fraudsters clearing out accounts.

Iterative Model Development: A clear progression from a simple baseline (Logistic Regression) to a more complex, effective model (Random Forest) to solve the core challenge of class imbalance.

Imbalanced Data Handling: Employs class_weight='balanced' to force the model to prioritize rare but critical fraud cases.

Full Dataset Validation: The final, optimized pipeline is applied to the complete 6.3 million+ row dataset to provide accurate and reliable performance metrics.

🔬 Methodology
The project follows a structured workflow, from understanding the data to providing actionable business recommendations.

1. Exploratory Data Analysis (on Sample)
The project began by analyzing a 10,000-row sample to quickly draw insights.

Data Quality: The dataset was found to be clean with no missing values.

Outlier Analysis: Extreme outliers in the amount column were intentionally kept, as anomalous, high-value transactions are often strong indicators of fraud.

Hypothesis Validation: The analysis confirmed that high-value transactions (> $200,000) are 1.5 times more likely to be fraudulent. It also revealed that CASH_OUT and TRANSFER are the highest-risk transaction types.

2. Feature Engineering: The funds_emptied Signal
The most impactful step was engineering a new feature to capture the primary signal of fraud described in the data dictionary. A new binary column, funds_emptied, was created to flag transactions that completely drain an account.

Python

# A value of 1 indicates the account's funds were emptied.
df['funds_emptied'] = ((df['oldbalanceOrg'] > 0) & (df['newbalanceOrig'] == 0)).astype(int)
This single feature proved to be the most powerful predictor in the final model.

3. Model Development & Iteration
The core challenge was the severe class imbalance. A simple accuracy metric would be misleading, so the focus was on improving recall for the fraud class.

Baseline (Logistic Regression): Achieved ~99% accuracy but had a very poor recall of only 7% for fraud. It was great at identifying legitimate transactions but missed almost all fraudulent ones.

Final Model (Random Forest Classifier): A RandomForestClassifier with class_weight='balanced' provided the best balance. It was flexible enough to handle the class imbalance, significantly improving fraud detection while maintaining high overall performance.

📈 Model Performance
Since accuracy is a misleading metric for this problem, the Classification Report was used as the primary evaluation tool. The final model was trained and tested on the full 6.3M+ row dataset.

Final Model Performance (on Full Dataset):
| Class | Precision | Recall | F1-Score |
| :--- | :--- | :--- | :--- |
| Not Fraud (0) | 1.00 | 1.00 | 1.00 |
| Fraud (1) | 0.18 | 0.21 | 0.20 |

The model successfully identified 21% of all fraudulent transactions in the test set. While many fraud cases still go undetected, this represents a massive improvement over a baseline model and provides a strong foundation for a fraud prevention system.

📊 Analysis and Insights
What are the key factors that predict fraudulent transactions?

The single most predictive factor is whether a transaction empties an account's funds (the funds_emptied feature).

High transaction amounts are the next strongest predictor.

The transaction type is crucial: TRANSFER and CASH_OUT are the riskiest.

Do these factors make sense?

Yes, perfectly. The engineered funds_emptied feature directly models the criminal behavior of draining an account. The fact that the model found this to be the most important variable confirms that it is learning logical, real-world patterns of fraud.

🚀 Recommendations & Business Strategy
Prevention Strategy
I recommend a two-tiered prevention strategy:

Rule-Based Alert: Implement a high-priority rule to immediately flag any TRANSFER transaction where the funds_emptied feature is true. This pattern is a very high-confidence indicator of fraud.

ML Model Scoring: Use the full Random Forest model to score all other transactions, flagging those with a high probability of being fraudulent for further review.

Measuring Success
To determine if these actions are effective, the company should conduct an A/B test:

Roll out the new rule and model to a small subset of users (e.g., 5%).

Compare key metrics (fraud detection rate, false positive rate, customer friction) against the remaining 95% of users. This will provide clear evidence of the system's value before a full-scale deployment.

⚙️ How to Run the Project
1. Prerequisites
The project requires Python 3 and the following libraries:

pandas

numpy

scikit-learn

seaborn

matplotlib

2. Setup

Clone the repository:

Bash

git clone https://github.com/YourUsername/Financial-Fraud-Detection.git
Install the required packages:

Bash

pip install pandas numpy scikit-learn seaborn matplotlib
Download the dataset from the link below and place the Fraud.csv file in the project's root directory.

3. Execution

Run the Jupyter Notebook fraud-detection-project.ipynb to execute the full analysis and see the results.

💾 Data Source
Dataset: Synthetic Financial Dataset for Fraud Detection
