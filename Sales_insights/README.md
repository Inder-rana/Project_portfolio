# Sales Insights for AtliQ hardware: Project Overview
### [Power BI | Tableau | SQL]
*  Designed a Power BI and Tableau dashboard to understand AtliQ hardware goods sales trends.
*  The final dashboard was effective at displaying the sales trend of AtliQ hardware, allowing users to understand the data and make informed decisions.
*  This dashboard could help in increasing the revenue in the next quarter. 

## Resources and References used
**Project Tutorial:** https://www.youtube.com/playlist?list=PLeo1K3hjS3uva8pk1FI3iK9kCOKQdz1I9  
**GitHub link:** https://github.com/codebasics/DataAnalysisProjects/tree/master/1_SalesInsights

## Data import and SQL

First I imported `Sales_mod_db.sql` in MySQL Server using MySQL Workbench, and did some Data Analysis and data exploration using SQL, below are the queries that I have used and I have also exported all the tables of the Sales database from MySQL into the CSV files with the folder name `Sales database CSV`, if you want to check it out. 

**Comment: showing all the records from each table**. 

`SELECT * FROM sales_mod.customers;`   
`SELECT * FROM sales_mod.date;`    
`SELECT * FROM sales_mod.markets;`    
`SELECT * FROM sales_mod.products;`     
`SELECT * FROM sales_mod.transactions;`   


**Comment: showing the total count of the records from each table.**

`SELECT count(*) FROM sales_mod.customers;`  
`SELECT count(*) FROM sales_mod.date;`    
`SELECT count(*) FROM sales_mod.markets;`  
`SELECT count(*) FROM sales_mod.products;`    
`SELECT count(*) FROM sales_mod.transactions;`    


**Comment: show all the transaction records from the Chennai region(Mark001), then give the count of total transaction records from the Chennai region, and last give the count of all transactions records from the Delhi NCR region(Mark004).**  

`SELECT * FROM sales_mod.transactions WHERE market_code="Mark001";`  
`SELECT count(*) FROM sales_mod.transactions WHERE market_code="Mark001"`   
`SELECT count(*) FROM sales_mod.transactions where market_code="Mark004";`    


**Comment: showing the transactions records for only USD currency.** 

`SELECT * FROM sales_mod.transactions WHERE currency="USD";`


**Comment: creating the inner join between transactions and date table, then showing the sum of total sales amount in 2020 in Chennai region(Mark001).**

`SELECT sales_mod.transactions.*, sales_mod.date.* from sales_mod.transactions inner join sales_mod.date on sales_mod.transactions.order_date=sales_mod.date.date;`

`SELECT sales_mod.transactions.*, sales_mod.date.* FROM sales_mod.transactions INNER JOIN sales_mod.date ON sales_mod.date.date=sales_mod.transactions.order_date where  sales_mod.date.year=2020;`

`SELECT sum(sales_mod.transactions.sales_amount) FROM sales_mod.transactions INNER JOIN sales_mod.date ON sales_mod.date.date=sales_mod.transactions.order_date where    sales_mod.date.year=2020;`

`SELECT sum(sales_mod.transactions.sales_amount) FROM sales_mod.transactions INNER JOIN sales_mod.date ON sales_mod.date.date=sales_mod.transactions.order_date where   sales_mod.date.year=2020 and sales_mod.transactions.market_code="Mark001";`  


**Comment: creating the inner join between transactions and date table, then showing the sum of total sales amount in 2019 in the Delhi NCR region(Mark004).**

`SELECT sales_mod.transactions.*, sales_mod.date.* from sales_mod.transactions inner join sales_mod.date on sales_mod.transactions.order_date=sales_mod.date.date;`

`SELECT sales_mod.transactions.*, sales_mod.date.* FROM sales_mod.transactions INNER JOIN sales_mod.date ON sales_mod.date.date=sales_mod.transactions.order_date where sales_mod.date.year=2019;`

`SELECT sum(sales_mod.transactions.sales_amount) FROM sales_mod.transactions INNER JOIN sales_mod.date ON sales_mod.date.date=sales_mod.transactions.order_date where sales_mod.date.year=2019;`

`SELECT sum(sales_mod.transactions.sales_amount) FROM sales_mod.transactions INNER JOIN sales_mod.date ON sales_mod.date.date=sales_mod.transactions.order_date where sales_mod.date.year=2019 and sales_mod.transactions.market_code="Mark004";`



**Comment: showing the unique products that are sold in Chennai Region(Mar001) and Delhi NCR Region(Mark004).** 

`select distinct product_code from sales_mod.transactions where market_code="Mark001";`   
`select distinct product_code from sales_mod.transactions where market_code="Mark004";`


**comment: showing unique currencies have been used.** 

`select distinct (transactions.currency) from transactions;`   
`select count(*) from transactions where transactions.currency='INR\r';`  

**comment: by selecting 'INR\r' or 'USD\r', we are excluding any duplicate records in the transactions table.** 

`select sum(sales_amount) from transactions inner join date on transactions.order_date = date.date where date.year=2020 and transactions.currency ='INR\r' or transactions.currency ='USD\r';`

`select sum(sales_amount) from transactions inner join date on transactions.order_date = date.date where date.year =2020 and date.month_name = 'January' and (transactions.currency ='INR\r' or transactions.currency ='USD\r');`
    
`select sum(sales_amount) from transactions inner join date on transactions.order_date = date.date where date.year =2020 and transactions.market_code="Mark001" and (transactions.currency ='INR\r' or transactions.currency ='USD\r');`

`select sum(sales_amount) from transactions inner join date on transactions.order_date = date.date where date.year =2017 and date.month_name="November" and transactions.market_code="Mark004" and (transactions.currency ='INR\r' or transactions.currency ='USD\r');`    


## Data Modeling, Data Cleaning, and ETL

After importing the data and exploring the data in MySQL, then I imported  MySQL sales database into PowerBI desktop by connecting to the MySQL server using username and password, once sales database was connected, I was able to see all the tables in the database, I started with Data Modeling by connecting relevant tables with each other to create a relationship between them using STAR schema by identifying the fact table and dimension tables and then joining them with their relevant columns, once Data Modeling was done, then I have done some data cleaning/ETL in the Power Query editor by excluding rows with null values in the markets table, excluding any values below 1 in the sales_amount column in the transactions table, excluding duplicate transactions records in both currencies (INR and USD) in the transactions table and lastly I have created an extra column name `Norm_sales_amount` in the Transactions table to normalize all the different currencies amount into INR to make analysis process easier, I converted all the USD values into INR by multiplying them by 75 INR. I have done a similar process mentioned above in Tableau as well, just to create an alternative version. 

**Data Modelling**
![](https://github.com/Inder-rana/Project_portfolio/blob/main/Sales_insights/images/Powerbi_data_modelling.PNG)


## Data visualization in PowerBI and Tableau
   
Created data visualization based on: Revenue, Sales Qty, Revenue by Markets, Sales Qty by Markets, Month, Year, Revenue trend by Month-Year, Top 5 Customers, and Top 5 Products. To check the dashboard in more detail open the `Power_BI_dashboard.pbix` and `Tableau_dashboard.twbx` files in their relevant environment. 


**PowerBI Dashboard**
![PowerBI Dashboard](https://github.com/Inder-rana/Project_portfolio/blob/main/Sales_insights/images/BI_snapshot_large.png)


**Tableau Dashboard** [Tableau dashboard link](https://public.tableau.com/app/profile/inderjeet.singh4420/viz/SalesInsightsDashboard_16796303589230/Dashboard1)
![](https://github.com/Inder-rana/Project_portfolio/blob/main/Sales_insights/images/Tableau_snapshot_large.png)











