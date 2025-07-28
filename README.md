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
