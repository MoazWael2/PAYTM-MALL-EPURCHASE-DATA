# Retail Purchase Analysis: SQL Queries 

## Introduction
- Welcome to the Psyliq Internship Project: Data Analysis repository. This project is a part of my internship with Psyliq, where I had the opportunity to work with a rich dataset that captures the sales transactions of a retail store in India of one month. The dataset contains valuable information, including store identifiers, customer details, shipping cities, product pricing, costs, discounts, product categories, and customer payments.

## About the Data
- The dataset used in this project represents a one-month snapshot of sales activity for a retail store based in India. It offers a comprehensive view of various aspects of the store's operations, including sales performance, product categories, and pricing strategies. The data encompasses key fields such as store identifiers, customer names, shipping cities, product prices, costs, discounts applied, product categories, and the amounts paid by customers.

## Analysis Objectives and SQL Queries
### 1- What does the "Category_Grouped" column represent, and how many unique categories are there?
``` SQL
-- SQL Query to Fetch Unique Categories and Their Count from 'paytm' Table

-- Part 1: Counting the Number of Unique Categories
SELECT 
    'No_OF_unique_categories' AS Indicator, 
    COUNT(DISTINCT Category_Grouped) AS Value
FROM 
    paytm
WHERE 
    Category_Grouped !=  ''
GROUP BY 
    Indicator

UNION ALL 

-- Part 2: Listing Distinct Category Grouped Values
SELECT 
    DISTINCT(Category_Grouped),
    'Category_Grouped' AS Value
FROM 
    paytm
WHERE 
    Category_Grouped !=  '';
```
##### Result
![1](https://github.com/MoazWael2/PAYTM-MALL-EPURCHASE-DATA/assets/137816418/0d2b092a-bec2-453a-95b9-5cb3e2d7f8d4)

### 2- List the top 5 shipping cities in terms of the number of orders?
##### SQL 
```SQL
-- SQL Query to Fetch Top 5 Shipping Cities by Number of Orders

SELECT 
    Shipping_city,
    COUNT(DISTINCT Order_ID) AS Total_order 
FROM 
    paytm
WHERE 
    Shipping_city IS NOT NULL
GROUP BY 
    Shipping_city
ORDER BY 
    Total_order DESC
LIMIT 
    5;
```
##### Result 
![2](https://github.com/MoazWael2/PAYTM-MALL-EPURCHASE-DATA/assets/137816418/eabad4a2-9e08-4f38-947e-209f1adadd1c) 

### 3- Show me a table with all the data for products that belong to the "Electronics" category?
##### SQL
```SQL
-- Show me a table with all the data for products that belong to the "Electronics" category.
  SELECT 
      *
  FROM 
      paytm
  WHERE
      Category = "Electronics";
```

### 4- Filter the data to show only rows with a "Sale_Flag" of 'Yes'?
##### SQL
```SQL
-- SQL Query to Filter Data for Items on Sale in the 'paytm' Table

-- In the 'Sale_Flag' column, items on sale are marked as 'on Sale', and items not on sale are marked differently (e.g., 'not on sale').
-- We use 'on Sale' in the WHERE clause to specifically target items that are currently being offered with a sales promotion.

SELECT 
    * 
FROM 
    paytm
WHERE 
    Sale_Flag = 'on Sale';
```
##### Result
![4](https://github.com/MoazWael2/PAYTM-MALL-EPURCHASE-DATA/assets/137816418/cfbab487-2939-4e5e-b455-f2f9d7e697cd)

### 5- What is the most expensive item?
##### SQL
```SQL
-- SQL Query to Find the Most Expensive Item in the 'paytm' Table Using CTE

-- The CTE, named 'expensive_item', isolates the process of finding the most expensive item.
-- The main query then selects the item name from this CTE.

WITH expensive_item AS (
    SELECT 
        Item_NM,
        Item_Price
    FROM 
        paytm
    ORDER BY 
        Item_Price DESC
    LIMIT 1
)
SELECT 
    Item_NM
FROM 
    expensive_item;
```
##### Result
![5](https://github.com/MoazWael2/PAYTM-MALL-EPURCHASE-DATA/assets/137816418/813def96-e0c7-4632-9478-fd7b433a57ed)


### 6- Find the total sales value for each category?
##### SQL
```SQL
-- SQL Query to Calculate Total Sales Value for Each Category

-- This query calculates the total sales value for each category by summing the product of 'Quantity' and 'Paid_pr' for each item.
-- It provides insights into the sales performance of different product categories.

SELECT
    Category,
    SUM(Quantity * Paid_pr) AS Total_Sales
FROM 
    paytm
WHERE 
    Category != ' ' 
GROUP BY 
    Category
ORDER BY
    Total_Sales DESC;
```
#####  Result
![6](https://github.com/MoazWael2/PAYTM-MALL-EPURCHASE-DATA/assets/137816418/087c427f-5e5b-4276-9e8e-f5877ee7b5d9)

### 7- Calculate the Sum "Quantity" sold for products in the "Clothing for man and women" category?
##### SQL
```SQL
-- SQL Query to Calculate Total Quantity Sold for "Clothing for Men and Women" Category

-- This query calculates the total quantity sold for products in the "Clothing for Men and Women" category.
-- It provides insights into the sales performance of this specific category.

SELECT
    Category,
    SUM(Quantity) AS Sum_Quantity
FROM 
    paytm
WHERE 
    Category IN ('Women Apparel', 'Men Apparel')
GROUP BY
    Category;
```
##### Result
![7](https://github.com/MoazWael2/PAYTM-MALL-EPURCHASE-DATA/assets/137816418/3888fc3c-621b-46de-8c62-7fc334ded325)

### 8- Find the Top 5 Product Types with the Highest Profit?
##### SQL
```SQL
-- SQL Query to Find the Top 5 Product Types with the Highest Profit

-- This query identifies the top 5 product types (Bricks) that generated the highest profits.
-- It calculates profits by subtracting the total amount paid by customers ('Paid_pr') from item prices ('Item_Price').

-- SQL Query to Find the Top 5 Product Types with the Highest Profit

-- This query identifies the top 5 product types (Bricks) that generated the highest profits.
-- It calculates profits by subtracting 'Paid_pr' from 'Cost_Price' and then multiplying by 'Quantity'.

WITH Profit AS (
    SELECT 
        Brick,
        ROUND(SUM(( Paid_pr - Cost_Price) * Quantity),2) AS Total_Profit
    FROM 
        paytm
    GROUP BY 
        Brick
    ORDER BY 
        Total_Profit DESC
    LIMIT 5
) 
SELECT 
    Brick,
    Total_Profit
FROM 
    Profit;
```
##### Result
![7](https://github.com/MoazWael2/PAYTM-MALL-EPURCHASE-DATA/assets/137816418/face2569-d849-4127-8a34-42bc2dc6411e)

### 9- what is the Top 5 Shipping Cities by Average Order Value?
##### SQL
```SQL
-- SQL Query to Identify the Top 5 Shipping Cities by Average Order Value

-- This query identifies the top 5 shipping cities based on the average order value (total sales amount divided by the number of orders).
-- It then displays the average order values for these cities.

WITH City AS (
    SELECT 
        Shipping_city,
        SUM(Paid_pr) AS Total_Revenue,
        COUNT(DISTINCT Order_ID) AS Total_order
    FROM 
        paytm
    GROUP BY Shipping_city
)
SELECT 
    Shipping_city,
    Total_Revenue / Total_order AS AVG_order_value
FROM 
    City
ORDER BY 
    AVG_order_value DESC
LIMIT 5;
```
### Result
![9](https://github.com/MoazWael2/PAYTM-MALL-EPURCHASE-DATA/assets/137816418/65188c14-f106-45f2-9e18-263871ab4c70)

### 10-
##### SQL
```SQL
-- SQL Query to Assess the Impact of Discounts on Profitability

-- This query calculates the impact of discounts on profitability by comparing total profit with and without discounts.

WITH Profit AS (
    SELECT 
        ROUND(SUM((Paid_pr - Cost_Price) * Quantity), 2) AS Total_Profit_with_Discount,
        ROUND(SUM((Item_Price - Cost_Price) * Quantity), 2) AS Total_Profit_IF_NOT_their_Discount
    FROM 
        paytm
)
SELECT 
    Total_Profit_with_Discount,
    Total_Profit_IF_NOT_their_Discount,
    Total_Profit_with_Discount - Total_Profit_IF_NOT_their_Discount AS Impact_of_Discount
FROM 
    Profit;
```
##### Result 
![10](https://github.com/MoazWael2/PAYTM-MALL-EPURCHASE-DATA/assets/137816418/b7fb90c0-f29d-4b00-94df-e30524cc0a29)
