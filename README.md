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
