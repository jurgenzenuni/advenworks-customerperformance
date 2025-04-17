📊 Customer Performance Dashboard | Adventure Works

![adventworks-dash-customers](https://github.com/user-attachments/assets/96ba91a6-dab3-4057-a0ed-c6b24f71ffc7)

This Power BI dashboard provides insights into customer performance using sales data from the Adventure Works dataset. The report includes key metrics, product trends, and customer behavior patterns.
🔍 Overview

Dashboard Features:

    Total Revenue, Total Orders, Total Customers, and Average Order Value KPIs.

    Revenue by Product

    Order Quantity by Product

    Orders Over Time (Monthly)

    Customer Purchase Frequency

    Top Customers by Revenue

    Average Order Value by Month

📅 Time Periods

Toggle between the years 2011, 2012, 2013, and 2014 to analyze year-specific data.
🛠 Data Source

    Database: AdventureWorksDW (or similar)

    Tools Used: SQL Server, Power BI

📂 SQL Queries Used
1. Main Sales Data Query

SELECT 
    soh.OrderDate,
    sod.SalesOrderID,
    p.Name AS Product,
    sod.OrderQty,
    sod.UnitPrice,
    sod.LineTotal,
    CONCAT_WS('', per.FirstName, ' ', per.LastName) AS Customer_name,
    per.BusinessEntityID AS customerid
FROM Sales_SalesOrderDetail sod
JOIN Sales_SalesOrderHeader soh ON sod.SalesOrderID = soh.SalesOrderID
JOIN Production_Product p ON sod.ProductID = p.ProductID
JOIN Sales_Customer sc ON soh.CustomerID = sc.CustomerID
JOIN Person_Person per ON sc.PersonID = per.BusinessEntityID
ORDER BY soh.OrderDate;

2. Product Categories and Revenue

SELECT 
    pc.Name AS Category,
    psc.Name AS Subcategory,
    p.Name AS Product,
    SUM(sod.LineTotal) AS TotalRevenue
FROM Sales_SalesOrderDetail sod
JOIN Production_Product p ON sod.ProductID = p.ProductID
LEFT JOIN Production_ProductSubcategory psc ON p.ProductSubcategoryID = psc.ProductSubcategoryID
LEFT JOIN Production_ProductCategory pc ON psc.ProductCategoryID = pc.ProductCategoryID
GROUP BY pc.Name, psc.Name, p.Name;

3. Region-Based Revenue

SELECT 
    st.Name AS Territory,
    SUM(sod.LineTotal) AS TotalRevenue
FROM Sales_SalesOrderDetail sod
JOIN Sales_SalesOrderHeader soh ON sod.SalesOrderID = soh.SalesOrderID
JOIN Sales_Customer c ON soh.CustomerID = c.CustomerID
JOIN Sales_SalesTerritory st ON soh.TerritoryID = st.TerritoryID
GROUP BY st.Name;

📈 Visualizations in Power BI

    Bar Charts for revenue and order quantity by product.

    Line Charts for orders and average order value over time.

    Tables for top customers by revenue and customer frequency.

    Cards for high-level KPIs.

📎 Notes

    The dashboard filters by year and aggregates multiple levels of data, such as customer-level and product-level metrics.

    Power BI's slicers and tooltips enhance interactivity and data exploration.
