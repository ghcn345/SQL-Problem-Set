<p>
<img src="images/sql.jpg" width="900" height="500">
</p>

# SQL Problem Set




## Question 1
For this question you’ll need to use SQL. [Follow this link](https://www.w3schools.com/SQL/TRYSQL.ASP?FILENAME=TRYSQL_SELECT_ALL) to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below.

- a. How many orders were shipped by Speedy Express in total?

Queries: 
```sql
SELECT SUM(od.Quantity) FROM OrderDetails od 
JOIN ORDERS o USING(OrderID) 
WHERE o.ShipperID = 1 
```


- b. What is the last name of the employee with the most orders?

Queries: 
```sql
SELECT e.Lastname, SUM(od.Quantity) as total FROM OrderDetails od 
JOIN ORDERS o USING(OrderID) 
JOIN Employees e USING(EmployeeID) 
GROUP BY EmployeeID 
ORDER BY total DESC 
LIMIT 1
```


- c. What product was ordered the most by customers in Germany?

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
Result: Dairy Products

## For More Information

For any additional questions, please contact **Ning Chen—chen.ning345@gmail.com**.

## Repository Structure

Description of the structure of the repository and its contents:
```
├── README.md                           <- Solution of SQL Problem Set
└── images                              <- logo
```