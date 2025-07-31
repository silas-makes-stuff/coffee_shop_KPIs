# coffee_shop_KPIs

OBTAINED DATA FROM https://www.kaggle.com/datasets/reignrichard/coffee-store-sales/data 

DATA CLEANING

Checked for blanks within each column. 

=COUNTBLANK(E2:E3637) - (L2:L3637)
Column E (card) has 89 blanks

Verify that all blanks coorespond with "cash"
=COUNTIF(D:D,  "cash") - 89 blanks

Cash transactions match number of blanks "card" column. 

Datetime column 
=FILTER(B2:B3637, COUNTIF(B2:B3637, B2:B3637)>1, "No Duplicates") 
No duplicates returned

Converted South African Rand to USD

XE.com conversion 0.056353407 US Dollar
=F2-F3637 * 0.056352149

EXPLORATORY DATA ANALYSIS USING EXCEL
Sales trends over time
Most popular drinks
Avg revenue by day of the week
Peak time of day/day of week

Sales over Time
<img width="989" height="676" alt="image" src="https://github.com/user-attachments/assets/1f0d0107-0e82-44ec-a204-9d6dac635e5d" />

Most popular Drinks
<img width="668" height="527" alt="image" src="https://github.com/user-attachments/assets/0aaeadc2-c31a-46a7-98a1-859aca74a133" />

Avg Revenue By Day of the Week
<img width="819" height="452" alt="image" src="https://github.com/user-attachments/assets/42ff482d-151a-4775-9f89-df396f30b037" />

Peak Time, Day of Week
<img width="796" height="611" alt="image" src="https://github.com/user-attachments/assets/9a0fbd53-5338-4f67-9f96-8d2f64d80918" />

Revenue by Time of Day
<img width="804" height="1129" alt="image" src="https://github.com/user-attachments/assets/ae793c03-158a-4486-bb57-aaf3d89c4706" />

NOW LET'S DO SOME PYTHON

Set up Data Frame

```
# import necessary libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score
from sklearn.linear_model import LinearRegression
from sklearn.ensemble import RandomForestRegressor
import warnings

#silence warnings
warnings.filterwarnings("ignore")

#set up data frame
url = "https://raw.githubusercontent.com/silas-makes-stuff/coffee_shop_KPIs/main/coffee_sales_clean.csv"
df = pd.read_csv(url)

#drop extra columns 
df = df.loc[:, ~df.columns.str.contains('^Unnamed')]

#show first few lines of data
df.head()
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>date</th>
      <th>datetime</th>
      <th>hour_of_day</th>
      <th>cash_type</th>
      <th>card</th>
      <th>money</th>
      <th>coffee_name</th>
      <th>Time_of_Day</th>
      <th>Weekday</th>
      <th>Month_name</th>
      <th>Weekdaysort</th>
      <th>Monthsort</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>3/1/2024</td>
      <td>3/1/24 10:15</td>
      <td>10</td>
      <td>card</td>
      <td>ANON-0000-0000-0001</td>
      <td>$2.18</td>
      <td>Latte</td>
      <td>Morning</td>
      <td>Fri</td>
      <td>Mar</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3/1/2024</td>
      <td>3/1/24 12:19</td>
      <td>12</td>
      <td>card</td>
      <td>ANON-0000-0000-0002</td>
      <td>$2.18</td>
      <td>Hot Chocolate</td>
      <td>Afternoon</td>
      <td>Fri</td>
      <td>Mar</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3/1/2024</td>
      <td>3/1/24 12:20</td>
      <td>12</td>
      <td>card</td>
      <td>ANON-0000-0000-0002</td>
      <td>$2.18</td>
      <td>Hot Chocolate</td>
      <td>Afternoon</td>
      <td>Fri</td>
      <td>Mar</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3/1/2024</td>
      <td>3/1/24 13:46</td>
      <td>13</td>
      <td>card</td>
      <td>ANON-0000-0000-0003</td>
      <td>$1.63</td>
      <td>Americano</td>
      <td>Afternoon</td>
      <td>Fri</td>
      <td>Mar</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>3/1/2024</td>
      <td>3/1/24 13:48</td>
      <td>13</td>
      <td>card</td>
      <td>ANON-0000-0000-0004</td>
      <td>$2.18</td>
      <td>Latte</td>
      <td>Afternoon</td>
      <td>Fri</td>
      <td>Mar</td>
      <td>5</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>

