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
![Screenshot (84)](https://github.com/user-attachments/assets/75941083-d968-4665-8e8c-b6a135143eff)

---
6. Who are the most valuable customers, and what products or services do they typically purchase?
```sql
select top 10 Customer_Name,
sum (sales) as total_sales
from [KMS Sql Case Study]
group by Customer_Name
order by total_sales desc;
```
![Screenshot (85)](https://github.com/user-attachments/assets/b1c80b95-0b43-47be-9042-13be0319faec)

Product they typically Purchase
```sql
select customer_name, product_category, Product_Name,Product_Sub_Category,
    sum(sales) as product_sales
from [Kms sql case study]
where Customer_Name in (
        select top 10 Customer_Name
        from [Kms sql case study]
        group by Customer_Name
        order by sum(sales) desc)
group by
    Customer_Name,Product_Category,Product_Name,Product_Sub_Category
order by 
    Customer_Name, product_sales desc;
```
---
7.Which small business customer had the highest sales?
```sql
	SELECT TOP 1
    Customer_Name,
    SUM(Sales) AS Total_Sales
FROM 
    [KMS Sql Case Study]
WHERE 
    Customer_Segment = 'Small Business'
GROUP BY 
    Customer_Name
ORDER BY 
    Total_Sales DESC;
```
![Screenshot (88)](https://github.com/user-attachments/assets/5a9eef31-ad9d-43d1-9fa9-b22161694491)

---
8. Which Corporate Customer placed the most number of orders in 2009 – 2012?
```sql
	SELECT TOP 1
    Customer_Name,
    COUNT(DISTINCT Order_ID) AS Number_Of_Orders
FROM 
    [KMS Sql Case Study]
WHERE 
    Customer_Segment = 'Corporate'
    AND Order_Date BETWEEN '2009-01-01' AND '2012-12-31'
GROUP BY 
    Customer_Name
ORDER BY 
    Number_Of_Orders DESC;
```
![Screenshot (89)](https://github.com/user-attachments/assets/000d1d88-cf08-4edc-b528-08203b6f62f8)

---
9. Which consumer customer was the most profitable one?
```sql
	SELECT TOP 1
    Customer_Name,
    SUM(Profit) AS Total_Profit
FROM 
    [KMS Sql Case Study]
WHERE 
    Customer_Segment = 'Consumer'
GROUP BY 
    Customer_Name
ORDER BY 
    Total_Profit DESC;
```
![Screenshot (90)](https://github.com/user-attachments/assets/37a23a77-2fb7-44ee-acdf-1d92aabf8e37)

---
10. Which customer returned items, and what segment do they belong to?

 Import another Table named Order Status 
```sql
	select k.Customer_Name, 
	       k.Customer_Segment
		   from [KMS Sql Case Study] k
		   join [Order_Status] o on k.Order_ID = o.Order_ID
		   where o.Status ='Returned'
		   group by k.Customer_Name, k.Customer_Segment;
```
![Screenshot (91)](https://github.com/user-attachments/assets/5e74a27e-c66e-4961-8531-ba237f35a865)

---
11. If the delivery truck is the most economical but the slowest shipping method and Express Air is the fastest but the most expensive one, do you think the company appropriately spent shipping costs based on the Order Priority?

To answer that, I first analyzed average Shipping Cost by Shipping Mode and Order Priority
 ```sql
    	SELECT 
    Order_Priority,
    Ship_Mode,
    COUNT(DISTINCT Order_ID) AS Number_Of_Orders,
    AVG(Shipping_Cost) AS Avg_Shipping_Cost
FROM 
    [KMS Sql Case Study]
GROUP BY 
    Order_Priority, Ship_Mode
ORDER BY 
    Order_Priority, Ship_Mode;
```
![Screenshot (93)](https://github.com/user-attachments/assets/1f85c480-65d5-4fff-a5ea-3394f3903be6)

---
### Insight

KMS appears to be:

* Using Express Air for 33% Critical orders.
* However, 33% of Low priority orders were also shipped using Express Air, which may be inflating costs unnecessarily.
* High priority orders and Crititcal priority orders shipped by Delivery Truck is about 33% and  could result in delays that hurt customer satisfaction.
---

### Recommendation
* Match Order Priority to appropriate shipping modes.
* Set guardrails in the order system to prevent low-priority orders from using Express Air.
* Consider incentives for customers to choose economy shipping when urgency is low.

	






