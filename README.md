# Pizza-Sales-Project



### **Dashboard Link Embeded link ** : [https://app.powerbi.com/links/9_R_PFXhtG?ctid=e59bad41-e823-45c9-b8d1-c55bd714b9ae&pbi_source=linkShare](https://app.powerbi.com/reportEmbed?reportId=afb21800-15b4-47fa-996f-d1e5b70f9fd0&autoAuth=true&ctid=e59bad41-e823-45c9-b8d1-c55bd714b9ae)


### **dashboard link** https://app.powerbi.com/links/2zeGeZaWLO?ctid=e59bad41-e823-45c9-b8d1-c55bd714b9ae&pbi_source=linkShare


## Problem Statement



![Image](https://github.com/users/chaitanyasja/projects/3/assets/114214825/e7a6f55b-1c87-413c-b618-dd8765d6e093)

## we use SSMS with SQL queries as per follow 


select * from pizza_sales;

**-- 1. Total revenue**

Select Sum(total_price) as Total_Revenue from pizza_sales ;

**--2.Average Order Value**

SELECT SUM(total_price)/COUNT(DISTINCT order_id) as Avg_Value from pizza_sales ;

**--3.Total Pizzas Sold**
SELECT SUM(quantity) AS Total_Pizzas_Sold FROM pizza_sales ;

**--4.Total Orders**
SELECT COUNT(DISTINCT order_id) AS Total_Orders FROM pizza_sales ; 

**--5.Average pizzas per order** 
SELECT CAST(CAST(SUM(quantity) AS DECIMAL(10,2)) / CAST(COUNT(DISTINCT order_id) AS DECIMAL(10,2)) as decimal(10,2)) AS Avg_pizzas_perOrder
from pizza_sales ;

**--6.Daily trend for Tatal orders**
SELECT DATENAME(DW, order_date) as order_day, 
	   COUNT(distinct order_id) AS TOTAL_ORDER
	   from pizza_sales 
	   GROUP BY DATENAME(DW, order_date)

**-- but we can see the day is not sorted right it not is sequence so lets solve it** 
SELECT DATENAME(DW, order_date) AS ORDER_DAY, 
	   COUNT(distinct order_id) AS TOTAL_ORDER
	   from pizza_sales 
	   GROUP BY DATENAME(DW, order_date)
	ORDER BY 
    CASE DATENAME(DW, order_date)
        WHEN 'Monday' THEN 1
        WHEN 'Tuesday' THEN 2
        WHEN 'Wednesday' THEN 3
        WHEN 'Thursday' THEN 4
        WHEN 'Friday' THEN 5
        WHEN 'Saturday' THEN 6
        WHEN 'Sunday' THEN 7 END ;

**--7.Monthly trend for total order**
	SELECT DATENAME (MONTH, order_date) as Order_Month, 
			COUNT(DISTINCT order_id) as total_order
			from pizza_sales 
			GROUP BY DATENAME (MONTH, order_date) 
			order by 
			CASE DATENAME (MONTH, order_date)
			WHEN 'January' THEN 1
        WHEN 'February' THEN 2
        WHEN 'March' THEN 3
        WHEN 'April' THEN 4
        WHEN 'May' THEN 5
        WHEN 'June' THEN 6
        WHEN 'July' THEN 7
        WHEN 'August' THEN 8
        WHEN 'September' THEN 9
        WHEN 'October' THEN 10
        WHEN 'November' THEN 11
        WHEN 'December' THEN 12
    END; 

**	**-- If you want highest rotal order as we need to descending**

	SELECT DATENAME (MONTH, order_date) as Order_Month, 
			COUNT(DISTINCT order_id) as total_order
			from pizza_sales 
			GROUP BY DATENAME (MONTH, order_date) 
			order by total_order DESC ;

**	**--8.Pdercentage of sales by pizza category.**

	SELECT pizza_category, SUM (total_price) as Total_sales, SUM(total_price)*100/(SELECT SUM(total_price)
	FROM pizza_sales) as Percof_Sales
	FROM pizza_sales
	GROUP BY pizza_category
	order by SUM(total_price)*100/(SELECT SUM(total_price)
	FROM pizza_sales) asc  ;
 

**	**--and there is also i need something extra , so lets do somthing extra we will get this in percentage**
**

SELECT 
    pizza_category,
    SUM(total_price) AS Total_sales,
    CONCAT(CAST((SUM(total_price)*100.0 / (SELECT SUM(total_price)
	FROM pizza_sales)) AS DECIMAL(10,2)), '%') AS Percof_Sales
FROM 
    pizza_sales
GROUP BY 
    pizza_category
ORDER BY 
    SUM(total_price)*100.0 / (SELECT SUM(total_price) FROM pizza_sales) ASC;
    

**-- and also i need by month means for specialy jan =1 , feb = 2....**

SELECT 
    pizza_category,
    SUM(total_price) AS Total_sales,
    CONCAT(CAST((SUM(total_price)*100.0 / (SELECT SUM(total_price)
	FROM pizza_sales)) AS DECIMAL(10,2)), '%') AS Percof_Sales
FROM 
    pizza_sales WHERE MONTH(order_date) IN (1)
GROUP BY 
    pizza_category
ORDER BY 
    SUM(total_price)*100.0 / (SELECT SUM(total_price) FROM pizza_sales) ASC;




**	**-- but if you look closely you are getting the wrong result let me put the right result 
**	-- we will also add pizza size**

	SELECT 
    pizza_category,pizza_size,
    SUM(total_price) AS Total_sales,
    CONCAT(CAST((SUM(total_price)*100.0 / (SELECT SUM(total_price)
	FROM pizza_sales WHERE MONTH(order_date) IN (1) )) AS DECIMAL(10,2)), '%') AS Percof_Sales
FROM 
    pizza_sales WHERE MONTH(order_date) IN (1)
GROUP BY 
    pizza_category , pizza_size
ORDER BY 
    SUM(total_price)*100.0 / (SELECT SUM(total_price) FROM pizza_sales) DESC;
    

**--top 5 best revenue , total quantity and total orders
	--(A) Revenue**
 
	Select top 5 pizza_name, Sum(total_price) as Total_Revenue 
	from pizza_sales
	group by pizza_name
	order by Total_Revenue desc ;
 

**	**--B.top 5 quantity**

Select top 5 pizza_name, Sum(quantity) as Total_Q 
	from pizza_sales
	group by pizza_name
	order by Total_Q desc ;


**	**--C. top 5 orders**
Select top 5 pizza_name, Sum(distinct order_id) as Total_order 
	from pizza_sales
	group by pizza_name
	order by Total_order desc ;






       

        
- Step 15 : New measure was created to find total 

Following DAX expression was written ,

**1.AVG Pizza Perorder = [Total Pizza Sold]/[Total Orders]**

**2. Total Avg Ordervalue = [Total Revenue]/[Total Orders]** 

**3.   Total Orders = DISTINCTCOUNT(pizza_sales[order_id])**  

**4.Total Pizza Sold = SUM(pizza_sales[quantity])**

**5.Total Revenue = sum(pizza_sales[total_price])**

**6.Total Vehicles = DISTINCTCOUNT(Electric_Vehicle_Population_Data[DOL Vehicle ID])**

 
 
    
 
 

 
 # Report Snapshot (Power BI DESKTOP)


![image](https://github.com/chaitanyasja/Pizza-Sales-Project/assets/114214825/834fd400-9cf7-442f-8687-aad259d64f93)


**#extra columns created for the simple visulization**


![image](https://github.com/chaitanyasja/Pizza-Sales-Project/assets/114214825/aea147f8-af9f-4203-befb-6d79942e98b7)



# Insights


**1.**

![image](https://github.com/chaitanyasja/Pizza-Sales-Project/assets/114214825/7c5c013c-8309-40f9-8b57-a04e63eb555e)


**2.**

![image](https://github.com/chaitanyasja/Pizza-Sales-Project/assets/114214825/7083179b-5429-4c5a-ad5a-7bb29aa3e386)


**3.**

![image](https://github.com/chaitanyasja/Pizza-Sales-Project/assets/114214825/b9e254ab-3e01-4355-8451-ec33ae4c5590)


**4.**

![image](https://github.com/chaitanyasja/Pizza-Sales-Project/assets/114214825/45a765b3-4eb7-4140-8820-d46d2c155b89)


**5.**

![image](https://github.com/chaitanyasja/Pizza-Sales-Project/assets/114214825/591fe44e-44e8-42df-967e-73b901e4754c)


**6.**

![image](https://github.com/chaitanyasja/Pizza-Sales-Project/assets/114214825/2dbbfd09-4185-4619-9de9-aec2c1e4e128)








