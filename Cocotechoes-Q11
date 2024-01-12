/*
Problem Statement : We have a product, say a pen. For each day of the year create a record of for sales of pens.
The record will have the units sold, the price, and the amount (units*price). The price and the
units would be random numbers within the range. The price can be between 20 and 30. The
units can be between 100 - 1000. But the price will remain the same for each month. Once the
data is created compute and output the following.
1. For each month the units sold and the amount
2. For each quarter the units sold and the amount.
3. The quarter which had the maximum sale in terms of amount.
4. The quarter that had the maximum sales in terms of units 
*/


create database cocotechoes_11;

use cocotechoes_11;

-- Step 1: Create the pen_sales table
CREATE TABLE pen_sales (
    sale_date DATE,
    units_sold INT,
    price FLOAT,
    amount FLOAT
);

-- Step 2: Insert random sales data for each day of the year
INSERT INTO pen_sales (sale_date, units_sold, price, amount)
SELECT 
    DATE '2024-01-01' + INTERVAL (numbers.n - 1) DAY AS sale_date,
    FLOOR(RAND() * (1000 - 100 + 1)) + 100 AS units_sold,
    ROUND(RAND() * (30 - 20) + 20, 2) AS price,
    0 AS amount
FROM (
    SELECT ROW_NUMBER() OVER () AS n
    FROM information_schema.columns
) AS numbers
WHERE numbers.n <= DAYOFYEAR(DATE '2024-12-31');

-- Step 3: Update the 'amount' column based on 'units_sold' and 'price'
UPDATE pen_sales
SET amount = units_sold * price;

-- Step 4: Create a table for monthly statistics
CREATE TABLE monthly_stats AS
SELECT
    EXTRACT(MONTH FROM sale_date) AS month,
    SUM(units_sold) AS total_units_sold,
    SUM(amount) AS total_amount
FROM pen_sales
GROUP BY EXTRACT(MONTH FROM sale_date)
ORDER BY month;

-- Step 5: Create a table for quarterly statistics
CREATE TABLE quarterly_stats AS
SELECT
    CONCAT('Q', CEIL(month / 3)) AS quarter,
    SUM(total_units_sold) AS total_units_sold,
    SUM(total_amount) AS total_amount
FROM monthly_stats
GROUP BY quarter
ORDER BY quarter;

-- Step 6: Find the quarter with the maximum sales in terms of amount
CREATE TABLE max_amount_quarter AS
SELECT
    quarter
FROM quarterly_stats
ORDER BY total_amount DESC
LIMIT 1;

-- Step 7: Find the quarter with the maximum sales in terms of units
CREATE TABLE max_units_quarter AS
SELECT
    quarter
FROM quarterly_stats
ORDER BY total_units_sold DESC
LIMIT 1;

-- Step 8: Display the results
SELECT * FROM pen_sales;

-- 1. For each month the units sold and the amount
SELECT * FROM monthly_stats;
-- 2. For each quarter the units sold and the amount.
SELECT * FROM quarterly_stats;
-- 3. The quarter which had the maximum sale in terms of amount.
SELECT * FROM max_amount_quarter;
-- 4. The quarter that had the maximum sales in terms of units
SELECT * FROM max_units_quarter;
