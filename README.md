<p>
<img src="images/sql.jpg" width="900" height="500">
</p>

# SQL Problem Set

## ASSESSMENT TEST 1

- 1. Return the customer IDs of customers who have spent at least 110 with the staff member who has an ID of 2.

Queries: 
```sql
SELECT customer_id,SUM(amount)
FROM payment
WHERE staff_id = 2
GROUP BY customer_id
HAVING SUM(amount) > 110; 
```

- 2. How many films begin with the letter J?


Queries: 
```sql
SELECT COUNT(*) FROM film
WHERE title LIKE 'J%';
```

- 3. What customer has the highest customer ID number whose name starts with an 'E' and has an address ID lower than 500?

Queries: 
```sql
SELECT first_name,last_name FROM customer
WHERE first_name LIKE 'E%'
AND address_id <500
ORDER BY customer_id DESC
LIMIT 1;
```


## ASSESSMENT TEST 2

- 1. How can you retrieve all the information from the cd.facilities table?

Queries: 
```sql
SELECT * FROM cd.facilities;
```


- 2. You want to print out a list of all of the facilities and their cost to members. How would you retrieve a list of only facility names and costs?

Queries: 
```sql
SELECT name, membercost FROM cd.facilities;



- 3. How can you produce a list of facilities that charge a fee to members?

Queries: 
```sql
SELECT * FROM cd.facilities WHERE membercost > 0;
```










## For More Information

For any additional questions, please contact **Ning Chen—chen.ning345@gmail.com**.

## Repository Structure

Description of the structure of the repository and its contents:
```
├── README.md                           <- Solution of SQL Problem Set
└── images                              <- logo
```