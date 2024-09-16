# Trendline

Trendline is a Python library designed for automatic estimation of financial asset price trendlines.

## Installation

You can install the `trendline` library using pip:

```bash
pip install trendline
```

## Usage

### Importing the Library

To use the library, first import the necessary functions:

```python
from trendline import calculate_trendlines_multiple, calculate_trendline_single
```


### Calculating Trendlines for a Single Asset
For a single asset, use the `calculate_trendline_single function`. This function calculates support and resistance trendlines based on the provided DataFrame.

Parameters:
- `dataframe (DataFrame)` : A DataFrame containing financial data for a single asset.
- `num_chunks (int, optional)` : Number of chunks to divide residuals into (default is 5).
- `residual_percentile (int, optional)` : Percentile for calculating residual cutoffs (default is 10).

Returns:
A dictionary containing trendline data for the single asset, including:
- `support_line_start` : First value of the support (low) trendline.
- `support_line_end` : Last value of the support (low) trendline.
- `support_line_gradient` : Gradient of the support (low) trendline.
- `resistance_line_start` : First value of the resistance (high) trendline.
- `resistance_line_end` : Last value of the resistance (high) trendline.
- `resistance_line_gradient` : Gradient of the resistance (high) trendline.


Example
```python
import pandas as pd
from trendline import calculate_trendline_single
import yfinance as yf
from datetime import datetime

# Set the start and end dates
start_date = "2024-01-01"
end_date = "2024-09-01"

# Fetch S&P 500 data from Yahoo Finance
sp500 = yf.download('^GSPC', start=start_date, end=end_date)
sp500 = sp500.resample('3D').ffill() 

# Calculate support and resistance line
sr = calculate_trendline_single(sp500)

print(sr)
```

Output
```python
{'resistance_line_gradient': 11.467201234683744,
 'support_line_gradient': 6.881977271455867,
 'resistance_line_start': 4924.116809777279,
 'resistance_line_end': 5841.492908551978,
 'support_line_start': 4688.572767152106,
 'support_line_end': 5239.130948868576}
```


### Calculating Trendlines for Multiple Assets
If you have data for multiple assets, use the `calculate_trendlines_multiple` function. This function calculates support and resistance trendlines for each asset in a dictionary of DataFrames.

Parameters:
- `dataframes (dict)`: Dictionary where keys are asset identifiers and values are DataFrames containing financial data for each asset.
- `num_chunks (int, optional)` : Number of chunks to divide residuals into (default is 5).
- `residual_percentile (int, optional)` : Percentile for calculating residual cutoffs (default is 10).

Returns:
A dictionary where keys are asset identifiers and values are dictionaries containing trendline data for each asset, including:
- `support_line_gradient` : Gradient of the support (low) trendline.
- `support_line_start` : First value of the support (low) trendline.
- `support_line_end` : Last value of the support (low) trendline.
- `resistance_line_start` : First value of the resistance (high) trendline.
- `resistance_line_end` : Last value of the resistance (high) trendline.
- `resistance_line_gradient` : Gradient of the resistance (high) trendline.


Example
```python
import pandas as pd
from trendline import calculate_trendlines_multiple
import yfinance as yf
from datetime import datetime

# Set the start and end dates
start_date = "2024-01-01"
end_date = "2024-09-01"

# Fetch S&P 500 data from Yahoo Finance
sp500 = yf.download('^GSPC', start=start_date, end=end_date)
sp500 = sp500.resample('3D').ffill() 

# Fetch Nikkei 225 data from Yahoo Finance
n225 = yf.download('^N225', start=start_date, end=end_date)
n225 = n225.resample('3D').ffill() 

data={"nikkei225":n225,
      "s&p500":sp500}

# Calculate multiple support and resistance lines
srs = calculate_trendlines_multiple(data)

print(srs)
```

Output
```python
{
'nikkei225':
    {
      'resistance_line_gradient': 39.69566114193926,
      'support_line_gradient': 21.805160216854723,
      'resistance_line_start': 39599.66548566491,
      'resistance_line_end': 42735.62271587811,
      'support_line_start': 33908.11786810943,
      'support_line_end': 35630.72552524095
    },
 's&p500':
    {
      'resistance_line_gradient': 11.467201234683744,
      'support_line_gradient': 6.881977271455867,
      'resistance_line_start': 4924.116809777279,
      'resistance_line_end': 5841.492908551978,
      'support_line_start': 4688.572767152106,
      'support_line_end': 5239.130948868576
    }
}
```





