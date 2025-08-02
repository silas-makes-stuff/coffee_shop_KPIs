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

CLEANING

```
#double check null values, shows 89 blanks under card
print(df.isnull().sum())

#match null values to count of string cash in column cash type
df['cash_type'].str.count('cash').sum()
```

date            0
datetime        0
hour_of_day     0
cash_type       0
card           89
money           0
coffee_name     0
Time_of_Day     0
Weekday         0
Month_name      0
Weekdaysort     0
Monthsort       0
dtype: int64

np.int64(89)

Blanks under Card match total number of string 'cash,' so we are good to go. Time to do some conversions and set some data types.

```
#convert dates to datetime
df['date'] = pd.to_datetime(df['date'])
df['datetime'] = pd.to_datetime(df['datetime'])

#drop failed rows and reset index
df = df.dropna(subset=['date', 'datetime'])
df = df.reset_index(drop=True)

# cleaning, removes strings
cat_cols= ['cash_type', 'card', 'coffee_name', 'Time_of_Day', 'Weekday', 'Month_name' ]
for col in cat_cols:
    df[col] = df[col].astype(str).str.strip()
num_cols= ['date','datetime','money','hour_of_day', 'Weekdaysort','Monthsort']
print("Categorical Variables: ")
print(cat_cols)
print("Numberical Variables:")
print(num_cols)
```
Categorical Variables: 
['cash_type', 'card', 'coffee_name', 'Time_of_Day', 'Weekday', 'Month_name']
Numberical Variables:
['date', 'datetime', 'money', 'hour_of_day', 'Weekdaysort', 'Monthsort']

```
#extracting year, mo, hour
df['year'] = df['date'].dt.year
df['month'] = df['date'].dt.month
df['day'] = df['date'].dt.day
df['weekday'] = df['date'].dt.weekday

df.head()
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
      <th>year</th>
      <th>month</th>
      <th>day</th>
      <th>weekday</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2024-03-01</td>
      <td>2024-03-01 10:15:00</td>
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
      <td>2024</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2024-03-01</td>
      <td>2024-03-01 12:19:00</td>
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
      <td>2024</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2024-03-01</td>
      <td>2024-03-01 12:20:00</td>
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
      <td>2024</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2024-03-01</td>
      <td>2024-03-01 13:46:00</td>
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
      <td>2024</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2024-03-01</td>
      <td>2024-03-01 13:48:00</td>
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
      <td>2024</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>

EXPLORATORY DATA ANALYSIS
Initial boxplot was innacurate do to 'money' outlier 

```
#convert money to remove dollar signs so that the program doesn't crash out
df['money'] = df['money'].replace('[\$,]', '', regex=True).astype(float)

#remove outlier
df = df[df['money'] != df['money'].max()]

#calculate boxplot stats 
q1, median, q3 = np.percentile(df['money'], [25, 50, 75])
iqr = q3 - q1
lower_whisker = max(df['money'].min(), q1 - 1.5 * iqr)
upper_whisker = min(df['money'].max(), q3 + 1.5 * iqr)

print(lower_whisker, q1, median, q3, upper_whisker)
```

Whoops! After a frustrating series of events, we've discovered that we are removing the maximum value EVERY TIME, changing the calculation.  

Let's back up. We can't just drop the max value, because when we run it again, it just keeps chopping off the top and switching up our data. Let's try something else. To the previous cell, I filtered the dataset using a Boolean. 

```
#convert money to remove dollar signs so that the program doesn't crash out
df['money'] = df['money'].replace('[\$,]', '', regex=True).astype(float)

# save orginal data
df_og = df.copy()

print(df_og['money'].max())

column_check = 'money'
number_drop = 35.76

df_filtered = df[df[column_check] != number_drop]

print(df_filtered['money'].max())
```
35.76
2.25

Beautiful. Now the filtered dataset has dropped that outlier. Now let's run the modified calculations.

```
#calculate boxplot stats 
q1, median, q3 = np.percentile(df_filtered['money'], [25, 50, 75])
iqr = q3 - q1
lower_whisker = max(df_filtered['money'].min(), q1 - 1.5 * iqr)
upper_whisker = min(df_filtered['money'].max(), q3 + 1.5 * iqr)

print(lower_whisker, q1, median, q3, upper_whisker)
```
1.02 1.57 1.85 2.02 2.25

I am certain there must be a way to calculate the above values within the code for the boxplot (below). However, using sns I couldn't find a way to do so that accurately reflected the x-tick positions. In previous iterations I'd end up with an x-axis that was inaccurate in positioning, or numbers which overlapped chaotically, or numbers which were accurate in position but incorrect in calculation. So, after several tries, I landed on doing the calculations mannually and using custom labels for the boxplot. 

```
sns.set_style("whitegrid")
sns.boxplot(x=df_filtered["money"]).set(title = 'Distribution of Sales')
plt.xticks(
    ticks=[lower_whisker, q1, median, q3, upper_whisker],
    labels=["$1.02", "$1.57", "$1.85", "$2.02", "$2.25"]
)

plt.show()
```

<Figure size 640x480 with 1 Axes><img width="515" height="455" alt="image" src="https://github.com/user-attachments/assets/00f24955-fdf0-4a92-b0cc-559a87783f91" />


