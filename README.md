# SQL_retail_sales_project
This is all about my first sql project i created and uploaded in github

**Project Overview**
Project Title: Retail Sales Analysis
Level: Beginner
Database: p1_retail_db

**This project is designed to demonstrate SQL skills and techniques typically used by data analysts to explore, clean, and analyze retail sales data. The project involves setting up a retail sales database, performing exploratory data analysis (EDA), and answering specific business questions through SQL queries. This project is ideal for those who are starting their journey in data analysis and want to build a solid foundation in SQL.**

**--Sql coding--**
**--date exploration--**
'''sql
select * 
from retail_sales
'''
**--how many sale records  we have--**
'''sql
select count(*) 
from retail_sales
'''

**--how many unique customers we have--**
'''sql
select count(distinct customer_id) as customer_count 
from retail_sales
'''

**--how many categories we have and their transactions count--**
'''sql
select category, 
count( distinct transactions_id) as transactions
from retail_sales
group by category;
'''

**--giving ranking based on their transtions count--**
'''sql
select category, 
count( distinct transactions_id) as transactions,
ROW_NUMBER() over(order by category) as RN,
rank() over(order by count(transactions_id))
from retail_sales
group by category;
'''


--Data Analysis--
'''sql
select * from retail_sales
'''

--Write a SQL query to retrieve all columns for sales made on '2022-11-05:--
'''sql
select * 
from retail_sales
where sale_date = '2022-11-05';
'''

--Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022:
'''sql
select * 
from retail_sales
where category = 'clothing' and
quantiy >=4 and 
year(sale_date)=2022 and 
MONTH(sale_date)=11;
'''


--Write a SQL query to calculate the total sales (total_sale) for each category--
'''sql
select category, 
sum(total_sale) as total_sale,
count(*) as total_orders
from retail_sales
group by category;
'''


--Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category--
'''sql
select avg(age) AS avg_age, 
category
from retail_sales
where category = 'clothing'
group by category
'''


select round(avg(age),2) as age
from retail_sales;

--Write a SQL query to find all transactions where the total_sale is greater than 100--
'''sql
select * from retail_sales
where total_sale>100;
'''


--Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.:
'''sql

select  gender,
category,
count(transactions_id) as transaction_count
from retail_sales
group by gender,category
order by category;
'''


--Write a SQL query to calculate the average sale for each month. Find out best selling month in each year--
'''sql
select year,each_month, sales_amount
from
(
select 
(avg(total_sale))as sales_amount,
month(sale_date) as each_month, 
year(sale_date) as year,
rank() over(partition by year(sale_date) order by avg(total_sale) desc) as rankk
from retail_sales
group by month(sale_date),year(sale_date)) as t1
where rankk=1
 ;
'''


 --**Write a SQL query to find the top 5 customers based on the highest total sales **:
 '''sql
select top 5
customer_id,
sum(total_sale) as total_salee
from retail_sales
group by customer_id
order by 
total_salee desc
;
'''


--Write a SQL query to find the number of unique customers who purchased items from each category.:
'''sql
select category, 
count(distinct customer_id) as unique_customers
from retail_sales
group by category;
'''


--Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17):
'''sql
with holy as (
select *,
case
		when DATEPART(HOUR, sale_time) <12  then 'morning'
		when DATEPART(HOUR, sale_time) between 12 and 17 then 'afternoon'
		else 'Evening'
		end as timings

from retail_sales)

select count(transactions_id), timings
from holy 
group by timings;
'''


--end--
