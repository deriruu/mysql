#mysql
CREATE DATABASE RetailStoreDB;
USE RetailStoreDB;

-- Customers Table
CREATE TABLE Customers (
    CustomerID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE NOT NULL,
    Phone VARCHAR(15),
    Address VARCHAR(255),
    JoinDate DATE DEFAULT CURDATE()
);

-- Products Table
CREATE TABLE Products (
    ProductID INT AUTO_INCREMENT PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Category VARCHAR(50),
    Price DECIMAL(10, 2) NOT NULL,
    Stock INT DEFAULT 0
);

-- Orders Table
CREATE TABLE Orders (
    OrderID INT AUTO_INCREMENT PRIMARY KEY,
    CustomerID INT,
    OrderDate DATE DEFAULT CURDATE(),
    TotalAmount DECIMAL(10, 2),
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);

-- OrderDetails Table
CREATE TABLE OrderDetails (
    OrderDetailID INT AUTO_INCREMENT PRIMARY KEY,
    OrderID INT,
    ProductID INT,
    Quantity INT NOT NULL,
    Price DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID),
    FOREIGN KEY (ProductID) REFERENCES Products(ProductID)
);

-- Payments Table
CREATE TABLE Payments (
    PaymentID INT AUTO_INCREMENT PRIMARY KEY,
    OrderID INT,
    PaymentDate DATE DEFAULT CURDATE(),
    Amount DECIMAL(10, 2),
    PaymentMethod VARCHAR(50),
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID)
);

-- Insert Customers
INSERT INTO Customers (Name, Email, Phone, Address)
VALUES
('Alice Johnson', 'alice@example.com', '1234567890', '123 Maple St'),
('Bob Smith', 'bob@example.com', '9876543210', '456 Oak St');

-- Insert Products
INSERT INTO Products (Name, Category, Price, Stock)
VALUES
('Laptop', 'Electronics', 800.00, 10),
('Headphones', 'Accessories', 50.00, 100),
('Smartphone', 'Electronics', 600.00, 20);

-- Insert Orders and OrderDetails
INSERT INTO Orders (CustomerID, TotalAmount)
VALUES
(1, 850.00);

INSERT INTO OrderDetails (OrderID, ProductID, Quantity, Price)
VALUES
(1, 1, 1, 800.00),
(1, 2, 1, 50.00);

-- Insert Payments
INSERT INTO Payments (OrderID, Amount, PaymentMethod)
VALUES
(1, 850.00, 'Credit Card');


-- 1. Retrieve all customers who have placed orders
SELECT C.Name, C.Email, O.OrderID, O.TotalAmount
FROM Customers C
JOIN Orders O ON C.CustomerID = O.CustomerID;

-- 2. Get the details of products with low stock (less than 5 items)
SELECT * FROM Products WHERE Stock < 5;

-- 3. Find total revenue generated
SELECT SUM(Amount) AS TotalRevenue FROM Payments;

-- 4. Get order details including products and their prices for a specific order
SELECT O.OrderID, P.Name AS ProductName, OD.Quantity, OD.Price
FROM OrderDetails OD
JOIN Products P ON OD.ProductID = P.ProductID
JOIN Orders O ON OD.OrderID = O.OrderID
WHERE O.OrderID = 1;

-- 5. List top 3 most popular products by sales
