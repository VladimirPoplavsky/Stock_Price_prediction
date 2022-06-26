```python
import re
import json
import csv
import requests
from bs4 import BeautifulSoup ,SoupStrainer
import pandas as pd 
from io import StringIO
from pathlib import Path  

#!pip install selenium

import os
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.service import Service



import sys

import unittest

import time

```


```python
def get_html_page(chromedriver_path="./chromedriver.exe", scroll_number=50, sleep_time=2, url = ""):
    # Create selenium driver
    s = Service(chromedriver_path)
    driver = webdriver.Chrome(service=s)
    driver.get(url)

    for i in range(1,scroll_number):
        print(f"scroll_number: {i}")
        driver.execute_script("window.scrollTo(1,100000)")
        time.sleep(sleep_time)
    html_data = driver.page_source
    driver.close()
    
    soup = BeautifulSoup(html_data, 'html.parser')    
    return soup

```


```python
def make_table(soup):
    table = soup.find_all('table')
    df = pd.read_html(str(table))[0]
    return df
```


```python
def data_cleaning(df):
    df = df[df["Open"].str.contains("Dividend") == False]
    df = df[df["Open"].str.contains("Stock") == False]
    df.drop(df.tail(1).index,inplace=True)
    return df
```


```python
def write_to_file(df, path):
    from pathlib import Path
    filepath = Path(path)
    filepath.parent.mkdir(parents=True, exist_ok=True)
    df.to_csv(filepath) 
```


```python
base_url = "https://finance.yahoo.com/quote/"
period_url = "/history?period1=1262304000&period2=1656115200&interval=1d&filter=history&frequency=1d&includeAdjustedClose=true"
indexes = ["NVDA","AAPL","TSLA","AMD","MSFT","INTC"]
main_df = pd.DataFrame()

for index in indexes:
    soup = get_html_page(url = (base_url + index + period_url))
    table = soup.find_all('table')
    df = pd.read_html(str(table))[0]
    
    df = data_cleaning(df)
    
    path = index + ".csv"
    write_to_file(df, path)
    
```

    scroll_number: 1
    scroll_number: 2
    scroll_number: 3
    scroll_number: 4
    scroll_number: 5
    scroll_number: 6
    scroll_number: 7
    scroll_number: 8
    scroll_number: 9
    scroll_number: 10
    scroll_number: 11
    scroll_number: 12
    scroll_number: 13
    scroll_number: 14
    scroll_number: 15
    scroll_number: 16
    scroll_number: 17
    scroll_number: 18
    scroll_number: 19
    scroll_number: 20
    scroll_number: 21
    scroll_number: 22
    scroll_number: 23
    scroll_number: 24
    scroll_number: 25
    scroll_number: 26
    scroll_number: 27
    scroll_number: 28
    scroll_number: 29
    scroll_number: 30
    scroll_number: 31
    scroll_number: 32
    scroll_number: 33
    scroll_number: 34
    scroll_number: 35
    scroll_number: 36
    scroll_number: 37
    scroll_number: 38
    scroll_number: 39
    scroll_number: 40
    scroll_number: 41
    scroll_number: 42
    scroll_number: 43
    scroll_number: 44
    scroll_number: 45
    scroll_number: 46
    scroll_number: 47
    scroll_number: 48
    scroll_number: 49
    scroll_number: 1
    scroll_number: 2
    scroll_number: 3
    scroll_number: 4
    scroll_number: 5
    scroll_number: 6
    scroll_number: 7
    scroll_number: 8
    scroll_number: 9
    scroll_number: 10
    scroll_number: 11
    scroll_number: 12
    scroll_number: 13
    scroll_number: 14
    scroll_number: 15
    scroll_number: 16
    scroll_number: 17
    scroll_number: 18
    scroll_number: 19
    scroll_number: 20
    scroll_number: 21
    scroll_number: 22
    scroll_number: 23
    scroll_number: 24
    scroll_number: 25
    scroll_number: 26
    scroll_number: 27
    scroll_number: 28
    scroll_number: 29
    scroll_number: 30
    scroll_number: 31
    scroll_number: 32
    scroll_number: 33
    scroll_number: 34
    scroll_number: 35
    scroll_number: 36
    scroll_number: 37
    scroll_number: 38
    scroll_number: 39
    scroll_number: 40
    scroll_number: 41
    scroll_number: 42
    scroll_number: 43
    scroll_number: 44
    scroll_number: 45
    scroll_number: 46
    scroll_number: 47
    scroll_number: 48
    scroll_number: 49
    scroll_number: 1
    scroll_number: 2
    scroll_number: 3
    scroll_number: 4
    scroll_number: 5
    scroll_number: 6
    scroll_number: 7
    scroll_number: 8
    scroll_number: 9
    scroll_number: 10
    scroll_number: 11
    scroll_number: 12
    scroll_number: 13
    scroll_number: 14
    scroll_number: 15
    scroll_number: 16
    scroll_number: 17
    scroll_number: 18
    scroll_number: 19
    scroll_number: 20
    scroll_number: 21
    scroll_number: 22
    scroll_number: 23
    scroll_number: 24
    scroll_number: 25
    scroll_number: 26
    scroll_number: 27
    scroll_number: 28
    scroll_number: 29
    scroll_number: 30
    scroll_number: 31
    scroll_number: 32
    scroll_number: 33
    scroll_number: 34
    scroll_number: 35
    scroll_number: 36
    scroll_number: 37
    scroll_number: 38
    scroll_number: 39
    scroll_number: 40
    scroll_number: 41
    scroll_number: 42
    scroll_number: 43
    scroll_number: 44
    scroll_number: 45
    scroll_number: 46
    scroll_number: 47
    scroll_number: 48
    scroll_number: 49
    scroll_number: 1
    scroll_number: 2
    scroll_number: 3
    scroll_number: 4
    scroll_number: 5
    scroll_number: 6
    scroll_number: 7
    scroll_number: 8
    scroll_number: 9
    scroll_number: 10
    scroll_number: 11
    scroll_number: 12
    scroll_number: 13
    scroll_number: 14
    scroll_number: 15
    scroll_number: 16
    scroll_number: 17
    scroll_number: 18
    scroll_number: 19
    scroll_number: 20
    scroll_number: 21
    scroll_number: 22
    scroll_number: 23
    scroll_number: 24
    scroll_number: 25
    scroll_number: 26
    scroll_number: 27
    scroll_number: 28
    scroll_number: 29
    scroll_number: 30
    scroll_number: 31
    scroll_number: 32
    scroll_number: 33
    scroll_number: 34
    scroll_number: 35
    scroll_number: 36
    scroll_number: 37
    scroll_number: 38
    scroll_number: 39
    scroll_number: 40
    scroll_number: 41
    scroll_number: 42
    scroll_number: 43
    scroll_number: 44
    scroll_number: 45
    scroll_number: 46
    scroll_number: 47
    scroll_number: 48
    scroll_number: 49
    scroll_number: 1
    scroll_number: 2
    scroll_number: 3
    scroll_number: 4
    scroll_number: 5
    scroll_number: 6
    scroll_number: 7
    scroll_number: 8
    scroll_number: 9
    scroll_number: 10
    scroll_number: 11
    scroll_number: 12
    scroll_number: 13
    scroll_number: 14
    scroll_number: 15
    scroll_number: 16
    scroll_number: 17
    scroll_number: 18
    scroll_number: 19
    scroll_number: 20
    scroll_number: 21
    scroll_number: 22
    scroll_number: 23
    scroll_number: 24
    scroll_number: 25
    scroll_number: 26
    scroll_number: 27
    scroll_number: 28
    scroll_number: 29
    scroll_number: 30
    scroll_number: 31
    scroll_number: 32
    scroll_number: 33
    scroll_number: 34
    scroll_number: 35
    scroll_number: 36
    scroll_number: 37
    scroll_number: 38
    scroll_number: 39
    scroll_number: 40
    scroll_number: 41
    scroll_number: 42
    scroll_number: 43
    scroll_number: 44
    scroll_number: 45
    scroll_number: 46
    scroll_number: 47
    scroll_number: 48
    scroll_number: 49
    scroll_number: 1
    scroll_number: 2
    scroll_number: 3
    scroll_number: 4
    scroll_number: 5
    scroll_number: 6
    scroll_number: 7
    scroll_number: 8
    scroll_number: 9
    scroll_number: 10
    scroll_number: 11
    scroll_number: 12
    scroll_number: 13
    scroll_number: 14
    scroll_number: 15
    scroll_number: 16
    scroll_number: 17
    scroll_number: 18
    scroll_number: 19
    scroll_number: 20
    scroll_number: 21
    scroll_number: 22
    scroll_number: 23
    scroll_number: 24
    scroll_number: 25
    scroll_number: 26
    scroll_number: 27
    scroll_number: 28
    scroll_number: 29
    scroll_number: 30
    scroll_number: 31
    scroll_number: 32
    scroll_number: 33
    scroll_number: 34
    scroll_number: 35
    scroll_number: 36
    scroll_number: 37
    scroll_number: 38
    scroll_number: 39
    scroll_number: 40
    scroll_number: 41
    scroll_number: 42
    scroll_number: 43
    scroll_number: 44
    scroll_number: 45
    scroll_number: 46
    scroll_number: 47
    scroll_number: 48
    scroll_number: 49
    


```python
indexes = ["NVDA","AAPL","TSLA","AMD","MSFT","INTC"]
main_df = pd.DataFrame()

for index in indexes:
    
    df = pd.read_csv( index + '.csv')
    df.drop(df.index[2500:], inplace=True)
    df.set_index('Date')
   
    
    main_df['Date'] = df['Date']
    main_df[index + " " + "Open"] = df['Open']
    main_df[index + " " + "High"] = df['High']
    main_df[index + " " + "Low"] = df['Low']
    main_df[index + " " + "Close"] = df['Close*']
    main_df[index + " " + "Volume"] = df['Volume']
    
    main_df[index + " " +'Daily change'] = (((df['Close*']).astype(float))-((df['Open']).astype(float)))/((df['Open']).astype(float))*100
    main_df[index + " " +'Daily change Max'] =  abs((df['High'].astype(float)-df['Low'].astype(float))/df['Low'].astype(float)*100)
    


    
    print(df)
    print(df.info())


main_df['Date'] = df['Date']
main_df.set_index('Date')
write_to_file(main_df, ("main_dataframe" + ".csv"))
main_df
```

          Unnamed: 0          Date    Open    High     Low  Close*  Adj Close**  \
    0              0  Jun 24, 2022  165.00  171.40  163.10  171.26       171.26   
    1              1  Jun 23, 2022  165.19  165.85  158.53  162.25       162.25   
    2              2  Jun 22, 2022  162.26  166.62  161.80  163.60       163.60   
    3              3  Jun 21, 2022  164.75  170.08  164.07  165.66       165.66   
    4              4  Jun 17, 2022  156.48  159.95  153.28  158.80       158.80   
    ...          ...           ...     ...     ...     ...     ...          ...   
    2495        2535  Jul 25, 2012    3.20    3.31    3.18    3.27         3.01   
    2496        2536  Jul 24, 2012    3.25    3.27    3.17    3.21         2.95   
    2497        2537  Jul 23, 2012    3.12    3.27    3.08    3.24         2.98   
    2498        2538  Jul 20, 2012    3.28    3.30    3.17    3.20         2.94   
    2499        2539  Jul 19, 2012    3.29    3.34    3.27    3.30         3.03   
    
            Volume  
    0     47166300  
    1     46368000  
    2     43713500  
    3     48308900  
    4     62905700  
    ...        ...  
    2495  41504400  
    2496  35116400  
    2497  45158400  
    2498  45100000  
    2499  40303600  
    
    [2500 rows x 8 columns]
    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 2500 entries, 0 to 2499
    Data columns (total 8 columns):
     #   Column       Non-Null Count  Dtype  
    ---  ------       --------------  -----  
     0   Unnamed: 0   2500 non-null   int64  
     1   Date         2500 non-null   object 
     2   Open         2500 non-null   float64
     3   High         2500 non-null   float64
     4   Low          2500 non-null   float64
     5   Close*       2500 non-null   float64
     6   Adj Close**  2500 non-null   float64
     7   Volume       2500 non-null   int64  
    dtypes: float64(5), int64(2), object(1)
    memory usage: 175.8+ KB
    None
          Unnamed: 0          Date    Open    High     Low  Close*  Adj Close**  \
    0              0  Jun 24, 2022  139.90  141.91  139.77  141.66       141.66   
    1              1  Jun 23, 2022  136.82  138.59  135.63  138.27       138.27   
    2              2  Jun 22, 2022  134.79  137.76  133.91  135.35       135.35   
    3              3  Jun 21, 2022  133.42  137.06  133.32  135.87       135.87   
    4              4  Jun 17, 2022  130.07  133.08  129.81  131.56       131.56   
    ...          ...           ...     ...     ...     ...     ...          ...   
    2495        2537  Jul 25, 2012   20.52   20.74   20.36   20.53        17.56   
    2496        2538  Jul 24, 2012   21.69   21.77   21.38   21.46        18.35   
    2497        2539  Jul 23, 2012   21.23   21.64   20.99   21.57        18.44   
    2498        2540  Jul 20, 2012   21.89   21.94   21.56   21.58        18.45   
    2499        2541  Jul 19, 2012   21.83   21.98   21.64   21.94        18.76   
    
             Volume  
    0      89047400  
    1      72433800  
    2      73409200  
    3      81000500  
    4     134118500  
    ...         ...  
    2495  877312800  
    2496  565132400  
    2497  487975600  
    2498  397471200  
    2499  436861600  
    
    [2500 rows x 8 columns]
    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 2500 entries, 0 to 2499
    Data columns (total 8 columns):
     #   Column       Non-Null Count  Dtype  
    ---  ------       --------------  -----  
     0   Unnamed: 0   2500 non-null   int64  
     1   Date         2500 non-null   object 
     2   Open         2500 non-null   float64
     3   High         2500 non-null   float64
     4   Low          2500 non-null   float64
     5   Close*       2500 non-null   float64
     6   Adj Close**  2500 non-null   float64
     7   Volume       2500 non-null   int64  
    dtypes: float64(5), int64(2), object(1)
    memory usage: 175.8+ KB
    None
          Unnamed: 0          Date    Open    High     Low  Close*  Adj Close**  \
    0              0  Jun 24, 2022  712.41  738.20  708.26  737.12       737.12   
    1              1  Jun 23, 2022  713.72  717.95  685.91  705.21       705.21   
    2              2  Jun 22, 2022  703.51  740.50  701.48  708.26       708.26   
    3              3  Jun 21, 2022  673.81  730.73  673.00  711.11       711.11   
    4              4  Jun 17, 2022  640.30  662.91  639.59  650.28       650.28   
    ...          ...           ...     ...     ...     ...     ...          ...   
    2495        2496  Jul 25, 2012    5.98    6.00    5.75    5.79         5.79   
    2496        2497  Jul 24, 2012    6.13    6.21    5.92    5.97         5.97   
    2497        2498  Jul 23, 2012    6.21    6.26    6.12    6.13         6.13   
    2498        2499  Jul 20, 2012    6.41    6.45    6.25    6.36         6.36   
    2499        2500  Jul 19, 2012    6.54    6.63    6.41    6.45         6.45   
    
            Volume  
    0     31866500  
    1     34734200  
    2     33702500  
    3     40931000  
    4     30810900  
    ...        ...  
    2495  14211000  
    2496   7501500  
    2497   6934000  
    2498   7842500  
    2499   7179500  
    
    [2500 rows x 8 columns]
    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 2500 entries, 0 to 2499
    Data columns (total 8 columns):
     #   Column       Non-Null Count  Dtype  
    ---  ------       --------------  -----  
     0   Unnamed: 0   2500 non-null   int64  
     1   Date         2500 non-null   object 
     2   Open         2500 non-null   float64
     3   High         2500 non-null   float64
     4   Low          2500 non-null   float64
     5   Close*       2500 non-null   float64
     6   Adj Close**  2500 non-null   float64
     7   Volume       2500 non-null   int64  
    dtypes: float64(5), int64(2), object(1)
    memory usage: 175.8+ KB
    None
          Unnamed: 0          Date   Open   High    Low  Close*  Adj Close**  \
    0              0  Jun 24, 2022  83.56  87.53  83.08   87.08        87.08   
    1              1  Jun 23, 2022  84.32  84.41  80.23   82.43        82.43   
    2              2  Jun 22, 2022  84.40  86.38  83.30   83.75        83.75   
    3              3  Jun 21, 2022  84.17  85.81  82.60   83.79        83.79   
    4              4  Jun 17, 2022  82.19  82.94  79.43   81.57        81.57   
    ...          ...           ...    ...    ...    ...     ...          ...   
    2495        2495  Jul 25, 2012   4.11   4.15   3.98    4.01         4.01   
    2496        2496  Jul 24, 2012   4.18   4.20   4.00    4.06         4.06   
    2497        2497  Jul 23, 2012   4.15   4.16   4.03    4.15         4.15   
    2498        2498  Jul 20, 2012   4.62   4.62   4.20    4.22         4.22   
    2499        2499  Jul 19, 2012   4.93   5.07   4.82    4.86         4.86   
    
             Volume  
    0      88477400  
    1     100614600  
    2      86634700  
    3      87780600  
    4     105125700  
    ...         ...  
    2495   20287800  
    2496   21019600  
    2497   22166600  
    2498   52272400  
    2499   27598400  
    
    [2500 rows x 8 columns]
    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 2500 entries, 0 to 2499
    Data columns (total 8 columns):
     #   Column       Non-Null Count  Dtype  
    ---  ------       --------------  -----  
     0   Unnamed: 0   2500 non-null   int64  
     1   Date         2500 non-null   object 
     2   Open         2500 non-null   float64
     3   High         2500 non-null   float64
     4   Low          2500 non-null   float64
     5   Close*       2500 non-null   float64
     6   Adj Close**  2500 non-null   float64
     7   Volume       2500 non-null   object 
    dtypes: float64(5), int64(1), object(2)
    memory usage: 175.8+ KB
    None
          Unnamed: 0          Date    Open    High     Low  Close*  Adj Close**  \
    0              0  Jun 24, 2022  261.81  267.98  261.72  267.70       267.70   
    1              1  Jun 23, 2022  255.57  259.37  253.63  258.86       258.86   
    2              2  Jun 22, 2022  251.89  257.17  250.37  253.13       253.13   
    3              3  Jun 21, 2022  250.26  254.75  249.51  253.74       253.74   
    4              4  Jun 17, 2022  244.70  250.50  244.03  247.65       247.65   
    ...          ...           ...     ...     ...     ...     ...          ...   
    2495        2535  Jul 25, 2012   29.24   29.33   28.78   28.83        23.54   
    2496        2536  Jul 24, 2012   29.24   29.36   28.90   29.15        23.80   
    2497        2537  Jul 23, 2012   29.57   29.58   29.01   29.28        23.91   
    2498        2538  Jul 20, 2012   31.00   31.05   30.05   30.12        24.59   
    2499        2539  Jul 19, 2012   30.51   30.80   30.38   30.67        25.04   
    
            Volume  
    0     33900700  
    1     25861400  
    2     25939900  
    3     29928300  
    4     42800400  
    ...        ...  
    2495  45579500  
    2496  47723300  
    2497  55151900  
    2498  64021700  
    2499  46663200  
    
    [2500 rows x 8 columns]
    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 2500 entries, 0 to 2499
    Data columns (total 8 columns):
     #   Column       Non-Null Count  Dtype  
    ---  ------       --------------  -----  
     0   Unnamed: 0   2500 non-null   int64  
     1   Date         2500 non-null   object 
     2   Open         2500 non-null   float64
     3   High         2500 non-null   float64
     4   Low          2500 non-null   float64
     5   Close*       2500 non-null   float64
     6   Adj Close**  2500 non-null   float64
     7   Volume       2500 non-null   int64  
    dtypes: float64(5), int64(2), object(1)
    memory usage: 175.8+ KB
    None
          Unnamed: 0          Date   Open   High    Low  Close*  Adj Close**  \
    0              0  Jun 24, 2022  37.85  38.64  37.74   38.61        38.61   
    1              1  Jun 23, 2022  37.61  37.62  36.91   37.41        37.41   
    2              2  Jun 22, 2022  37.33  37.77  37.22   37.38        37.38   
    3              3  Jun 21, 2022  37.36  38.03  37.33   37.73        37.73   
    4              4  Jun 17, 2022  37.48  38.12  36.60   36.97        36.97   
    ...          ...           ...    ...    ...    ...     ...          ...   
    2495        2535  Jul 25, 2012  25.02  25.53  25.00   25.13        18.72   
    2496        2536  Jul 24, 2012  25.19  25.22  24.79   25.01        18.63   
    2497        2537  Jul 23, 2012  25.05  25.36  24.80   25.26        18.82   
    2498        2538  Jul 20, 2012  25.87  26.00  25.50   25.52        19.01   
    2499        2539  Jul 19, 2012  26.35  26.35  25.82   26.06        19.41   
    
            Volume  
    0     38152100  
    1     30163000  
    2     32571000  
    3     34004900  
    4     71235800  
    ...        ...  
    2495  34407600  
    2496  33637600  
    2497  36378200  
    2498  48844000  
    2499  36555800  
    
    [2500 rows x 8 columns]
    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 2500 entries, 0 to 2499
    Data columns (total 8 columns):
     #   Column       Non-Null Count  Dtype  
    ---  ------       --------------  -----  
     0   Unnamed: 0   2500 non-null   int64  
     1   Date         2500 non-null   object 
     2   Open         2500 non-null   float64
     3   High         2500 non-null   float64
     4   Low          2500 non-null   float64
     5   Close*       2500 non-null   float64
     6   Adj Close**  2500 non-null   float64
     7   Volume       2500 non-null   int64  
    dtypes: float64(5), int64(2), object(1)
    memory usage: 175.8+ KB
    None
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>NVDA Open</th>
      <th>NVDA High</th>
      <th>NVDA Low</th>
      <th>NVDA Close</th>
      <th>NVDA Volume</th>
      <th>NVDA Daily change</th>
      <th>NVDA Daily change Max</th>
      <th>AAPL Open</th>
      <th>AAPL High</th>
      <th>...</th>
      <th>MSFT Volume</th>
      <th>MSFT Daily change</th>
      <th>MSFT Daily change Max</th>
      <th>INTC Open</th>
      <th>INTC High</th>
      <th>INTC Low</th>
      <th>INTC Close</th>
      <th>INTC Volume</th>
      <th>INTC Daily change</th>
      <th>INTC Daily change Max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Jun 24, 2022</td>
      <td>165.00</td>
      <td>171.40</td>
      <td>163.10</td>
      <td>171.26</td>
      <td>47166300</td>
      <td>3.793939</td>
      <td>5.088903</td>
      <td>139.90</td>
      <td>141.91</td>
      <td>...</td>
      <td>33900700</td>
      <td>2.249723</td>
      <td>2.391869</td>
      <td>37.85</td>
      <td>38.64</td>
      <td>37.74</td>
      <td>38.61</td>
      <td>38152100</td>
      <td>2.007926</td>
      <td>2.384738</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jun 23, 2022</td>
      <td>165.19</td>
      <td>165.85</td>
      <td>158.53</td>
      <td>162.25</td>
      <td>46368000</td>
      <td>-1.779769</td>
      <td>4.617423</td>
      <td>136.82</td>
      <td>138.59</td>
      <td>...</td>
      <td>25861400</td>
      <td>1.287319</td>
      <td>2.263139</td>
      <td>37.61</td>
      <td>37.62</td>
      <td>36.91</td>
      <td>37.41</td>
      <td>30163000</td>
      <td>-0.531773</td>
      <td>1.923598</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Jun 22, 2022</td>
      <td>162.26</td>
      <td>166.62</td>
      <td>161.80</td>
      <td>163.60</td>
      <td>43713500</td>
      <td>0.825835</td>
      <td>2.978986</td>
      <td>134.79</td>
      <td>137.76</td>
      <td>...</td>
      <td>25939900</td>
      <td>0.492278</td>
      <td>2.715980</td>
      <td>37.33</td>
      <td>37.77</td>
      <td>37.22</td>
      <td>37.38</td>
      <td>32571000</td>
      <td>0.133941</td>
      <td>1.477700</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Jun 21, 2022</td>
      <td>164.75</td>
      <td>170.08</td>
      <td>164.07</td>
      <td>165.66</td>
      <td>48308900</td>
      <td>0.552352</td>
      <td>3.663071</td>
      <td>133.42</td>
      <td>137.06</td>
      <td>...</td>
      <td>29928300</td>
      <td>1.390554</td>
      <td>2.100116</td>
      <td>37.36</td>
      <td>38.03</td>
      <td>37.33</td>
      <td>37.73</td>
      <td>34004900</td>
      <td>0.990364</td>
      <td>1.875167</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Jun 17, 2022</td>
      <td>156.48</td>
      <td>159.95</td>
      <td>153.28</td>
      <td>158.80</td>
      <td>62905700</td>
      <td>1.482618</td>
      <td>4.351514</td>
      <td>130.07</td>
      <td>133.08</td>
      <td>...</td>
      <td>42800400</td>
      <td>1.205558</td>
      <td>2.651313</td>
      <td>37.48</td>
      <td>38.12</td>
      <td>36.60</td>
      <td>36.97</td>
      <td>71235800</td>
      <td>-1.360726</td>
      <td>4.153005</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2495</th>
      <td>Jul 25, 2012</td>
      <td>3.20</td>
      <td>3.31</td>
      <td>3.18</td>
      <td>3.27</td>
      <td>41504400</td>
      <td>2.187500</td>
      <td>4.088050</td>
      <td>20.52</td>
      <td>20.74</td>
      <td>...</td>
      <td>45579500</td>
      <td>-1.402189</td>
      <td>1.911049</td>
      <td>25.02</td>
      <td>25.53</td>
      <td>25.00</td>
      <td>25.13</td>
      <td>34407600</td>
      <td>0.439648</td>
      <td>2.120000</td>
    </tr>
    <tr>
      <th>2496</th>
      <td>Jul 24, 2012</td>
      <td>3.25</td>
      <td>3.27</td>
      <td>3.17</td>
      <td>3.21</td>
      <td>35116400</td>
      <td>-1.230769</td>
      <td>3.154574</td>
      <td>21.69</td>
      <td>21.77</td>
      <td>...</td>
      <td>47723300</td>
      <td>-0.307798</td>
      <td>1.591696</td>
      <td>25.19</td>
      <td>25.22</td>
      <td>24.79</td>
      <td>25.01</td>
      <td>33637600</td>
      <td>-0.714569</td>
      <td>1.734570</td>
    </tr>
    <tr>
      <th>2497</th>
      <td>Jul 23, 2012</td>
      <td>3.12</td>
      <td>3.27</td>
      <td>3.08</td>
      <td>3.24</td>
      <td>45158400</td>
      <td>3.846154</td>
      <td>6.168831</td>
      <td>21.23</td>
      <td>21.64</td>
      <td>...</td>
      <td>55151900</td>
      <td>-0.980724</td>
      <td>1.964840</td>
      <td>25.05</td>
      <td>25.36</td>
      <td>24.80</td>
      <td>25.26</td>
      <td>36378200</td>
      <td>0.838323</td>
      <td>2.258065</td>
    </tr>
    <tr>
      <th>2498</th>
      <td>Jul 20, 2012</td>
      <td>3.28</td>
      <td>3.30</td>
      <td>3.17</td>
      <td>3.20</td>
      <td>45100000</td>
      <td>-2.439024</td>
      <td>4.100946</td>
      <td>21.89</td>
      <td>21.94</td>
      <td>...</td>
      <td>64021700</td>
      <td>-2.838710</td>
      <td>3.327787</td>
      <td>25.87</td>
      <td>26.00</td>
      <td>25.50</td>
      <td>25.52</td>
      <td>48844000</td>
      <td>-1.352918</td>
      <td>1.960784</td>
    </tr>
    <tr>
      <th>2499</th>
      <td>Jul 19, 2012</td>
      <td>3.29</td>
      <td>3.34</td>
      <td>3.27</td>
      <td>3.30</td>
      <td>40303600</td>
      <td>0.303951</td>
      <td>2.140673</td>
      <td>21.83</td>
      <td>21.98</td>
      <td>...</td>
      <td>46663200</td>
      <td>0.524418</td>
      <td>1.382488</td>
      <td>26.35</td>
      <td>26.35</td>
      <td>25.82</td>
      <td>26.06</td>
      <td>36555800</td>
      <td>-1.100569</td>
      <td>2.052672</td>
    </tr>
  </tbody>
</table>
<p>2500 rows × 43 columns</p>
</div>




```python
main_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 2500 entries, 0 to 2499
    Data columns (total 43 columns):
     #   Column                 Non-Null Count  Dtype  
    ---  ------                 --------------  -----  
     0   Date                   2500 non-null   object 
     1   NVDA Open              2500 non-null   float64
     2   NVDA High              2500 non-null   float64
     3   NVDA Low               2500 non-null   float64
     4   NVDA Close             2500 non-null   float64
     5   NVDA Volume            2500 non-null   int64  
     6   NVDA Daily change      2500 non-null   float64
     7   NVDA Daily change Max  2500 non-null   float64
     8   AAPL Open              2500 non-null   float64
     9   AAPL High              2500 non-null   float64
     10  AAPL Low               2500 non-null   float64
     11  AAPL Close             2500 non-null   float64
     12  AAPL Volume            2500 non-null   int64  
     13  AAPL Daily change      2500 non-null   float64
     14  AAPL Daily change Max  2500 non-null   float64
     15  TSLA Open              2500 non-null   float64
     16  TSLA High              2500 non-null   float64
     17  TSLA Low               2500 non-null   float64
     18  TSLA Close             2500 non-null   float64
     19  TSLA Volume            2500 non-null   int64  
     20  TSLA Daily change      2500 non-null   float64
     21  TSLA Daily change Max  2500 non-null   float64
     22  AMD Open               2500 non-null   float64
     23  AMD High               2500 non-null   float64
     24  AMD Low                2500 non-null   float64
     25  AMD Close              2500 non-null   float64
     26  AMD Volume             2500 non-null   object 
     27  AMD Daily change       2500 non-null   float64
     28  AMD Daily change Max   2500 non-null   float64
     29  MSFT Open              2500 non-null   float64
     30  MSFT High              2500 non-null   float64
     31  MSFT Low               2500 non-null   float64
     32  MSFT Close             2500 non-null   float64
     33  MSFT Volume            2500 non-null   int64  
     34  MSFT Daily change      2500 non-null   float64
     35  MSFT Daily change Max  2500 non-null   float64
     36  INTC Open              2500 non-null   float64
     37  INTC High              2500 non-null   float64
     38  INTC Low               2500 non-null   float64
     39  INTC Close             2500 non-null   float64
     40  INTC Volume            2500 non-null   int64  
     41  INTC Daily change      2500 non-null   float64
     42  INTC Daily change Max  2500 non-null   float64
    dtypes: float64(36), int64(5), object(2)
    memory usage: 859.4+ KB
    


```python
indexes = ["NVDA","AAPL","TSLA","AMD","MSFT","INTC"]

for index in indexes:
      
    main_df[index + " " +'Daily change'] = (((main_df[index + " " +'Close']).astype(float))-((main_df[index + " " +'Open']).astype(float)))/((main_df[index + " " +'Open']).astype(float))*100
    main_df[index + " " +'Daily change Max'] =  abs((main_df[index + " " +'High'].astype(float)-main_df[index + " " +'Low'].astype(float))/main_df[index + " " +'Low'].astype(float)*100)


write_to_file(main_df, ("main_dataframe" + ".csv"))

```


```python
main_df.tail()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>NVDA Open</th>
      <th>NVDA High</th>
      <th>NVDA Low</th>
      <th>NVDA Close</th>
      <th>NVDA Volume</th>
      <th>NVDA Daily change</th>
      <th>NVDA Daily change Max</th>
      <th>AAPL Open</th>
      <th>AAPL High</th>
      <th>...</th>
      <th>MSFT Volume</th>
      <th>MSFT Daily change</th>
      <th>MSFT Daily change Max</th>
      <th>INTC Open</th>
      <th>INTC High</th>
      <th>INTC Low</th>
      <th>INTC Close</th>
      <th>INTC Volume</th>
      <th>INTC Daily change</th>
      <th>INTC Daily change Max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2495</th>
      <td>Jul 25, 2012</td>
      <td>3.20</td>
      <td>3.31</td>
      <td>3.18</td>
      <td>3.27</td>
      <td>41504400</td>
      <td>2.187500</td>
      <td>4.088050</td>
      <td>20.52</td>
      <td>20.74</td>
      <td>...</td>
      <td>45579500</td>
      <td>-1.402189</td>
      <td>1.911049</td>
      <td>25.02</td>
      <td>25.53</td>
      <td>25.00</td>
      <td>25.13</td>
      <td>34407600</td>
      <td>0.439648</td>
      <td>2.120000</td>
    </tr>
    <tr>
      <th>2496</th>
      <td>Jul 24, 2012</td>
      <td>3.25</td>
      <td>3.27</td>
      <td>3.17</td>
      <td>3.21</td>
      <td>35116400</td>
      <td>-1.230769</td>
      <td>3.154574</td>
      <td>21.69</td>
      <td>21.77</td>
      <td>...</td>
      <td>47723300</td>
      <td>-0.307798</td>
      <td>1.591696</td>
      <td>25.19</td>
      <td>25.22</td>
      <td>24.79</td>
      <td>25.01</td>
      <td>33637600</td>
      <td>-0.714569</td>
      <td>1.734570</td>
    </tr>
    <tr>
      <th>2497</th>
      <td>Jul 23, 2012</td>
      <td>3.12</td>
      <td>3.27</td>
      <td>3.08</td>
      <td>3.24</td>
      <td>45158400</td>
      <td>3.846154</td>
      <td>6.168831</td>
      <td>21.23</td>
      <td>21.64</td>
      <td>...</td>
      <td>55151900</td>
      <td>-0.980724</td>
      <td>1.964840</td>
      <td>25.05</td>
      <td>25.36</td>
      <td>24.80</td>
      <td>25.26</td>
      <td>36378200</td>
      <td>0.838323</td>
      <td>2.258065</td>
    </tr>
    <tr>
      <th>2498</th>
      <td>Jul 20, 2012</td>
      <td>3.28</td>
      <td>3.30</td>
      <td>3.17</td>
      <td>3.20</td>
      <td>45100000</td>
      <td>-2.439024</td>
      <td>4.100946</td>
      <td>21.89</td>
      <td>21.94</td>
      <td>...</td>
      <td>64021700</td>
      <td>-2.838710</td>
      <td>3.327787</td>
      <td>25.87</td>
      <td>26.00</td>
      <td>25.50</td>
      <td>25.52</td>
      <td>48844000</td>
      <td>-1.352918</td>
      <td>1.960784</td>
    </tr>
    <tr>
      <th>2499</th>
      <td>Jul 19, 2012</td>
      <td>3.29</td>
      <td>3.34</td>
      <td>3.27</td>
      <td>3.30</td>
      <td>40303600</td>
      <td>0.303951</td>
      <td>2.140673</td>
      <td>21.83</td>
      <td>21.98</td>
      <td>...</td>
      <td>46663200</td>
      <td>0.524418</td>
      <td>1.382488</td>
      <td>26.35</td>
      <td>26.35</td>
      <td>25.82</td>
      <td>26.06</td>
      <td>36555800</td>
      <td>-1.100569</td>
      <td>2.052672</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 43 columns</p>
</div>




```python

```


```python

```
