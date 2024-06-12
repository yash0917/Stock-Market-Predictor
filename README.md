# Stock Market Predictor
 
This code is designed to perform financial analysis and prediction on the S&P 500 index using historical data from Yahoo Finance and machine learning techniques. Here's a breakdown of what the code does:

1. **Data Loading and Preparation:**
   - The code attempts to load the S&P 500 data from a local CSV file (`sp500.csv`). If the file does not exist, it downloads the data using the `yfinance` library and saves it as `sp500.csv`.
   - The historical data includes columns such as `Open`, `High`, `Low`, `Close`, `Volume`, `Dividends`, and `Stock Splits`.
   - The index of the DataFrame is converted to datetime format.
   - The `Dividends` and `Stock Splits` columns are deleted.
   - A new column `Tomorrow` is created, representing the next day's closing price.
   - A new column `Target` is created, indicating whether the next day's closing price is higher than the current day's closing price (binary classification: 1 for up, 0 for down).

2. **Initial Model Training and Evaluation:**
   - A `RandomForestClassifier` model is initialized with specific parameters.
   - The data is split into training (all but the last 100 rows) and test sets (last 100 rows).
   - The model is trained using the predictors `["Close", "Volume", "Open", "High", "Low"]`.
   - The precision score of the model is evaluated on the test set.

3. **Backtesting:**
   - A `predict` function is defined to fit the model on the training data and make predictions on the test data, combining the actual and predicted values into a DataFrame.
   - A `backtest` function is defined to perform rolling window backtesting. It trains the model on a growing window of data and tests on subsequent data in steps, accumulating predictions over time.
   - The precision score and value counts of predictions are calculated for the backtest results.

4. **Feature Engineering:**
   - Rolling averages and trends are calculated for different time horizons (`2, 5, 60, 250, 1000` days).
   - New predictors are created: the ratio of the closing price to the rolling average (`Close_Ratio_horizon`) and the trend over the horizon (`Trend_horizon`).
   - The DataFrame is updated to include these new predictors and any rows with missing values (except for the `Tomorrow` column) are dropped.

5. **Enhanced Model Training and Evaluation:**
   - The model is re-initialized with different parameters (`n_estimators=200`, `min_samples_split=50`).
   - The `predict` function is modified to use probability thresholds for predictions.
   - The backtesting process is repeated using the new predictors.
   - The precision score and value counts of the updated predictions are calculated.

6. **Output:**
   - The final predictions DataFrame contains the actual targets and the model's predictions.

Overall, the code aims to predict whether the S&P 500 index will close higher or lower the next day, using historical data and machine learning, and evaluates the model's performance using backtesting and precision metrics.
