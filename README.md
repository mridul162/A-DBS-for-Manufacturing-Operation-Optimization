drop table Demand_Of;
drop table Has_Dept; 
drop table Has_PL;
drop table Produce;
drop table Place_Order;
drop table Demand; 
drop table Works_On;

drop table ProductionLine;
drop table Employee;
drop table Department;
drop table Orders;
drop table Product;
drop table PhoneBook_Manu;
drop table Manufacturer;
drop table customer;

set linesize 200;
set pagesize 200;

-- Create Customer table
CREATE TABLE Customer (
  CustomerID INT PRIMARY KEY,
  Name VARCHAR(15) NOT NULL,
  Phone VARCHAR(15) NOT NULL,
  Street VARCHAR(15) NOT NULL,
  City VARCHAR(15) NOT NULL,
  Division VARCHAR(15) NOT NULL
);

CREATE TABLE Manufacturer (
  Trade_License NUMBER PRIMARY KEY,
  Name VARCHAR(20),
  Address VARCHAR(30),
  Details CLOB
);

CREATE TABLE PhoneBook_Manu (
  Trade_License NUMBER,
  Phone VARCHAR(15) NOT NULL,
  CONSTRAINT fk_pb_manufacturer FOREIGN KEY (Trade_License) REFERENCES Manufacturer (Trade_License)
);

-- Create Product table
CREATE TABLE Product (
  ProductID INT PRIMARY KEY,
  Name VARCHAR(20) NOT NULL,
  Price DECIMAL(10,2) NOT NULL,
  MFG DATE NOT NULL
);

-- Create Orders table
CREATE TABLE Orders (
  OrderID INT PRIMARY KEY,
  OrderDate DATE NOT NULL,
  CustomerID INT NOT NULL,
  ProductID INT NOT NULL,
  Quantity INT NOT NULL,
  Price INT NOT NULL,
  CONSTRAINT fk_order_customer FOREIGN KEY (CustomerID) REFERENCES Customer (CustomerID),
  CONSTRAINT fk_order_product FOREIGN KEY (ProductID) REFERENCES Product (ProductID)
);
ALTER TABLE Orders ADD Amount INT GENERATED ALWAYS AS (Quantity * Price) VIRTUAL;

-- Create Department table
CREATE TABLE Department (
  DepartmentID INT PRIMARY KEY,
  Name VARCHAR(30) NOT NULL,
  ManagerID INT NOT NULL
);

-- Create Employee table
CREATE TABLE Employee (
  EmployeeID INT PRIMARY KEY,
  Name VARCHAR(20) NOT NULL,
  Phone VARCHAR2(20),
  Salary DECIMAL(10,2) NOT NULL,
  DepartmentID INT NOT NULL,
  CONSTRAINT fk_employee_department FOREIGN KEY (DepartmentID) REFERENCES Department (DepartmentID)
);

-- Create ProductionLine table
CREATE TABLE ProductionLine (
  ProductionLineID INT PRIMARY KEY,
  Name VARCHAR(20) NOT NULL,
  Capacity INT NOT NULL,
  ProductID INT NOT NULL,
  CONSTRAINT fk_line_product FOREIGN KEY (ProductID) REFERENCES Product (ProductID)
);

-- Relational Tables

CREATE TABLE Place_Order (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    ManufacturerID NUMBER,
    OrderDate DATE,
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID),
    FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID),
    FOREIGN KEY (ManufacturerID) REFERENCES Manufacturer(Trade_License)
);

CREATE TABLE Demand (
    CustomerID INT PRIMARY KEY,
    OrderID INT,
    CONSTRAINT fk_demand_customer FOREIGN KEY (CustomerID) REFERENCES Customer (CustomerID),
    CONSTRAINT fk_demand_order FOREIGN KEY (OrderID) REFERENCES Orders (OrderID)
);

CREATE TABLE Demand_Of (
    OrderID INT PRIMARY KEY,
    ProductID INT,
    CONSTRAINT fk_demand_of_order FOREIGN KEY (OrderID) REFERENCES Orders (OrderID),
    CONSTRAINT fk_demand_of_product FOREIGN KEY (ProductID) REFERENCES Product (ProductID)
);

CREATE TABLE Produce (
  ProductionLineID INT PRIMARY KEY,
  ProductID INT,
  CONSTRAINT fk_produce_line FOREIGN KEY (ProductionLineID) REFERENCES ProductionLine (ProductionLineID),
  CONSTRAINT fk_produce_product FOREIGN KEY (ProductID) REFERENCES Product (ProductID)
);

CREATE TABLE Has_Dept (
    DepartmentID INT PRIMARY KEY,
    ManufacturerID NUMBER,
    FOREIGN KEY (ManufacturerID) REFERENCES Manufacturer (Trade_License),
    FOREIGN KEY (DepartmentID) REFERENCES Department (DepartmentID)
);

CREATE TABLE Has_PL (
    ProductionLineID INT PRIMARY KEY,
    ManufacturerID NUMBER,
    FOREIGN KEY (ManufacturerID) REFERENCES Manufacturer (Trade_License),
    FOREIGN KEY (ProductionLineID) REFERENCES ProductionLine (ProductionLineID)
);

CREATE TABLE Works_On (
    EmployeeID INT PRIMARY KEY,
    DepartmentID INT,
    CONSTRAINT fk_works_employee FOREIGN KEY (EmployeeID) REFERENCES Employee (EmployeeID),
    CONSTRAINT fk_works_department FOREIGN KEY (DepartmentID) REFERENCES Department (DepartmentID)
);


-- Inserting demo values into the Customer table
INSERT INTO Customer (CustomerID, Name, Phone, Street, City, Division)
VALUES (1, 'Mohammad Rahman', '01711223344', '3 Main Road', 'Dhaka', 'Dhaka');

INSERT INTO Customer (CustomerID, Name, Phone, Street, City, Division)
VALUES (2, 'Fatima Begum', '01987654321', '25 Park Street', 'Chittagong', 'Chittagong');

INSERT INTO Customer (CustomerID, Name, Phone, Street, City, Division)
VALUES (3, 'Ahmed Ali', '01899887766', '8 North Avenue', 'Sylhet', 'Sylhet');

INSERT INTO Customer (CustomerID, Name, Phone, Street, City, Division)
VALUES (4, 'Aisha Khan', '01511223344', '12 West Road', 'Rajshahi', 'Rajshahi');

INSERT INTO Customer (CustomerID, Name, Phone, Street, City, Division)
VALUES (5, 'Yusuf Ahmed', '01699887766', '17 South Street', 'Khulna', 'Khulna');

INSERT INTO Customer (CustomerID, Name, Phone, Street, City, Division)
VALUES (6, 'Zainab Hasan', '01711223399', '21 East Avenue', 'Dhaka', 'Dhaka');

INSERT INTO Customer (CustomerID, Name, Phone, Street, City, Division)
VALUES (7, 'Omar Siddique', '01987659876', '7 West Street', 'Chittagong', 'Chittagong');

INSERT INTO Customer (CustomerID, Name, Phone, Street, City, Division)
VALUES (8, 'Sofia Akhtar', '01899887700', '14 North Road', 'Sylhet', 'Sylhet');

INSERT INTO Customer (CustomerID, Name, Phone, Street, City, Division)
VALUES (9, 'Ibrahim Khan', '01511223388', '32 South Avenue', 'Rajshahi', 'Rajshahi');

INSERT INTO Customer (CustomerID, Name, Phone, Street, City, Division)
VALUES (10, 'Ayesha Ali', '01699887755', '5 Park Avenue', 'Khulna', 'Khulna');


-- Inserting demo values into the Manufacturer table
INSERT INTO Manufacturer (Trade_License, Name, Address, Details)
VALUES (1, 'ABC Manufacturing', '123 Factory St, Dhaka', 'Manufactures various items.');

INSERT INTO Manufacturer (Trade_License, Name, Address, Details)
VALUES (2, 'XYZ Industries', '456 Industrial Ave, Chittagong', 'Specializes in technology products.');

INSERT INTO Manufacturer (Trade_License, Name, Address, Details)
VALUES (3, 'Smith Manufacturing', '789 Production Rd, Sylhet', 'Family-owned business with quality products.');

INSERT INTO Manufacturer (Trade_License, Name, Address, Details)
VALUES (4, 'NewCo', '101 New Way, Rajshahi', 'Innovative products for modern needs.');

INSERT INTO Manufacturer (Trade_License, Name, Address, Details)
VALUES (5, 'MegaCorp', '555 Mega Blvd, Khulna', 'Global leader in various industries.');


-- Inserting demo values into the PhoneBook_Manu table
INSERT INTO PhoneBook_Manu (Trade_License, Phone)
VALUES (1, '01711112222');

INSERT INTO PhoneBook_Manu (Trade_License, Phone)
VALUES (2, '01944445555');

INSERT INTO PhoneBook_Manu (Trade_License, Phone)
VALUES (2, '01945454545');

INSERT INTO PhoneBook_Manu (Trade_License, Phone)
VALUES (1, '01712121212');

INSERT INTO PhoneBook_Manu (Trade_License, Phone)
VALUES (3, '01877778888');

INSERT INTO PhoneBook_Manu (Trade_License, Phone)
VALUES (4, '01533334444');

INSERT INTO PhoneBook_Manu (Trade_License, Phone)
VALUES (5, '01699990000');


-- Inserting demo values into the Product table
INSERT INTO Product (ProductID, Name, Price, MFG)
VALUES (1, 'Laptop', 85000.00, TO_DATE('2023-11-01', 'YYYY-MM-DD'));

INSERT INTO Product (ProductID, Name, Price, MFG)
VALUES (2, 'Smartphone', 23999.00, TO_DATE('2023-10-15', 'YYYY-MM-DD'));

INSERT INTO Product (ProductID, Name, Price, MFG)
VALUES (3, 'Headphones', 2900.00, TO_DATE('2023-09-20', 'YYYY-MM-DD'));

INSERT INTO Product (ProductID, Name, Price, MFG)
VALUES (4, 'Smartwatch', 3999.00, TO_DATE('2023-11-05', 'YYYY-MM-DD'));

INSERT INTO Product (ProductID, Name, Price, MFG)
VALUES (5, 'Tablet', 29999.00, TO_DATE('2023-10-30', 'YYYY-MM-DD'));


-- Inserting demo values into the Orders table
-- Updated queries based on Product and Customer tables
INSERT INTO Orders (OrderID, OrderDate, CustomerID, ProductID, Quantity, Price)
VALUES (1, TO_DATE('2023-11-10', 'YYYY-MM-DD'), 1, 2, 2, (SELECT Price FROM Product WHERE Name = 'Laptop'));

INSERT INTO Orders (OrderID, OrderDate, CustomerID, ProductID, Quantity, Price)
VALUES (2, TO_DATE('2023-11-12', 'YYYY-MM-DD'), 2, 1, 1, (SELECT Price FROM Product WHERE Name = 'Headphones'));

INSERT INTO Orders (OrderID, OrderDate, CustomerID, ProductID, Quantity, Price)
VALUES (3, TO_DATE('2023-11-15', 'YYYY-MM-DD'), 3, 3, 3, (SELECT Price FROM Product WHERE Name = 'Smartphone'));

INSERT INTO Orders (OrderID, OrderDate, CustomerID, ProductID, Quantity, Price)
VALUES (4, TO_DATE('2023-11-18', 'YYYY-MM-DD'), 4, 5, 1, (SELECT Price FROM Product WHERE Name = 'Smartwatch'));

INSERT INTO Orders (OrderID, OrderDate, CustomerID, ProductID, Quantity, Price)
VALUES (5, TO_DATE('2023-11-20', 'YYYY-MM-DD'), 5, 4, 2, (SELECT Price FROM Product WHERE Name = 'Tablet'));


-- Inserting demo values into the Department table
INSERT INTO Department (DepartmentID, Name, ManagerID)
VALUES (1, 'Sales', 101);

INSERT INTO Department (DepartmentID, Name, ManagerID)
VALUES (2, 'Marketing', 102);

INSERT INTO Department (DepartmentID, Name, ManagerID)
VALUES (3, 'Human Resources', 103);

INSERT INTO Department (DepartmentID, Name, ManagerID)
VALUES (4, 'Finance', 104);

INSERT INTO Department (DepartmentID, Name, ManagerID)
VALUES (5, 'Operations', 105);


-- Inserting demo values into the Employee table
INSERT INTO Employee (EmployeeID, Name, Phone, Salary, DepartmentID)
VALUES (101, 'Tahmid Rahman', '017-1234-5678', 50000.00, 1);

INSERT INTO Employee (EmployeeID, Name, Phone, Salary, DepartmentID)
VALUES (102, 'Ayesha Islam', '019-8765-4321', 55000.00, 2);

INSERT INTO Employee (EmployeeID, Name, Phone, Salary, DepartmentID)
VALUES (103, 'Imran Khan', '018-2222-3333', 60000.00, 3);

INSERT INTO Employee (EmployeeID, Name, Phone, Salary, DepartmentID)
VALUES (104, 'Nadia Akhtar', '016-3456-7890', 52000.00, 4);

INSERT INTO Employee (EmployeeID, Name, Phone, Salary, DepartmentID)
VALUES (105, 'Rahat Hasan', '015-6789-0123', 48000.00, 5);


-- Inserting demo values into the ProductionLine table
INSERT INTO ProductionLine (ProductionLineID, Name, Capacity, ProductID)
VALUES (1, 'Line A', 100, 1);

INSERT INTO ProductionLine (ProductionLineID, Name, Capacity, ProductID)
VALUES (2, 'Line B', 150, 2);

INSERT INTO ProductionLine (ProductionLineID, Name, Capacity, ProductID)
VALUES (3, 'Line C', 120, 3);

INSERT INTO ProductionLine (ProductionLineID, Name, Capacity, ProductID)
VALUES (4, 'Line D', 200, 4);

INSERT INTO ProductionLine (ProductionLineID, Name, Capacity, ProductID)
VALUES (5, 'Line E', 180, 5);

INSERT INTO ProductionLine (ProductionLineID, Name, Capacity, ProductID)
VALUES (6, 'Line F', 180, 5);

INSERT INTO ProductionLine (ProductionLineID, Name, Capacity, ProductID)
VALUES (7, 'Line G', 200, 4);


INSERT INTO Place_Order (OrderID, CustomerID, ManufacturerID, OrderDate)
VALUES (1, 1, 1, TO_DATE('2023-11-10', 'YYYY-MM-DD'));

INSERT INTO Place_Order (OrderID, CustomerID, ManufacturerID, OrderDate)
VALUES (2, 2, 2, TO_DATE('2023-11-12', 'YYYY-MM-DD'));

INSERT INTO Place_Order (OrderID, CustomerID, ManufacturerID, OrderDate)
VALUES (3, 3, 3, TO_DATE('2023-11-15', 'YYYY-MM-DD'));


INSERT INTO Demand (CustomerID, OrderID)
VALUES (1, 1);

INSERT INTO Demand (CustomerID, OrderID)
VALUES (2, 2);

INSERT INTO Demand (CustomerID, OrderID)
VALUES (3, 3);

INSERT INTO Demand (CustomerID, OrderID)
VALUES (4, 4);

INSERT INTO Demand (CustomerID, OrderID)
VALUES (5, 5);


INSERT INTO Demand_Of (OrderID, ProductID)
VALUES (1, 1);

INSERT INTO Demand_Of (OrderID, ProductID)
VALUES (2, 2);

INSERT INTO Demand_Of (OrderID, ProductID)
VALUES (3, 3);

INSERT INTO Demand_Of (OrderID, ProductID)
VALUES (4, 4);

INSERT INTO Demand_Of (OrderID, ProductID)
VALUES (5, 5);


INSERT INTO Produce (ProductionLineID, ProductID)
VALUES (1, 1);

INSERT INTO Produce (ProductionLineID, ProductID)
VALUES (2, 2);

INSERT INTO Produce (ProductionLineID, ProductID)
VALUES (3, 3);

INSERT INTO Produce (ProductionLineID, ProductID)
VALUES (4, 4);

INSERT INTO Produce (ProductionLineID, ProductID)
VALUES (5, 5);


-- Insert demo data into Has_Dept table
INSERT INTO Has_Dept (ManufacturerID, DepartmentID) 
VALUES (1, 1);

INSERT INTO Has_Dept (ManufacturerID, DepartmentID) 
VALUES (2, 2);

INSERT INTO Has_Dept (ManufacturerID, DepartmentID) 
VALUES (3, 3);

INSERT INTO Has_Dept (ManufacturerID, DepartmentID) 
VALUES (4, 4);

INSERT INTO Has_Dept (ManufacturerID, DepartmentID) 
VALUES (5, 5);


-- Insert demo data into Has_PL table
INSERT INTO Has_PL (ManufacturerID, ProductionLineID) 
VALUES (1, 1);

INSERT INTO Has_PL (ManufacturerID, ProductionLineID) 
VALUES (2, 2);

INSERT INTO Has_PL (ManufacturerID, ProductionLineID) 
VALUES (3, 3);

INSERT INTO Has_PL (ManufacturerID, ProductionLineID) 
VALUES (4, 4);

INSERT INTO Has_PL (ManufacturerID, ProductionLineID) 
VALUES (5, 5);


-- Insert demo data into Works_On table
INSERT INTO Works_On (EmployeeID, DepartmentID) 
VALUES (101, 1);

INSERT INTO Works_On (EmployeeID, DepartmentID) 
VALUES (102, 2);

INSERT INTO Works_On (EmployeeID, DepartmentID) 
VALUES (103, 3);

INSERT INTO Works_On (EmployeeID, DepartmentID) 
VALUES (104, 4);

INSERT INTO Works_On (EmployeeID, DepartmentID) 
VALUES (105, 5);


select * from customer;
select * from Manufacturer;
select * from PhoneBook_Manu;
select * from Product; 
select * from Orders;
select * from Employee;
select * from Department;
select * from ProductionLine;

select * from Place_Order;
select c.name as Customer, m.name as Manufacturer from Customer c, Manufacturer m, Place_order po where c.CustomerID=po.CustomerID and m.Trade_License=po.ManufacturerID;


select * from Demand;
select * from Demand_Of;
select c.name as Customer, p.name as Product from Customer c, Orders o, Product p where c.CustomerID=o.CustomerID and o.ProductID= p.productID;

select * from Produce;
select p.name as Product, pl.name as ProductionLine from Product p, ProductionLine pl, Produce pc where pc.ProductID=p.ProductID and pc.ProductionLineID=pl.ProductionLineID;

select * from Has_Dept;
select m.name as Manufacturer, d.name as Department from Manufacturer m, Department d, Has_Dept hd where hd.ManufacturerID = m.Trade_License and hd.DepartmentID=d.DepartmentID;
 
select * from Has_PL;
select m.name as Manufacturer, pl.name as ProductionLine from Manufacturer m, ProductionLine pl, Has_PL hpl where hpl.ManufacturerID = m.Trade_License and hpl.ProductionLineID=pl.ProductionLineID;

select * from Works_On;
select e.name as Employee, d.name as Department from Employee e, Department d, Works_On w where w.EmployeeID = e.employeeID and w.DepartmentID=d.DepartmentID;
