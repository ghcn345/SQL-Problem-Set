<p>
<img src="images/sql.jpg" width="900" height="506">
</p>

# SQL Problem Set

## ASSESSMENT TEST 1

- 1. Return the customer IDs of customers who have spent at least 110 with the staff member who has an ID of 2.
```sql
SELECT customer_id,SUM(amount)
FROM payment
WHERE staff_id = 2
GROUP BY customer_id
HAVING SUM(amount) > 110; 
```

- 2. How many films begin with the letter J?
```sql
SELECT COUNT(*) FROM film
WHERE title LIKE 'J%';
```

- 3. What customer has the highest customer ID number whose name starts with an 'E' and has an address ID lower than 500?
```sql
SELECT first_name,last_name FROM customer
WHERE first_name LIKE 'E%' AND address_id < 500
ORDER BY customer_id DESC
LIMIT 1;
```


## ASSESSMENT TEST 2

- 1. How can you retrieve all the information from the cd.facilities table?
```sql
SELECT * FROM cd.facilities;
```


- 2. You want to print out a list of all of the facilities and their cost to members. How would you retrieve a list of only facility names and costs?
```sql
SELECT name, membercost FROM cd.facilities;
```


- 3. How can you produce a list of facilities that charge a fee to members?
```sql
SELECT * FROM cd.facilities 
WHERE membercost > 0;
```


- 4. How can you produce a list of facilities that charge a fee to members, and that fee is less than 1/50th of the monthly maintenance cost? Return the facid, facility name, member cost, and monthly maintenance of the facilities in question.
```sql
SELECT facid, name, membercost, monthlymaintenance 
FROM cd.facilities
WHERE membercost > 0 AND (membercost < monthlymaintenance/50.0);
```


- 5. How can you produce a list of all facilities with the word 'Tennis' in their name?
```sql
SELECT * FROM cd.facilities 
WHERE name LIKE '%Tennis%';
```


- 6. How can you retrieve the details of facilities with ID 1 and 5? Try to do it without using the OR operator.
```sql
SELECT * FROM cd.facilities 
WHERE facid IN (1,5);
```


- 7. How can you produce a list of members who joined after the start of September 2012? Return the memid, surname, firstname, and joindate of the members in question.
```sql
SELECT memid, surname, firstname, joindate 
FROM cd.members 
WHERE joindate >= '2012-09-01';
```


- 8. How can you produce an ordered list of the first 10 surnames in the members table? The list must not contain duplicates.
```sql
SELECT DISTINCT surname FROM cd.members
ORDER BY surname 
LIMIT 10;
```

- 9. You'd like to get the signup date of your last member. How can you retrieve this information? 
```sql
SELECT MAX(joindate) AS latest_signup FROM cd.members;
```


- 10. Produce a count of the number of facilities that have a cost to guests of 10 or more. 
```sql
SELECT COUNT(*) FROM cd.facilities 
WHERE guestcost >= 10;
```


- 11. Produce a list of the total number of slots booked per facility in the month of September 2012. Produce an output table consisting of facility id and slots, sorted by the number of slots.
```sql
SELECT facid, sum(slots) AS "Total Slots" FROM cd.bookings 
WHERE starttime >= '2012-09-01' AND starttime < '2012-10-01' 
GROUP BY facid 
ORDER BY SUM(slots);
```


- 12. Produce a list of facilities with more than 1000 slots booked. Produce an output table consisting of facility id and total slots, sorted by facility id.
```sql
SELECT facid, sum(slots) AS total_slots FROM cd.bookings 
GROUP BY facid 
HAVING SUM(slots) > 1000 
ORDER BY facid;
```
 

- 13. How can you produce a list of the start times for bookings for tennis courts, for the date '2012-09-21'? Return a list of start time and facility name pairings, ordered by the time. 
```sql
SELECT cd.bookings.starttime AS start, cd.facilities.name AS name 
FROM cd.facilities 
JOIN cd.bookings ON cd.facilities.facid = cd.bookings.facid 
WHERE cd.facilities.facid IN (0,1) AND cd.bookings.starttime >= '2012-09-21' AND cd.bookings.starttime < '2012-09-22' 
ORDER BY cd.bookings.starttime;
```


- 14. How can you produce a list of the start times for bookings by members named 'David Farrell'?
```sql
SELECT cd.bookings.starttime FROM cd.bookings 
JOIN cd.members ON cd.members.memid = cd.bookings.memid 
WHERE cd.members.firstname='David' AND cd.members.surname='Farrell';
```



## Assessment Test 3

- 1. Create a new database called "School" this database should have two tables: teachers and students. The students table should have columns for student_id, first_name,last_name, homeroom_number, phone,email, and graduation year. 
```sql
CREATE TABLE students(
student_id serial PRIMARY KEY,
first_name VARCHAR(45) NOT NULL,
last_name VARCHAR(45) NOT NULL, 
homeroom_number integer,
phone VARCHAR(20) UNIQUE NOT NULL,
email VARCHAR(115) UNIQUE,
grad_year integer);
```


- 2. The teachers table should have columns for teacher_id, first_name, last_name, homeroom_number, department, email, and phone. The constraints are mostly up to you, but your table constraints do have to consider the following: We must have a phone number to contact students in case of an emergency; We must have ids as the primary key of the tables; Phone numbers and emails must be unique to the individual. 
```sql
CREATE TABLE teachers(
teacher_id serial PRIMARY KEY,
first_name VARCHAR(45) NOT NULL,
last_name VARCHAR(45) NOT NULL, 
homeroom_number integer,
department VARCHAR(45),
email VARCHAR(20) UNIQUE,
phone VARCHAR(20) UNIQUE);
```


- 3. Once you've made the tables, insert a student named Mark Watney (student_id=1) who has a phone number of 777-555-1234 and doesn't have an email. He graduates in 2035 and has 5 as a homeroom number. 
```sql
INSERT INTO students(first_name,last_name, homeroom_number,phone,grad_year)VALUES ('Mark','Watney',5,'7755551234',2035);
```


- 4. Then insert a teacher names Jonas Salk (teacher_id = 1) who as a homeroom number of 5 and is from the Biology department. His contact info is: jsalk@school.org and a phone number of 777-555-4321.
```sql
INSERT INTO teachers(first_name,last_name, homeroom_number,department,email,phone)VALUES ('Jonas','Salk',5,'Biology','jsalk@school.org','7755554321');
```


## Assessment Test 4

- 1. Write a query that retrieves suppliers that work in either Georgia or California.
```sql
SELECT * FROM suppliers
WHERE state = 'Georgia' OR state = 'California';
```

- 2. Write a query that retrieves suppliers with the characters "wo" and the character "I" or "i" in their name.
```sql
SELECT * FROM suppliers
WHERE supplier_name like '%wo%' AND (supplier_name like '%i%' OR supplier_name like '%I%');
```

- 3. Write a query that retrieves suppliers on which a minimum of 37,000 and a maximum of 80,000 was spent. You may also use the BETWEEN operator to solve this problem.
```sql
SELECT * FROM suppliers
WHERE total_spent >= 37000 AND total_spent <= 80000;
```

- 4. Write a query that returns the supplier names and the state in which they operate meeting the following conditions: belong in the state Georgia or Alaska, the supplier id is 100 or greater than 600, the amount spent is less than 100,000 or the amount spent is 220,000.
```sql
SELECT supplier_name, state FROM suppliers
WHERE state IN ('Georgia', 'Alaska') AND (supplier_id = 100 OR supplier_id > 600) AND (total_spent < 100000 OR total_spent = 220000);
```




## For More Information

For any additional questions, please contact **Ning Chen—chen.ning345@gmail.com**.

## Repository Structure

Description of the structure of the repository and its contents:
```
├── README.md                           <- Solution of SQL Problem Set
└── data                                <- SQL database
└── images                              <- logo
```