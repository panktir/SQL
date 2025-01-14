# Customer & Order Data Querying

### SQL Project

This project analyzes sales data from a database to gain insights into sales patterns and product performance. The following SQL queries were executed:

#### How many orders were placed in January?
```sql
SELECT COUNT(orderID) FROM BIT_DB.JanSales
WHERE length(orderID) = 6
AND orderID <> ''
```

#### How many of those orders were for an iPhone?
```sql
SELECT COUNT(*) FROM BIT_DB.JanSales
WHERE length(orderID) = 6
AND orderID <> ''
AND Product = 'iPhone'
```

#### Select the customer account numbers for all the orders that were placed in February.
```sql
SELECT DISTINCT(acctnum)
FROM BIT_DB.customers AS cust
INNER JOIN BIT_DB.FebSales Feb
ON cust.order_id = Feb.orderID
WHERE length(orderID) = 6
AND orderID <> ''
```

#### Which product was the cheapest one sold in January, and what was the price?
```sql
SELECT DISTINCT Product, MIN(price) AS min_price
FROM BIT_DB.JanSales
ORDER BY price ASC
LIMIT 1
```

#### What is the total revenue for each product sold in January?
```sql
SELECT ROUND(sum(quantity) * price,2) AS revenue, product
FROM BIT_DB.JanSales
WHERE product <> ''
GROUP BY product
```

#### Which products were sold in February at 548 Lincoln St, Seattle, WA 98101, how many of each were sold, and what was the total revenue?
```sql
SELECT product, quantity, ROUND(sum(quantity) * price,2) AS feb_revenue
FROM BIT_DB.FebSales
WHERE location = '548 Lincoln St, Seattle, WA 98101'
GROUP BY product
```

#### How many customers ordered more than 2 products at a time in February, and what was the average amount spent for those customers?
```sql
SELECT COUNT(DISTINCT customer.acctnum), AVG(price)
FROM BIT_DB.FebSales Feb
LEFT JOIN BIT_DB.customers customer
ON Feb.orderID = customer.order_id
WHERE Feb.Quantity > 2
AND length(orderID) = 6
AND orderID <> ''
```

#### List all the products sold in Los Angeles in February, and include how many of each were sold.
```sql
SELECT Product, SUM(Quantity)
FROM BIT_DB.FebSales
WHERE location like '%Los Angeles%'
GROUP BY Product
```

#### Which locations in New York received at least 3 orders in January, and how many orders did they each receive?
```sql
SELECT DISTINCT (location), COUNT(orderID)
FROM BIT_DB.JanSales
WHERE location like '%NY%'
AND length (orderID) = 6
AND (orderID) <> ''
GROUP BY location
HAVING count(orderID) > 3
```

#### How many of each type of headphone was sold in February?
```sql
SELECT sum(Quantity), Product
FROM BIT_DB.FebSales
WHERE Product like '%headphones%'
GROUP BY Product
```

#### What was the average amount spent per account in February?
```sql
SELECT AVG(quantity*price)
FROM BIT_DB.FebSales Feb
LEFT JOIN BIT_DB.customers cust
ON Feb.orderID=cust.order_id
WHERE length(orderID) = 6
AND (orderID) <> ''
```

#### Which product brought in the most revenue in January and how much revenue did it bring in total? 
```sql
SELECT product, sum(Quantity*price)
FROM BIT_DB.JanSales
GROUP BY Product
ORDER BY sum(Quantity*price) DESC 
LIMIT 1
```

These queries provide valuable insights into sales data, enabling data-driven decision-making and business optimization. By analyzing the data, we can understand the sales trends, popular products, customer behavior, and revenue generation for different periods and locations.
