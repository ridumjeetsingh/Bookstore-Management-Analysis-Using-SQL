# Bookstore-Management-Analysis-Using-SQL

  ## Project Overview

This project focuses on analyzing data from a bookstore's database using SQL. It involves data retrieval, aggregation, and insights generation across books, customers, and orders.

## Database Structure

Books Table: Contains book details such as title, author, genre, price, and stock.

Customers Table: Stores customer information including name, email, and location.

Orders Table: Tracks book purchases, linking customers to their orders with details like order date, quantity, and total amount.

## Key Insights and Analysis

1. Genre and Publication Insights

- Retrieve Fiction books.

- Find books published after 1950.

2. Customer Insights

- List customers from Canada.

- Identify customers who placed multiple orders.

3. Sales and Revenue Insights

- Total orders placed in November 2023.

- Total bookstore revenue and total stock available.

4. Book Performance Analysis

- Identify the most expensive book.

- Find the top 3 most expensive Fantasy books.

- List books with the lowest stock.

5. Order and Spending Insights

- Identify the customer who spent the most.

- List cities where high-spending customers are located.

- Calculate remaining stock after fulfilling orders.

``` SQL
-- Cretaing Books table.

DROP TABLE IF EXISTS Books;
CREATE TABLE Books(
Book_ID INT PRIMARY KEY,
Title VARCHAR(100),
Author VARCHAR(100),
Genre VARCHAR(50),
Published_Year INT, 
Price NUMERIC(10,2),
Stock INT
);

DROP TABLE IF EXISTS Customers;
CREATE TABLE Customers(
Customer_ID INT PRIMARY KEY,
Name VARCHAR(100),
Email VARCHAR(100),	
Phone VARCHAR(15),
City VARCHAR(50),
Country VARCHAR(70)
);

DROP TABLE IF EXISTS Orders;
CREATE TABLE Orders(
Order_ID INT PRIMARY KEY,
Customer_ID INT REFERENCES Customers(Customer_ID), 
Book_ID INT REFERENCES Books(Book_ID),
Order_Date DATE,
Quantity INT,
Total_Amount NUMERIC(10,2)
);

-- IMPORTING DATA OF ALL THREE DATA SETS

SELECT * FROM Books;
SELECT * FROM Customers;
SELECT * FROM Orders;

-- 1. Retrive all the Fiction queries from books:

SELECT * 
FROM Books
WHERE genre = 'Fiction'

-- 2. Find books published after 1950:

SELECT * 
FROM Books
WHERE published_year > 1950
ORDER BY published_year

-- 3. List all the customers from Canada:

SELECT *
FROM Customers
WHERE country = 'Canada'

-- 4. Show no. of orders placed in November 2023:

SELECT COUNT(*) as Total_orders
FROM orders
WHERE order_date BETWEEN '2023-11-01' AND '2023-11-30'

-- 5. Retrieve the total stock of book available:

SELECT SUM(stock) as Total_stocks
FROM Books

-- 6. Find the most Expensive book:

SELECT book_id,title,price
FROM Books
ORDER BY price DESC
LIMIT 1

-- 7. List of customers who have order more than 1 quantity of a books:

SELECT o.customer_id,c.name,o.quantity 
FROM Orders o
INNER JOIN customers c ON c.customer_id = o.customer_id  
WHERE quantity > 1
ORDER BY quantity

-- 8. Retrieve all the orders where the total amount exceeds $20:

SELECT *
FROM Orders
WHERE total_amount > 20
ORDER BY total_amount

-- 9. List all the genre available in the book table: 

SELECT DISTINCT genre
FROM Books

-- 10. Find the books with 5 lowest stock:

SELECT book_id, title, stock
FROM Books
ORDER BY stock
LIMIT 5

-- ADVANCED SECTION
-- 11. Find the total revenue from all orders:

SELECT SUM(quantity * total_amount) total_revenue
FROM Orders

-- 12. Find the total number of books sold for each genre:

SELECT b.genre, SUM(o.quantity) as total_quantity
FROM Books b
JOIN Orders o ON b.book_id=o.book_id
GROUP BY 1

-- 13. Find the Average price of books in Fantasy genre:

SELECT AVG(price) as Average_price
FROM Books
WHERE genre = 'Fantasy'

-- 14. List customers from customer table who have placed at least 2 orders from order table:

SELECT o.customer_id, c.name, COUNT(o.order_id) as Total_orders 
FROM Orders o
JOIN Customers c ON c.customer_id=o.customer_id
GROUP BY 1,2
HAVING COUNT(o.order_id) >= 2

-- 15. Find the most frequently ordered books:

SELECT o.book_id,b.title,COUNT(o.order_id) as Total_orders
FROM Orders o
JOIN Books b ON b.book_id=o.book_id
GROUP BY 1,2
ORDER BY Total_orders DESC
LIMIT 1

-- 16. Show the top 3 most expensive books from 'Fantasy' Genre:

SELECT *
FROM Books
WHERE genre = 'Fantasy'
ORDER BY price DESC
LIMIT 3

-- 17. Find the total quantity of books sold by each author: 

SELECT b.author,SUM(o.quantity) as Total_quantity
FROM Books b
JOIN Orders o ON b.book_id=o.book_id
GROUP BY 1
ORDER BY Total_quantity DESC

-- 18. List the cities where customers who spent over $30 are located:	

SELECT DISTINCT c.city, o.total_amount
FROM Customers c
JOIN Orders o ON o.customer_id=c.customer_id
WHERE o.total_amount > 30


-- 19. Find the customer who spent most on orders:

SELECT c.customer_id, c.name, SUM(o.total_amount) as TotaL_amount
FROM Customers c
JOIN Orders o ON o.customer_id=c.customer_id
GROUP BY 1,2
ORDER BY TotaL_amount DESC
LIMIT 1

-- 20. Calculate the stock	remaining after fulfilling all orders:

SELECT b.book_id,b.title,b.stock,COALESCE(SUM(o.quantity),0) as order_quantity,
		(b.stock - COALESCE(SUM(o.quantity),0)) as Remaining_stock
FROM books b
LEFT JOIN orders o ON o.book_id=b.book_id
GROUP BY 1,2,3
ORDER BY book_id



```

Advanced Techniques Used

- JOINS: For combining data across tables.

- Aggregation Functions: For revenue calculation, average price, and total orders.

- Filtering & Conditions: Applied for targeted insights.

- Ordering & Limiting: Used to identify top-performing books, authors, and customers.

## Conclusion

This project demonstrates effective use of SQL for data extraction, transformation, and insights generation in a bookstore environment. It highlights data-driven decision-making capabilities for improving stock management, customer engagement, and revenue analysis.

