### Customers Tablosu ###

```
CREATE TABLE Customers (
CustomerID INT PRIMARY KEY,
[Customer Name] VARCHAR(50),
Email VARCHAR(50),
[Phone Number] VARCHAR(20)
)


INSERT INTO Customers VALUES
(1, 'Ali Yılmaz', 'ali@gmail.com', '5551234567'),
(2, 'Gözde Gültekin', 'gozde@gmail.com', '5557654321'),
(3, 'Havva Baltacıoğlu', 'havva@gmail.com', '5559876543'),
(4, 'Mihriban Kantaroğlu', 'mihriban@gmail.com', '5551234567'),
(5, 'Timurhan Gültekin', 'timurhan@gmail.com', '5551112233'),
(6, 'Esma Ülker', 'esma@gmail.com', '5553332211'),
(7, 'Ayşenur Dalçık', 'aysenur@gmail.com', '5555555555'),
(8, 'Jülide Başoğlu', 'julide@gmail.com', '5551110000'),
(9, 'Furkan Dündar', 'furkan@gmail.com', '5552223344'),
(10, 'Bedirhan  Kol', 'bedirhan@gmail.com', '5559998877')


SELECT * FROM Customers
```
![image](https://github.com/user-attachments/assets/c155af39-0716-452c-9c94-a743f848d277)


### Products Tablosu ###

```
CREATE TABLE Products (
ProductID INT PRIMARY KEY,
[Product Name] VARCHAR(50),
Price DECIMAL (10,2)
)


INSERT INTO Products VALUES
(1, 'Laptop', 15000.00),
(2, 'Tablet', 8000.00),
(3, 'Mobile Phone', 10000.00),
(4, 'Headphone', 1500.00),
(5, 'Keyboard', 1000.00)


SELECT * FROM Products
```

![image](https://github.com/user-attachments/assets/55b8b0b5-659e-48ae-aeae-98c1fd74132e)


### Orders Tablosu ###

```
CREATE TABLE Orders (
OrderID INT PRIMARY KEY,
CustomerID INT, 
ProductID INT,
[Order Date] DATE,
Quantity INT,
FOREIGN KEY (CustomerID)
REFERENCES  Customers(CustomerID),
FOREIGN  KEY (ProductID)
REFERENCES Products(ProductID)
)



INSERT INTO Orders VALUES
(1, 1, 1, '2024-12-01', 1), -- Ali Yılmaz 1 Laptop aldı
(2, 2, 2, '2024-12-02', 2), -- Gözde Gültekin 2 Tablet aldı
(3, 3, 3, '2024-12-03', 1), -- Havva Baltacıoğlu 1 Mobile Phone aldı
(4, 4, 4, '2024-12-04', 3), -- Mihriban Kantaroğlu 3 Headphone aldı
(5, 5, 5, '2024-12-05', 1), -- Timurhan Gültekin 1 Keyboard aldı
(6, 6, 1, '2024-12-06', 1), -- Esma Ülker 1 Laptop aldı
(7, 7, 2, '2024-12-07', 1), -- Ayşenur Dalçık 1 Tablet aldı
(8, 8, 4, '2024-12-08', 4), -- Jülide Başoğlu 4 Headphone aldı
(9, 9, 3, '2024-12-09', 1), -- Furkan Dündar 1 Mobile Phone aldı
(10, 10, 5, '2024-12-10', 2); -- Bedirhan Kol 2 Keyboard aldı


SELECT * FROM Orders
```
![image](https://github.com/user-attachments/assets/fc77a3c5-5a5c-42e8-8946-976d1fca6c94)


### Categories Tablosu ###

```
CREATE TABLE Categories(
CategoryID INT PRIMARY KEY,
[Category Name] VARCHAR(50)
)

INSERT INTO Categories VALUES
(1, 'Electronic'),
(2, 'Accessory'),
(3, 'Computer')


SELECT * FROM Categories
```

![image](https://github.com/user-attachments/assets/84f9d2cc-ca23-4d95-8609-f837f654837d)

### Product Categories Tablosu ###

```
CREATE TABLE [Product Categories] (
ProductID INT,
CategoryID INT,
PRIMARY KEY (ProductID, CategoryID),
FOREIGN KEY (ProductID) REFERENCES Products(ProductID),
FOREIGN KEY (CategoryID) REFERENCES Categories(CategoryID)
)

INSERT INTO [Product Categories] VALUES
(1,3),
(2,1),
(3,1),
(4,2),
(5,2)

SELECT * FROM [Product Categories]
```
![image](https://github.com/user-attachments/assets/72fa9363-236a-4ea7-b22c-1fd86e0e9fde)


### Müşterilerin Verdiği Sipariş Listesi ###

```
SELECT C.[Customer Name],
P.[Product Name],
O.Quantity,
O.[Order Date]
FROM Orders O
INNER JOIN Customers C
ON O.CustomerID = C.CustomerID
INNER JOIN Products P
ON  O.ProductID = P.ProductID
```

![image](https://github.com/user-attachments/assets/261e1f7b-0985-4dc0-8d36-ee36c491acc3)


### Kategori Bilgisi Olmayan Ürün Mevcut mu? ###

```
SELECT P.[Product Name], C.[Category Name] FROM Products AS P 
LEFT JOIN [Product Categories] PC 
ON P.ProductID=PC.ProductID
LEFT JOIN Categories AS C ON
PC.CategoryID= C.CategoryID
WHERE C.CategoryID IS NULL
```

Kategori bilgisi olmayan bir ürün bulunmamaktadır.

![image](https://github.com/user-attachments/assets/844e6c6a-7b44-471f-ac7f-8461ca49b59f)



### Toplam Sipariş Tutarı Listeleme ###


```
SELECT [Customer Name], SUM(Price*Quantity) AS [Total Order Value] FROM Orders O
INNER JOIN Customers C ON
O.CustomerID = C.CustomerID
INNER JOIN  Products P ON O.ProductID = P.ProductID
GROUP BY [Customer Name]
```

![image](https://github.com/user-attachments/assets/42be5108-5843-40c9-bd00-0c64f27d78bf)


### En Çok Sipariş Edilen Ürün ###

```
SELECT [Product Name],
SUM (Quantity) AS [Total Quantity]
FROM Orders O
INNER JOIN Products P ON  O.ProductID= P.ProductID
GROUP BY [Product Name] ORDER BY [Total Quantity] DESC -- En çok Headphone (kulaklık) alınmış
```

![image](https://github.com/user-attachments/assets/c83fe3f1-af64-44bc-b37f-532269082447)


### En Çok Sipariş Veren Müşteri ###

```
SELECT [Customer Name], SUM(Quantity) AS [Total Quantity Ordered]
FROM Orders O 
INNER JOIN Customers C ON
O.CustomerID= C.[CustomerID]
GROUP BY [Customer Name] ORDER BY [Total Quantity Ordered] DESC -- En çok sipariş veren müşteri Jülide Başoğlu 4 sipariş vermiş
```

![image](https://github.com/user-attachments/assets/cba72c7a-6097-46c4-893a-1e1a8b7a0743)


### Eskiden Yeniye Sipariş Tarihine Göre Satışları Listeleme ###

```
SELECT OrderID, [Order Date], [Product Name], Quantity
FROM Orders O 
INNER JOIN Products P ON O.ProductID= P.ProductID
ORDER BY [Order Date] ASC  --2024-12-01 tarihinde 1 adet laptop satılmış 2024-12-10 tarihinde 2 adet keyboard(klavye) satılmış.
```

![image](https://github.com/user-attachments/assets/4a532f2d-ee6b-4b4e-9789-470b636bb288)



### Hiç Sipariş Vermeyen Müşteri Mevcut mu? ###

```
SELECT [Customer Name], Email
FROM Customers C
LEFT JOIN Orders O ON C.CustomerID=O.CustomerID
WHERE O.CustomerID IS NULL
```

Hiç sipariş vermeyen müşteri bulunmamaktadır.

![image](https://github.com/user-attachments/assets/e39f585c-f254-4f76-9e3f-9685b32d56b1) 



### Hiç Satılmamış Ürün Mevcut mu? ###

```
SELECT [Product Name], Price 
FROM Products P 
LEFT JOIN Orders O ON P.ProductID = O.ProductID
WHERE P.ProductID IS NULL
```

Hiç satılmayan ürün bulunmamaktadır.

![image](https://github.com/user-attachments/assets/4b7c1419-4b67-427e-86a4-67bcbb6e381f) 


### Müşteri ve Ürün Bazında Sipariş Detayları ###

```
SELECT [Customer Name], [Product Name], SUM(Quantity) AS [Total Quantity Ordered]
FROM Orders  O
INNER JOIN Customers C 
ON O.CustomerID = C.CustomerID 
INNER JOIN  Products P 
ON O.ProductID = P.ProductID
GROUP BY [Customer Name], [Product Name]
ORDER BY [Customer Name], [Product Name]
```

![image](https://github.com/user-attachments/assets/7234ce90-7913-4b2e-8100-73d9ebfa1bf8) 


### Son 7 Gündeki Siparişler ###

```
SELECT [Customer Name], [Product Name], Quantity, [Order Date]
FROM Orders O
INNER JOIN Customers C 
ON O.CustomerID = C.CustomerID
INNER JOIN Products P 
ON O.ProductID= P.ProductID
WHERE O.[Order Date] >= DATEADD(DAY, -7, '20241210') ORDER BY  [Order Date] DESC
```

![image](https://github.com/user-attachments/assets/461f0d1d-02a7-4697-9b45-9b54f1f49f28)
