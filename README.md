# 🛒 E-Commerce SQL Database Analysis

The project demonstrates core SQL concepts such as:
- Database creation
- Data querying
- Aggregations
- Views
- Joins
- Subqueries
- Index optimization

---

## 📂 Project Structure

```
├── customers.sql        # SQL file to create and populate the database
├── README.md            # Project documentation (this file)
```

---

## 🧱 Database Setup

### 1️⃣ Create and Select Database

```sql
CREATE DATABASE ecommerce;
USE ecommerce;
```

### 2️⃣ Load SQL File in MySQL CLI

```sql
SOURCE C:/Users/joker/Desktop/Internship/Day3/customers.sql;
```

> 💡 Use forward slashes `/` and absolute file paths.

---

## 🔍 SQL Queries Implemented

### 1. View all transactions from France
```sql
SELECT * FROM customers
WHERE Country = 'France'
ORDER BY InvoiceDate;
```

### 2. High-value items (price > 10)
```sql
SELECT InvoiceNo, Description, UnitPrice
FROM customers
WHERE UnitPrice > 10
ORDER BY UnitPrice DESC;
```

### 3. Total sales per country
```sql
SELECT Country, SUM(Quantity * UnitPrice) AS TotalSales
FROM customers
WHERE Quantity > 0
GROUP BY Country
ORDER BY TotalSales DESC;
```

### 4. Average order value per customer
```sql
SELECT CustomerID, AVG(Quantity * UnitPrice) AS AvgOrderValue
FROM customers
WHERE Quantity > 0
GROUP BY CustomerID
ORDER BY AvgOrderValue DESC;
```

---

## 👁️ Views

### Customers View
```sql
CREATE VIEW customerzz AS
SELECT DISTINCT CustomerID, Country
FROM customers
WHERE CustomerID IS NOT NULL;
```

### Products View
```sql
CREATE VIEW productzz AS
SELECT DISTINCT StockCode, Description, UnitPrice
FROM customers;
```

---

## 🔗 Joins

### INNER JOIN – Customer Purchases
```sql
SELECT c.CustomerID, p.Description, t.Quantity, t.UnitPrice
FROM customers t
INNER JOIN customerzz c ON t.CustomerID = c.CustomerID
INNER JOIN productzz p ON t.StockCode = p.StockCode;
```

### LEFT JOIN – Customers even if no purchases
```sql
SELECT c.CustomerID, COUNT(t.InvoiceNo) AS TotalTransactions
FROM customerzz c
LEFT JOIN customers t ON c.CustomerID = t.CustomerID
GROUP BY c.CustomerID;
```

---

## 🧠 Subquery

### Products priced above average
```sql
SELECT StockCode, Description, UnitPrice
FROM customers
WHERE UnitPrice > (SELECT AVG(UnitPrice) FROM customers);
```

---

## 📊 Analytical View

### Sales Summary View
```sql
CREATE VIEW sales_summary AS
SELECT InvoiceNo, CustomerID, Country,
       SUM(Quantity * UnitPrice) AS InvoiceTotal
FROM customers
WHERE Quantity > 0
GROUP BY InvoiceNo, CustomerID, Country;
```

```sql
SELECT * FROM sales_summary ORDER BY InvoiceTotal DESC;
```

---

## ⚡ Index Optimization

```sql
CREATE INDEX idx_customer_id ON customers(CustomerID(50));
CREATE INDEX idx_invoice_no ON customers(InvoiceNo(50));
CREATE INDEX idx_country ON customers(Country(50));
```

---

## ✅ Key Learnings

- Efficient SQL querying on transactional data
- Use of views for abstraction
- Performance optimization using indexes
- Writing clean, readable, and interview-ready SQL

---
