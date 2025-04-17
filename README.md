📊 Customer Performance Dashboard | Adventure Works

![adventworks-dash-customers](https://github.com/user-attachments/assets/96ba91a6-dab3-4057-a0ed-c6b24f71ffc7)

This project visualizes customer and sales performance metrics from the Adventure Works dataset using Power BI. The data was sourced via SQL queries from a relational database and transformed into insightful charts and KPIs.

## 🔍 Key Metrics Displayed

- Total Revenue: 109.85M  
- Total Orders: 31.47K  
- Total Customers: 19.12K  
- Average Order Value: 3.49K

## 📈 Visualizations Included

- Revenue by Product  
- Orders Over Time (Monthly)  
- Top Customers by Revenue  
- Order Quantity by Product  
- Customer Purchase Frequency  
- Average Order Value by Month

## 🧠 Insights

- The top contributing customers and products can be easily identified.  
- Seasonal trends are visible in order frequency and value.  
- The most popular and highest-revenue-generating products are highlighted.  
- Average order value fluctuations hint at promotional periods or buying behavior changes.

## 🗂️ SQL Queries Used

```sql
-- 1. Main Sales Data Query
SELECT 
    soh.OrderDate,
    sod.SalesOrderID,
    p.Name AS Product,
    sod.OrderQty,
    sod.UnitPrice,
    sod.LineTotal,
    CONCAT_WS(' ', per.FirstName, per.LastName) AS Customer_name,
    per.BusinessEntityID AS customerid
FROM Sales_SalesOrderDetail sod
JOIN Sales_SalesOrderHeader soh 
    ON sod.SalesOrderID = soh.SalesOrderID
JOIN Production_Product p 
    ON sod.ProductID = p.ProductID
JOIN Sales_Customer sc 
    ON soh.CustomerID = sc.CustomerID
JOIN Person_Person per 
    ON sc.PersonID = per.BusinessEntityID
ORDER BY soh.OrderDate;

-- 2. Product Categories and Revenue
SELECT 
    pc.Name AS Category,
    psc.Name AS Subcategory,
    p.Name AS Product,
    SUM(sod.LineTotal) AS TotalRevenue
FROM Sales_SalesOrderDetail sod
JOIN Production_Product p 
    ON sod.ProductID = p.ProductID
LEFT JOIN Production_ProductSubcategory psc 
    ON p.ProductSubcategoryID = psc.ProductSubcategoryID
LEFT JOIN Production_ProductCategory pc 
    ON psc.ProductCategoryID = pc.ProductCategoryID
GROUP BY pc.Name, psc.Name, p.Name;

-- 3. Region-Based Revenue
SELECT 
    st.Name AS Territory,
    SUM(sod.LineTotal) AS TotalRevenue
FROM Sales_SalesOrderDetail sod
JOIN Sales_SalesOrderHeader soh 
    ON sod.SalesOrderID = soh.SalesOrderID
JOIN Sales_Customer c 
    ON soh.CustomerID = c.CustomerID
JOIN Sales_SalesTerritory st 
    ON soh.TerritoryID = st.TerritoryID
GROUP BY st.Name;

