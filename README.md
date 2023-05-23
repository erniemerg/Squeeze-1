# Squeeze-1
Squeeze identifier 


```
import pandas as pd

# Load historical stock data
df = pd.read_csv("historical_stock_data.csv")

# Calculate Bollinger Bands
def calculate_bollinger_bands(df, window_size):
    rolling_mean = df['Close'].rolling(window=window_size).mean()
    rolling_std = df['Close'].rolling(window=window_size).std()
    df['upper_band'] = rolling_mean + (rolling_std * 2)
    df['lower_band'] = rolling_mean - (rolling_std * 2)
    return df

# Calculate squeeze
def calculate_squeeze(df, window_size, threshold):
    df = calculate_bollinger_bands(df, window_size)
    df['squeeze_on'] = ((df['upper_band'] - df['lower_band']) / df['Close']) * 100
    df['squeeze_off'] = df['squeeze_on'].shift(window_size)
    df['squeeze_alert'] = (df['squeeze_on'] < threshold) & (df['squeeze_off'] > threshold)
    return df[df['squeeze_alert']]

# Identify stock exchange squeeze
squeeze_df = calculate_squeeze(df, 20, 2.0)
print(squeeze_df)
```
