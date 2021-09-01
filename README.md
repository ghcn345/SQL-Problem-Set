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
WHERE first_name LIKE 'E%' AND address_id < 500
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
```


- 3. How can you produce a list of facilities that charge a fee to members?
Queries: 
```sql
SELECT * FROM cd.facilities 
WHERE membercost > 0;
```


- 4. How can you produce a list of facilities that charge a fee to members, and that fee is less than 1/50th of the monthly maintenance cost? Return the facid, facility name, member cost, and monthly maintenance of the facilities in question.
Queries: 
```sql
SELECT facid, name, membercost, monthlymaintenance 
FROM cd.facilities
WHERE membercost > 0 AND (membercost < monthlymaintenance/50.0);
```


- 5. How can you produce a list of all facilities with the word 'Tennis' in their name?
Queries: 
```sql
SELECT * FROM cd.facilities 
WHERE name LIKE '%Tennis%';
```


- 6. How can you retrieve the details of facilities with ID 1 and 5? Try to do it without using the OR operator.
Queries: 
```sql
SELECT * FROM cd.facilities 
WHERE facid IN (1,5);
```


- 7. How can you produce a list of members who joined after the start of September 2012? Return the memid, surname, firstname, and joindate of the members in question.
Queries: 
```sql
SELECT memid, surname, firstname, joindate 
FROM cd.members 
WHERE joindate >= '2012-09-01';
```


- 8. How can you produce an ordered list of the first 10 surnames in the members table? The list must not contain duplicates.
Queries: 
```sql
SELECT DISTINCT surname FROM cd.members
ORDER BY surname 
LIMIT 10;
```

- 9. You'd like to get the signup date of your last member. How can you retrieve this information?
Queries: 
```sql
SELECT MAX(joindate) AS latest_signup FROM cd.members;
```


- 10. Produce a count of the number of facilities that have a cost to guests of 10 or more.
Queries: 
```sql
SELECT COUNT(*) FROM cd.facilities 
WHERE guestcost >= 10;
```


- 11. Produce a list of the total number of slots booked per facility in the month of September 2012. Produce an output table consisting of facility id and slots, sorted by the number of slots.
Queries: 
```sql
SELECT facid, sum(slots) AS "Total Slots" FROM cd.bookings 
WHERE starttime >= '2012-09-01' AND starttime < '2012-10-01' 
GROUP BY facid 
ORDER BY SUM(slots);
```


- 12. Produce a list of facilities with more than 1000 slots booked. Produce an output table consisting of facility id and total slots, sorted by facility id.
Queries: 
```sql
SELECT facid, sum(slots) AS total_slots FROM cd.bookings 
GROUP BY facid 
HAVING SUM(slots) > 1000 
ORDER BY facid;
```
 

- 13. How can you produce a list of the start times for bookings for tennis courts, for the date '2012-09-21'? Return a list of start time and facility name pairings, ordered by the time.
Queries: 
```sql
SELECT cd.bookings.starttime AS start, cd.facilities.name AS name 
FROM cd.facilities 
JOIN cd.bookings ON cd.facilities.facid = cd.bookings.facid 
WHERE cd.facilities.facid IN (0,1) AND cd.bookings.starttime >= '2012-09-21' AND cd.bookings.starttime < '2012-09-22' 
ORDER BY cd.bookings.starttime;
```


- 14. How can you produce a list of the start times for bookings by members named 'David Farrell'?
Queries: 
```sql
SELECT cd.bookings.starttime FROM cd.bookings 
JOIN cd.members ON cd.members.memid = cd.bookings.memid 
WHERE cd.members.firstname='David' AND cd.members.surname='Farrell';
```









## For More Information

For any additional questions, please contact **Ning Chen—chen.ning345@gmail.com**.

## Repository Structure

Description of the structure of the repository and its contents:
```
├── README.md                           <- Solution of SQL Problem Set
└── images                              <- logo
```