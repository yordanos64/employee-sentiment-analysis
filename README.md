# Employee Sentiment Analysis

## 1. Project Overview

This project analyzes employee email sentiment using Python. It classifies emails as Positive, Neutral, or Negative, explores monthly sentiment patterns, calculates monthly employee sentiment scores, ranks employees by their monthly scores, applies a rolling 30-day negative-email rule, and evaluates a Linear Regression model for predicting monthly sentiment scores.

## 2. Objectives

- Inspect and prepare the email dataset.
- Analyze email sentiment using TextBlob.
- Explore sentiment distributions and monthly trends.
- Calculate monthly employee sentiment scores.
- Identify the three highest-scoring and three lowest-scoring employees per month.
- Apply a rolling 30-day rule for detecting repeated negative emails.
- Train and evaluate a Linear Regression model.
- Document the methodology, results, and limitations.

## 3. Technologies

- Python
- Pandas and NumPy
- Matplotlib
- TextBlob
- scikit-learn
- Jupyter Notebook

## 4. Dataset

The dataset contains 2,191 email records and four original columns:

- `Subject`: Email subject.
- `body`: Email body.
- `date`: Email date.
- `from`: Sender identifier.

The data was checked for missing values, duplicate rows, date validity, and sender consistency.

The recorded dates span January 2010 through December 2011. Sender identifiers are normalized for consistent grouping.

The original dataset contains potentially sensitive employee communications. It should remain private and should not be committed to a public repository without explicit authorization.

## 5. Methodology

### 5.1 Sentiment Analysis

TextBlob calculates a polarity score between -1 and +1 for each message.

The project uses these classification thresholds:

- Positive: polarity greater than 0.1.
- Neutral: polarity between -0.1 and 0.1, inclusive.
- Negative: polarity less than -0.1.

The email subject and body are combined for analysis.

TextBlob's default analyzer is lexicon- and rule-based; it is not trained on this dataset. The thresholds are project-specific heuristics and have not been established as universally valid classification boundaries.

### 5.2 Monthly Employee Scores

Each email is assigned a numerical score:

| Sentiment | Score |
|---|---:|
| Positive | +1 |
| Neutral | 0 |
| Negative | -1 |

For each employee and month, the scores are summed to calculate the monthly sentiment score.

The analysis also retains total message counts and sentiment-specific counts to support interpretation.

### 5.3 Monthly Employee Rankings

The project selects the three highest positive-scoring employees and the three employees with the lowest monthly scores for each month.

Ties are resolved using a consistent sender-identifier sort order.

The lowest-score ranking may include zero or positive scores if fewer than three employees have negative monthly scores. These results are interpreted as relative score rankings, not as definitive evidence about employee performance or well-being.

### 5.4 Rolling 30-Day Negative-Email Analysis

The project counts negative emails within rolling 30-day windows for each sender, including windows that cross calendar-month boundaries.

A flag is recorded when the rolling count is at least four negative emails.

This is an assessment-defined, rule-based indicator. It does not prove that an employee intends to leave their job.

### 5.5 Linear Regression

The model uses the previous month's positive, neutral, and negative email counts to predict the current month's summed sentiment score.

The dataset is sorted chronologically within each sender before previous-month features are constructed.

The chronological split is:

- Training period: February 2010 through June 2011.
- Training records: 170.
- Testing period: July 2011 through December 2011.
- Testing records: 60.

The model is evaluated using Mean Absolute Error (MAE), Root Mean Squared Error (RMSE), and R². A baseline that predicts the training-set mean is used for comparison.

## 6. Results

### 6.1 Sentiment Classification

| Sentiment | Messages | Percentage |
|---|---:|---:|
| Positive | 976 | 44.55% |
| Neutral | 1,019 | 46.51% |
| Negative | 196 | 8.95% |
| Total | 2,191 | Approximately 100% |

The monthly aggregation contains 240 employee-month records, representing 10 distinct sender identifiers across 24 months.

### 6.2 Rolling 30-Day Analysis

- Negative emails analyzed: 196.
- Flagging events: 13.
- Distinct sender identifiers flagged: 6.
- Maximum negative emails in a rolling 30-day window: 6.

These figures describe the results of the specified rule, not verified employee intentions.

### 6.3 Regression Evaluation

| Metric | Linear Regression | Baseline |
|---|---:|---:|
| MAE | 2.411 | 2.300 |
| RMSE | 2.858 | 2.796 |
| R² | -0.063 | Not reported |

The Linear Regression model performed worse than the baseline on both MAE and RMSE. Its negative R² indicates poor predictive performance on the test set.

The results do not demonstrate that the model provides useful forecasting performance with the current features and dataset. Additional data, feature engineering, validation, and model comparison would be needed before considering a forecasting solution.

## 7. Visualizations

The `visualizations/` directory contains:

- `sentiment_distribution.png`
- `monthly_sentiment_trend.png`
- `regression_actual_vs_predicted.png`

These plots show the overall sentiment distribution, monthly sentiment trends, and actual-versus-predicted regression results.

## 8. Project Structure

```text
employee-sentiment-analysis/
├── README.md
├── requirements.txt
├── Employee_Sentiment_Analysis.ipynb
├── data/
│   ├── test.xlsx
│   └── test.csv
└── visualizations/
    ├── sentiment_distribution.png
    ├── monthly_sentiment_trend.png
    └── regression_actual_vs_predicted.png
```

The filenames above describe the intended organization; the actual filenames in the project should be verified before submission.

## 9. Environment Setup

1. Create and activate a Python virtual environment.
2. Install the dependencies:

   `pip install -r requirements.txt`

3. Place the authorized dataset in the expected local `data/` directory.
4. Open the notebook and select the project's virtual environment as the Python kernel.
5. Run the notebook cells from top to bottom.

## 10. Limitations and Responsible Use

Automated sentiment analysis can misinterpret context, sarcasm, and workplace-specific language. Sentiment labels have not been validated against a human-labeled reference set.

Monthly summed scores are affected by email volume. Employees who send more messages may accumulate larger absolute scores.

The dataset contains only 10 distinct sender identifiers over 24 months. The regression results are exploratory and should not be interpreted as a reliable production forecasting system.

Sentiment rankings and rolling-window flags should not be used on their own to make employment decisions. Employee messages and sender identities must be handled confidentially.

## 11. Conclusion

This project demonstrates an end-to-end exploratory workflow covering data auditing, sentiment classification, monthly aggregation and ranking, rolling-window rule implementation, visualization, and machine-learning evaluation.

The model evaluation also highlights an important engineering principle: a successfully trained model is not necessarily a useful model. Results should be compared against a baseline, limitations should be reported honestly, and further validation should be performed before real-world deployment.