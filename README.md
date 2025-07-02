# KMS-Inventory

A complete SQL project that explores Kultra Mega Stores inventory data containing order data from 2009 to 2012 to analyze the data and present key insights and findings.

---
##  Objective
- Query kms inventory data using SQL  
- Answer key business questions on revenue, sales, shipping cost, region, customer segments, and product category  
- Provide actionable recommendations based on data‑driven insights

##  Project Description
- The dataset is an Excel file that captures order data from 2009 to 2012 transactions from multiple regions, including fields such as customer name, order Id, product category,shipping method, costumer segment, sales, profit, product name, shipping date and discount.  
We create a database, import data, run analytical SQL queries, summarize findings, and suggest strategies to boost performance.
---

## 🗄️ Creating Database
```sql
create database kultra
```
- Import the KMS Dataset into the sql server within the database

---
1. Which product category had the highest sales?
```sql
select top 1 Product_Category,sum(sales) as total_sales
from [KMS Sql Case Study]
group by Product_Category
order by total_sales desc;
```
![Screenshot (78)](https://github.com/user-attachments/assets/a6108b5f-88e8-4631-8a09-c3632dfcd544)

---
2a. What are the Top 3 regions in terms of sales?
```sql
select top 3 Region, sum(sales) as total_sales
from [KMS Sql Case Study]
group by Region
order by total_sales desc;
```
![Screenshot (79)](https://github.com/user-attachments/assets/d169b2ce-a1ea-46c8-b77f-90238e845f42)

---
2b. What are the Bottom 3 regions in terms of sales?
```sql
select top 3 Region, sum(sales) as total_sales
from [KMS Sql Case Study]
group by Region
order by total_sales asc;
```
![Screenshot (80)](https://github.com/user-attachments/assets/f3427b30-5050-4582-9ab4-57c6a573145f)

---
3. What were the total sales of appliances in Ontario?
```sql
select sum(sales) as total_sales
from [KMS Sql Case Study]
where Product_Sub_Category ='Appliances' or Region = 'Ontario';
```
![Screenshot (81)](https://github.com/user-attachments/assets/067f51f4-7486-4239-828e-6db5741301f0)

---
4. Advise the management of KMS on what to do to increase the revenue from the bottom 10 customers?

To do that we will need to check some relationship within the data;

   What are the Bottom 10 customers according to sales?
```sql
select top 10 Customer_Name,
sum(sales) as total_sales
from [Kms sql case study]
group by Customer_Name
order by total_sales asc;
```
![Screenshot (82)](https://github.com/user-attachments/assets/2113e0c9-7ac5-47ab-9980-97076c5b9794)

What is the bottom 10 customers by Region and Customer segment?
```sql
	SELECT 
    [Customer_Name],
    region,
    [Customer_Segment]
FROM 
    [Kms sql case study]
WHERE 
    [Customer_Name] IN (
        SELECT TOP 10 [Customer_Name]
        FROM [Kms sql case study]
        GROUP BY [Customer_Name]
        ORDER BY SUM(sales) ASC
    )
GROUP BY 
    [Customer_Name], region, [Customer_Segment];
```
What is the Bottom 10 Customers by Product Category?
```sql
select Customer_Name,Product_Category,
sum(sales) as category_sales
from [Kms sql case study]
where Customer_Name in (
        select top 10 [Customer_Name]
        from[Kms sql case study]
        group by [Customer_Name]
        order by sum(sales) asc)
group by
    [Customer_Name], [Product_Category]
order by 
    [Customer_Name], category_sales desc;
```
![Screenshot (92)](https://github.com/user-attachments/assets/6c52b137-80d9-40f4-83a6-a76b522e4322)

Recommendation to increase Revenue
* Promote high-margin products within their preferred categories (Office Supplies).
* Introduce loyalty incentives for low-frequency customers
* Explore regional marketing in the regions they are from.
---
5. KMS incurred the most shipping cost using which shipping method?
```sql
select top 1 ship_mode,
    sum(Shipping_Cost) AS total_shipping_cost
    from  [Kms sql case study]
group by ship_mode
order by total_shipping_cost desc;
```
---
```
```
```
```
```
```

