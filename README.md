# coffee_shop_KPIs
 
## **Goal**
Clean, transform, analyze and visualize coffee shop data for Key Performance Indicators.

OBTAINED DATA FROM https://www.kaggle.com/datasets/reignrichard/coffee-store-sales/data 

Examined
1. Total sales over time
2. Relationship between sales and hour of day, day of week
3. Distribution of average transcation
4. Most popular products

## Description
Dataset captures sales transcations from a coffee shop in Cape Town, South Africa. Included is timestamp (day, month, year, time), payment type (cash or card), customer id, product name, and revenue per transaction (ZAR). Purpose of project is to explore sales data, time series, business performance, practice data cleaning, KPI design and data cleaning.

## Skills
Data cleaning, exploratory data analysis (EDA), data manipulation, data visualization

## Technology
Excel, Python, Copilot (dashboard)

# Cleaning, Refining, EDA

## Excel 
Used conditional formatting to check for blanks, then COUNTBLANK to confirm blank cells under 'money' matched 'cash.' Used FILTER, COUNTIF to check for duplicates. Converted ZAR to USD XE.com conversion 0.056353407 US Dollar =F2-F3637 * 0.056352149. Converted Datetime from string to date/time formatting. From here, I converted to table and conducted exploratory data analysis using PivotTables. 

- Line graph - Sales over time
- Histogram - 7 most ordered drinks
- Bar graph - Avg Revenue by day of week, then by weekday and time of day
- Boxplot - Avg Revenue by Morning, Afternoon, Night

## Python
Imported necessary libraries and set up data frame. 

- Checked that null values showed 89 blanks under card and string cash also showed 89.
```
print(df.isnull().sum())
df['cash_type'].str.count('cash').sum()
```
- Converted necessary variables into categorical vs. numerical, removed strings.
- Removed dollar sign in money column
- Removed outliers using Boolean
- sns.boxplot stats for Avg Revenue per Transaction
- sns.lineplot for Total Sales Over Time
- sns.histplot for most popular products
- sns.barplot for avg revenue per hour
- calculated correlations df_numeric.corr()
- sns.regplot() money by hour of day
- sns.heatmap() money by day of week/hour
- sns.heatmap() money by month, day of week

## Conclusions and Recommendations

- Average individual transaction is $1.85 and revenue per hour peaks at 9pm. Tuesdays are the most profitible day overall, returning the most in the morning and evenings, with Saturday returning more in the afternoons.
- There is a positive coorelation between hour of day and average money spent, with a peak at 7pm on Wednesdays.
- Customers spend the least amount of money per transaction in August, July, and September. 
- Most popular drink is a Americano with Milk followed closely by Latte.

  # Dashboard
 ## Dashboard
  https://super-duper-train-v6q7pr5555592jr5-8501.app.github.dev/ 

## Dashboard Code
  https://github.com/silas-makes-stuff/coffee_shop_KPIs/blob/main/dashboard%20code 

## Investigate 
- Higher average transactions later in the day. Are customers buying more than one coffee (there with a friend) or buying a pastry with their coffee? This could help inform specials or products to push around this time.
- Most popular drinks on Saturday afternoons, consider pushing a new menu item or pastry to maximize sales.
- How to increase traffic and sales from 8-11am. If people are at work during this time, consider advertising free WiFi or a discount to come work in the shop.
- Pricing of most popular drinks during August, July, September (colder and rainier months). Sales on Hot Chocolate and Cocoa are low overall, maybe pushing for shots of flavors in these drinks during cold months would increase average sales. 
