# coffee_shop_KPIs


```
#Set up Data Frame
# import necessary libraries

import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
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

Now we've seen the distribution of individual purchases. Let's look at sales over time. 

```
#sum sales by day
daily_total = df.groupby('date')['money'].sum().reset_index()
sns.set_style("whitegrid")
sns.lineplot(x='date', y='money', data=daily_total) 
plt.title('Total Sales Over Time')
plt.xlabel('Date')
plt.ylabel('Daily Sales (money)')
plt.tight_layout()
plt.show()
```
<Figure size 640x480 with 1 Axes><img width="630" height="470" alt="image" src="https://github.com/user-attachments/assets/e0ce3e9e-3278-451a-be65-8d8ccf09ebb0" />

Next, let's look at most popular drinks. This will help with inventory, inform any specials, etc. 

```
#most popular drinks

#count drink totals
count_drinks = df['coffee_name'].value_counts()

#pre-set data to be in descending order
df['coffee_name'] = pd.Categorical(df['coffee_name'], categories=count_drinks.index, ordered=True)

sns.histplot(data=df, x='coffee_name', color = 'blue')
plt.title('Most Popular Drinks')
plt.xlabel('Coffee Name')
plt.ylabel('Frequency')
plt.grid(True)
plt.tight_layout()
plt.xticks(rotation=80, fontsize=8)
plt.show()
```
<Figure size 640x480 with 1 Axes><img width="630" height="569" alt="image" src="https://github.com/user-attachments/assets/afd6a7c7-6d7a-4b1e-b387-468ab9725596" />

Average Revenue by Hour helps with staffing/scheduling as well, and considers any events or specials which could run when the shop is slow.

```
df_filtered['date'] = df_filtered['datetime'].dt.date
sales_hour_day = df_filtered.groupby(['date', 'hour_of_day'])['money'].sum().reset_index()
avg_sales_hour = sales_hour_day.groupby('hour_of_day')['money'].mean().reset_index()

sns.barplot(x='hour_of_day', y='money', data=avg_sales_hour, ci=None, color= 'skyblue')
plt.title('Avg Revenue By Hour')
plt.xlabel('Hour of Day')
plt.ylabel('Avg Sales (USD)')
plt.tight_layout()
plt.show()
```

<Figure size 640x480 with 1 Axes><img width="630" height="470" alt="image" src="https://github.com/user-attachments/assets/117666f4-592a-4115-b6f6-ab63ab13db17" />

Correlations of numeric variables

```
df_numeric = df.select_dtypes(include=[float, int])
correlation_matrix = df_numeric.corr()

print(correlation_matrix)
```
hour_of_day     money  Weekdaysort  Monthsort      year  \
hour_of_day     1.000000  0.100106    -0.005423   0.012870 -0.045488   
money           0.100106  1.000000     0.018617  -0.042064  0.006439   
Weekdaysort    -0.005423  0.018617     1.000000   0.041689 -0.067836   
Monthsort       0.012870 -0.042064     0.041689   1.000000 -0.725960   
year           -0.045488  0.006439    -0.067836  -0.725960  1.000000   
month           0.012870 -0.042064     0.041689   1.000000 -0.725960   
day             0.015518  0.002162    -0.030948   0.028615 -0.080394   
weekday        -0.005423  0.018617     1.000000   0.041689 -0.067836   

                month       day   weekday  
hour_of_day  0.012870  0.015518 -0.005423  
money       -0.042064  0.002162  0.018617  
Weekdaysort  0.041689 -0.030948  1.000000  
Monthsort    1.000000  0.028615  0.041689  
year        -0.725960 -0.080394 -0.067836  
month        1.000000  0.028615  0.041689  
day          0.028615  1.000000 -0.030948  
weekday      0.041689 -0.030948  1.000000


Scatterplot reflecting coorelation between Hour of Day and Money 

```
sns.regplot(x='hour_of_day', y='money', data=df_filtered, scatter=True, color = 'skyblue', line_kws={'color': 'blue'})
plt.title('Correlation Between Hour of Day and Money')
plt.xlabel('Hour of Day')
plt.ylabel('Money')
plt.grid(True)
plt.xticks(range(6, 22))
plt.tight_layout()
plt.show()
```
<Figure size 640x480 with 1 Axes><img width="630" height="470" alt="image" src="https://github.com/user-attachments/assets/8426f3ee-eb95-4533-ae27-fba712ddd261" />

Heat map avg money by day of week/hour

```
pivot_table = df_filtered.pivot_table(values='money', index='weekday', columns='hour_of_day', aggfunc='mean')
plt.figure(figsize=(14, 6))
sns.heatmap(pivot_table, cmap='YlGnBu', linewidths=0.5, annot=True, fmt='.1f')
plt.title('Average Money Spent by Day of Week & Hour')
plt.xlabel('Hour of Day')
plt.ylabel('Day of Week')
plt.tight_layout()
plt.show()
```

<Figure size 1400x600 with 2 Axes><img width="1256" height="590" alt="image" src="https://github.com/user-attachments/assets/fe97ac27-858e-4311-aba2-8e91a8186b45" />

Heat map avg money, month and day of week

```
pivot_table = df_filtered.pivot_table(values='money', index='month_name', columns='weekday', aggfunc='mean')
plt.figure(figsize=(14, 6))
sns.heatmap(pivot_table, cmap='YlGnBu', linewidths=0.5, annot=True, fmt='.1f')
plt.title('Average Money Spent by Month & Day of Week')
plt.xlabel('Day of Week')
plt.ylabel('Month')
plt.tight_layout()
plt.show()
```

<Figure size 1400x600 with 2 Axes><img width="1257" height="590" alt="image" src="https://github.com/user-attachments/assets/580ae2bd-831c-4b01-b48d-16c700c44c40" />
