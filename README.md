# SQL-Retail-sales-analysis-Project
--SQL Retail sales analysis--
--Create Table--
Create Table Retail 
(transactions_id INT,
sale_date DATE,
sale_time TIME,
customer_id INT,
gender VARCHAR(50),
age INT,
category VARCHAR(50),
quantiy INT,
price_per_unit FLOAT,
cogs FLOAT,
total_sale FLOAT
)
SELECT*FROM Retail
SELECT COUNT(*)
FROM RETAIL

--DATA CLEANING--

SELECT*FROM Retail
WHERE 
transactions_id IS NULL
OR
sale_date IS NULL
OR
sale_time IS NULL
OR
customer_id IS NULL
OR
gender IS NULL
OR
age IS NULL
OR
category IS NULL
OR
quantiy IS NULL
OR
price_per_unit IS NULL
OR
cogs IS NULL
OR
total_sale IS NULL;

-- DATA EXPLORATION--

--1.HOW MANY SALES WE HAVE?--

SELECT COUNT(*) AS total_sale FROM Retail

--2.HOW MANY CUSTOMERS WE HAVE?--

SELECT COUNT(DISTINCT customer_id)AS total_sale FROM Retail

--3.HOW MANY CATEGORY WE HAVE?--

SELECT DISTINCT category FROM Retail

SELECT * FROM Retail

--DATA ANALYSIS AND BUSINESS PROBLEMS&ANSWERS--

--1.Write a SQL query to retrieve all columns for sales made on '2022-11-05'--

SELECT*
FROM Retail
WHERE sale_date = '2022-11-05';

--2.Write a SQL query to retrieve all transactions where the category is 'Clothing' and the quantity sold is more than 4 in the month of Nov-2022.--

SELECT *
FROM Retail
WHERE category = 'Clothing'
AND
TO_CHAR(sale_date, 'YYYY-MM')='2022-11'
AND
quantiy >=4;

--3.Write a SQL query to calculate the total sales (total_sale) for each category.--

SELECT
category,
SUM(total_sale) AS net_sale
FROM Retail
GROUP BY 1;

--4.Write a SQL query to find the average age of customers who purchased items from the 'Beauty' category.--

SELECT * FROM Retail

SELECT
ROUND(AVG(age),2) AS avg_age
FROM Retail
WHERE category = 'Beauty';

--5.Write a SQL query to find all transactions where the total_sale is greater than 2000.--

SELECT*
FROM Retail
WHERE total_sale >=2000
LIMIT 10;

--6.Write a SQL query to find the total number of transactions (transaction_id) made by each gender in each category.--

SELECT
category,
gender,
COUNT(*) AS total_Trans
FROM Retail
GROUP BY category,gender
ORDER BY 1;

--7.Write a SQL query to calculate the average sale for each month. Find out best selling month in each year.--
SELECT
year,
month,
avg_sale
FROM
(
SELECT
EXTRACT(YEAR FROM sale_date) AS year,
EXTRACT(MONTH FROM sale_date) AS month,
AVG(total_sale) AS avg_sale,
RANK()OVER(PARTITION BY EXTRACT(YEAR FROM sale_date)ORDER BY AVG(total_sale)DESC) AS RANK
FROM Retail
GROUP BY 1,2
) AS T1
WHERE RANK=1
--ORDER BY 1,3 DESC--

--8.Write a SQL query to find the top 5 customers based on the highest total sales.--

SELECT*FROM Retail

SELECT
customer_id,
SUM(total_sale) AS High_sale
FROM Retail
GROUP BY 1
ORDER BY 2 DESC

--9.Write a SQL query to find the number of unique customers who purchased items from each category.--

SELECT
category,
COUNT(DISTINCT customer_id) AS Customer_uq
FROM Retail
GROUP BY category

-- 10.Write a SQL query to create each shift and number of orders (Example Morning <12, Afternoon Between 12 & 17, Evening >17)--

WITH Hourly_sale
AS
(
SELECT*,
CASE
WHEN EXTRACT(HOUR FROM sale_time) <12 THEN 'Morning'
WHEN EXTRACT(HOUR FROM sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
ELSE 'Evening'
END AS SHIFT
FROM Retail
)
SELECT
Shift,
COUNT(*) AS total_orders
FROM Hourly_sale
GROUP BY Shift


--END OF PROJECT--
