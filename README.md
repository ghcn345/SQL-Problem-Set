<p>
<img src="images/sql.jpg" width="900" height="500">
</p>

# SQL Problem Set


- 1. 

Queries: 
```sql
SELECT SUM(od.Quantity) FROM OrderDetails od 
JOIN ORDERS o USING(OrderID) 
WHERE o.ShipperID = 1 
```

- 2. 


Queries: 
```sql
SELECT e.Lastname, SUM(od.Quantity) as total FROM OrderDetails od 
JOIN ORDERS o USING(OrderID) 
JOIN Employees e USING(EmployeeID) 
GROUP BY EmployeeID 
ORDER BY total DESC 
LIMIT 1
```


- 3. 

Queries: 
```sql
SELECT c.CategoryName, COUNT(c.CategoryName) as total FROM Categories c 
JOIN Products p USING(CategoryID) 
JOIN OrderDetails od USING(ProductID) 
JOIN Orders o USING(OrderID) 
JOIN Customers cu USING(CustomerID) 
WHERE cu.Country = 'Germany' 
GROUP BY CategoryID 
ORDER BY total DESC 
LIMIT 1
```


## For More Information

For any additional questions, please contact **Ning Chen—chen.ning345@gmail.com**.

## Repository Structure

Description of the structure of the repository and its contents:
```
├── README.md                           <- Solution of SQL Problem Set
└── images                              <- logo
```