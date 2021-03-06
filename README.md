<p>
<img src="images/sql.jpg" width="900" height="506">
</p>

# SQL Problem Set

## ASSESSMENT TEST 1

- 1. Return the customer IDs of customers who have spent at least 110 with the staff member who has an ID of 2.
```mysql
SELECT customer_id,SUM(amount)
FROM payment
WHERE staff_id = 2
GROUP BY customer_id
HAVING SUM(amount) > 110; 
```

- 2. How many films begin with the letter J?
```mysql
SELECT COUNT(*) FROM film
WHERE title LIKE 'J%';
```

- 3. What customer has the highest customer ID number whose name starts with an 'E' and has an address ID lower than 500?
```mysql
SELECT first_name,last_name FROM customer
WHERE first_name LIKE 'E%' AND address_id < 500
ORDER BY customer_id DESC
LIMIT 1;
```


## ASSESSMENT TEST 2

- 1. How can you retrieve all the information from the cd.facilities table?
```mysql
SELECT * FROM cd.facilities;
```


- 2. You want to print out a list of all of the facilities and their cost to members. How would you retrieve a list of only facility names and costs?
```mysql
SELECT name, membercost FROM cd.facilities;
```


- 3. How can you produce a list of facilities that charge a fee to members?
```mysql
SELECT * FROM cd.facilities 
WHERE membercost > 0;
```


- 4. How can you produce a list of facilities that charge a fee to members, and that fee is less than 1/50th of the monthly maintenance cost? Return the facid, facility name, member cost, and monthly maintenance of the facilities in question.
```mysql
SELECT facid, name, membercost, monthlymaintenance 
FROM cd.facilities
WHERE membercost > 0 AND (membercost < monthlymaintenance/50.0);
```


- 5. How can you produce a list of all facilities with the word 'Tennis' in their name?
```mysql
SELECT * FROM cd.facilities 
WHERE name LIKE '%Tennis%';
```


- 6. How can you retrieve the details of facilities with ID 1 and 5? Try to do it without using the OR operator.
```mysql
SELECT * FROM cd.facilities 
WHERE facid IN (1,5);
```


- 7. How can you produce a list of members who joined after the start of September 2012? Return the memid, surname, firstname, and joindate of the members in question.
```mysql
SELECT memid, surname, firstname, joindate 
FROM cd.members 
WHERE joindate >= '2012-09-01';
```


- 8. How can you produce an ordered list of the first 10 surnames in the members table? The list must not contain duplicates.
```mysql
SELECT DISTINCT surname FROM cd.members
ORDER BY surname 
LIMIT 10;
```

- 9. You'd like to get the signup date of your last member. How can you retrieve this information? 
```mysql
SELECT MAX(joindate) AS latest_signup FROM cd.members;
```


- 10. Produce a count of the number of facilities that have a cost to guests of 10 or more. 
```mysql
SELECT COUNT(*) FROM cd.facilities 
WHERE guestcost >= 10;
```


- 11. Produce a list of the total number of slots booked per facility in the month of September 2012. Produce an output table consisting of facility id and slots, sorted by the number of slots.
```mysql
SELECT facid, sum(slots) AS "Total Slots" FROM cd.bookings 
WHERE starttime >= '2012-09-01' AND starttime < '2012-10-01' 
GROUP BY facid 
ORDER BY SUM(slots);
```


- 12. Produce a list of facilities with more than 1000 slots booked. Produce an output table consisting of facility id and total slots, sorted by facility id.
```mysql
SELECT facid, sum(slots) AS total_slots FROM cd.bookings 
GROUP BY facid 
HAVING SUM(slots) > 1000 
ORDER BY facid;
```
 

- 13. How can you produce a list of the start times for bookings for tennis courts, for the date '2012-09-21'? Return a list of start time and facility name pairings, ordered by the time. 
```mysql
SELECT cd.bookings.starttime AS start, cd.facilities.name AS name 
FROM cd.facilities 
JOIN cd.bookings ON cd.facilities.facid = cd.bookings.facid 
WHERE cd.facilities.facid IN (0,1) AND cd.bookings.starttime >= '2012-09-21' AND cd.bookings.starttime < '2012-09-22' 
ORDER BY cd.bookings.starttime;
```


- 14. How can you produce a list of the start times for bookings by members named 'David Farrell'?
```mysql
SELECT cd.bookings.starttime FROM cd.bookings 
JOIN cd.members ON cd.members.memid = cd.bookings.memid 
WHERE cd.members.firstname='David' AND cd.members.surname='Farrell';
```



## Assessment Test 3

- 1. Create a new database called "School" this database should have two tables: teachers and students. The students table should have columns for student_id, first_name,last_name, homeroom_number, phone,email, and graduation year. 
```mysql
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
```mysql
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
```mysql
INSERT INTO students(first_name,last_name, homeroom_number,phone,grad_year)VALUES ('Mark','Watney',5,'7755551234',2035);
```


- 4. Then insert a teacher names Jonas Salk (teacher_id = 1) who as a homeroom number of 5 and is from the Biology department. His contact info is: jsalk@school.org and a phone number of 777-555-4321.
```mysql
INSERT INTO teachers(first_name,last_name, homeroom_number,department,email,phone)VALUES ('Jonas','Salk',5,'Biology','jsalk@school.org','7755554321');
```


## Assessment Test 4

- 1. Write a query that retrieves suppliers that work in either Georgia or California.
```mysql
SELECT * FROM suppliers
WHERE state = 'Georgia' OR state = 'California';
```

- 2. Write a query that retrieves suppliers with the characters "wo" and the character "I" or "i" in their name.
```mysql
SELECT * FROM suppliers
WHERE supplier_name like '%wo%' AND (supplier_name like '%i%' OR supplier_name like '%I%');
```

- 3. Write a query that retrieves suppliers on which a minimum of 37,000 and a maximum of 80,000 was spent. You may also use the BETWEEN operator to solve this problem.
```mysql
SELECT * FROM suppliers
WHERE total_spent >= 37000 AND total_spent <= 80000;
```

- 4. Write a query that returns the supplier names and the state in which they operate meeting the following conditions: belong in the state Georgia or Alaska, the supplier id is 100 or greater than 600, the amount spent is less than 100,000 or the amount spent is 220,000.
```mysql
SELECT supplier_name, state FROM suppliers
WHERE state IN ('Georgia', 'Alaska') AND (supplier_id = 100 OR supplier_id > 600) AND (total_spent < 100000 OR total_spent = 220000);
```


## Assessment Test 5

- 1. Considering the data exists in the city table, write a query that will return records similar to what is shown below for those cities that have the COUNTRYCODE of 'cbd' : "NEW YORK CITY has the population of 8,500,000", "LOS ANGELES has the population of 632,000". Note: I'd like you to use functions in the SELECT statement to solve this problem.
```mysql
SELECT CONCAT(CONCAT(UPPER(name), ' has the population of '), population)
FROM city
WHERE LOWER(countrycode) = 'cbd';
```


- 2. Write a query that would show the first three letters and the last three letters of the DISTRICT capitalized and separated by a dash. Note: I'd like you to use functions in the SELECT statement to solve this problem.
```mysql
SELECT CONCAT(CONCAT(UPPER(SUBSTR(district, 1, 3)), ' - '), UPPER(SUBSTR(district, LENGTH(district)-2)))
FROM city;
```

## Assessment Test 6

- 1. [Type of Triangle](https://www.hackerrank.com/challenges/what-type-of-triangle/problem?isFullScreen=false): Write a query identifying the type of each record in the TRIANGLES table using its three side lengths. Output one of the following statements for each record in the table: Equilateral: It's a triangle with  sides of equal length. Isosceles: It's a triangle with  sides of equal length. Scalene: It's a triangle with  sides of differing lengths. Not A Triangle: The given values of A, B, and C don't form a triangle.
```mysql
SELECT CASE
    WHEN 2*GREATEST(A, B, C) >= (A+B+C) THEN "Not A Triangle"
    WHEN A = B AND A = C THEN "Equilateral"
    WHEN A = B OR A = C OR B = C THEN "Isosceles"
    ELSE "Scalene"
    END
FROM TRIANGLES
```


- 2. [Weather Observation Station](https://www.hackerrank.com/challenges/weather-observation-station-5/problem?isFullScreen=false): Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths (i.e.: number of characters in the name). If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically. 
```mysql
(SELECT city, LENGTH(city)
FROM  station
ORDER BY LENGTH(city), city
LIMIT 1)
UNION
(SELECT city, LENGTH(city)
FROM  station
ORDER BY LENGTH(city) DESC, city
LIMIT 1)
```



## Assessment Test 7


- 1. [The PADS](https://www.hackerrank.com/challenges/the-pads/problem?isFullScreen=false): Generate the following two result sets: Query an alphabetically ordered list of all names in OCCUPATIONS, immediately followed by the first letter of each profession as a parenthetical (i.e.: enclosed in parentheses). For example: AnActorName(A), ADoctorName(D), AProfessorName(P), and ASingerName(S). Query the number of ocurrences of each occupation in OCCUPATIONS. Sort the occurrences in ascending order, and output them in the following format: There are a total of occupation_count occupations. where occupation_count is the number of occurrences of an occupation in OCCUPATIONS and [occupation] is the lowercase occupation name. If more than one Occupation has the same occupation_count, they should be ordered alphabetically. Note: There will be at least two entries in the table for each type of occupation.
```mysql
SELECT CONCAT(name,'(', LEFT(occupation, 1),')') AS name
FROM occupations
ORDER BY name;
```
```mysql
SELECT CONCAT('There are a total of ', COUNT(occupation), ' ', LOWER(occupation), 's.') AS total
FROM occupations
GROUP BY occupation
ORDER BY total;
```





- 2. [Occupations](https://www.hackerrank.com/challenges/occupations/problem?isFullScreen=false): Pivot the Occupation column in OCCUPATIONS so that each Name is sorted alphabetically and displayed underneath its corresponding Occupation. The output column headers should be Doctor, Professor, Singer, and Actor, respectively. Note: Print NULL when there are no more names corresponding to an occupation.
```mysql
SELECT MIN(doctor), MIN(professor), MIN(singer), MIN(actor)
FROM (
    SELECT ROW_NUMBER() OVER (PARTITION BY occupation ORDER BY name) AS rn,  
    IF(occupation = 'Doctor', name, NULL) doctor,
    IF(occupation = 'Professor', name, NULL) professor,
    IF(occupation = 'Singer', name, NULL) singer,
    IF(occupation = 'Actor', name, NULL) actor
    FROM occupations) temp
GROUP BY rn;
```
OR
```mysql
SET @r1=0, @r2=0, @r3=0, @r4=0;

SELECT MIN(doctor), MIN(professor), MIN(singer), MIN(actor)
FROM (SELECT 
    CASE WHEN OCCUPATION = 'Doctor' THEN @r1:=@r1+1
         WHEN OCCUPATION = 'Professor' THEN @r2:=@r2+1
         WHEN OCCUPATION = 'Singer' THEN @r3:=@r3+1
         WHEN OCCUPATION = 'Actor' THEN @r4:=@r4+1 
         END AS rn,
    CASE WHEN OCCUPATION = 'Doctor' THEN Name END AS doctor,
    CASE WHEN OCCUPATION = 'Professor' THEN Name END AS professor,
    CASE WHEN OCCUPATION = 'Singer' THEN Name END AS singer,
    CASE WHEN OCCUPATION = 'Actor' THEN Name END AS actor
    FROM OCCUPATIONS
    ORDER BY Name) as temp
GROUP BY rn;
```

- 3. [Binary Tree Nodes](https://www.hackerrank.com/challenges/binary-search-tree-1/problem?isFullScreen=false): You are given a table, BST, containing two columns: N and P, where N represents the value of a node in Binary Tree, and P is the parent of N. Write a query to find the node type of Binary Tree ordered by the value of the node. Output one of the following for each node. Root: If node is root node. Leaf: If node is leaf node. Inner: If node is neither root nor leaf node.
```mysql
SELECT N, CASE
    WHEN P IS NULL THEN ' Root'
    WHEN N IN (SELECT P FROM BST) THEN 'Inner'
    ELSE 'Leaf'
    END
FROM BST
ORDER BY N;
```
OR
```mysql
SELECT N, IF(P IS NULL, 'Root', IF((SELECT COUNT(*) FROM BST WHERE P=B.N)>0, 'Inner', 'Leaf')) FROM BST AS B
ORDER BY N;
```


- 4. [New Companies](https://www.hackerrank.com/challenges/the-company/problem?isFullScreen=false): Amber's conglomerate corporation just acquired some new companies. Given the table schemas below, write a query to print the company_code, founder name, total number of lead managers, total number of senior managers, total number of managers, and total number of employees. Order your output by ascending company_code.
```mysql
SELECT c.company_code, c.founder, COUNT(DISTINCT l.lead_manager_code), COUNT(DISTINCT s.senior_manager_code), COUNT(DISTINCT m.manager_code), COUNT(DISTINCT e.employee_code)
FROM Company c, Lead_Manager l, Senior_Manager s, Manager m, Employee e
WHERE c.company_code = l.company_code
  AND l.lead_manager_code = s.lead_manager_code
  AND s.senior_manager_code = m.senior_manager_code
  AND m.manager_code = e.manager_code
GROUP BY c.company_code, c.founder
ORDER BY c.company_code;
```


- 5. [Weather Observation Station](https://www.hackerrank.com/challenges/weather-observation-station-20/problem?isFullScreen=false): A median is defined as a number separating the higher half of a data set from the lower half. Query the median of the Northern Latitudes (LAT_N) from STATION and round your answer to 4 decimal places.
```mysql
SELECT ROUND(S.Lat_N, 4)
FROM Station S 
WHERE (SELECT COUNT(Lat_N) FROM Station WHERE Lat_N < S.Lat_N ) = (SELECT COUNT(Lat_N) FROM Station WHERE Lat_N > S.Lat_N);
```
OR
```mysql
SET @N := 0;
SELECT COUNT(*) FROM Station INTO @total;

SELECT ROUND(AVG(S.Lat_N), 4)
FROM (SELECT @N := @N+1 AS row_id, Lat_N FROM Station ORDER BY Lat_N) S
WHERE CASE WHEN @total%2 = 0 THEN S.row_id IN (@total/2, @total/2+1)
           ELSE S.row_id = (@total+1)/2
           END;
```
OR
```mysql
SET @N := -1;

SELECT ROUND(AVG(Lat_N), 4)
FROM (SELECT @N := @N+1 AS row_id, Lat_N FROM Station ORDER BY Lat_N) S
WHERE S.row_id IN (FLOOR(@N/2), CEIL(@N/2));
```




- 6. [The Report](https://www.hackerrank.com/challenges/the-report/problem?isFullScreen=false): You are given two tables: Students and Grades. Students contains three columns ID, Name and Marks. Ketty gives Eve a task to generate a report containing three columns: Name, Grade and Mark. Ketty doesn't want the NAMES of those students who received a grade lower than 8. The report must be in descending order by grade -- i.e. higher grades are entered first. If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically. Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in descending order. If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order. Write a query to help Eve.
```mysql
SELECT IF(Grade > 7, Name, NULL), Grade, Marks
FROM Students 
JOIN Grades
WHERE Marks BETWEEN Min_Mark AND Max_Mark
ORDER BY Grade DESC, Name;
```
OR
```mysql
SELECT CASE WHEN G.Grade > 7 THEN S.Name ELSE 'NULL' END AS Name, G.Grade, S.Marks
FROM Students S
JOIN Grades G 
ON S.Marks BETWEEN G.Min_Mark AND G.Max_Mark
ORDER BY G.Grade DESC, Name;
```


- 7. [Top Competitors](https://www.hackerrank.com/challenges/full-score/problem?isFullScreen=false): Julia just finished conducting a coding contest, and she needs your help assembling the leaderboard! Write a query to print the respective hacker_id and name of hackers who achieved full scores for more than one challenge. Order your output in descending order by the total number of challenges in which the hacker earned a full score. If more than one hacker received full scores in same number of challenges, then sort them by ascending hacker_id.
```mysql
SELECT h.hacker_id, h.name
FROM submissions s
JOIN challenges c USING(challenge_id)
JOIN difficulty d USING(difficulty_level)
JOIN hackers h on s.hacker_id = h.hacker_id
WHERE s.score = d.score
GROUP BY h.hacker_id, h.name
HAVING COUNT(h.hacker_id) > 1
ORDER BY COUNT(h.hacker_id) DESC, h.hacker_id;
```



- 8. [Ollivander's Inventory](https://www.hackerrank.com/challenges/harry-potter-and-wands/problem?h_r=next-challenge&h_v=zen): Harry Potter and his friends are at Ollivander's with Ron, finally replacing Charlie's old broken wand. Hermione decides the best way to choose is by determining the minimum number of gold galleons needed to buy each non-evil wand of high power and age. Write a query to print the id, age, coins_needed, and power of the wands that Ron's interested in, sorted in order of descending power. If more than one wand has same power, sort the result in order of descending age.
```mysql
SELECT w.id, p.age, w.coins_needed, w.power 
FROM Wands w 
JOIN Wands_Property p USING(code)
WHERE p.is_evil = 0 AND w.coins_needed = (SELECT MIN(coins_needed)
                                          FROM Wands ws
                                          JOIN Wands_Property wp USING(code)
                                          WHERE ws.power = w.power AND wp.age = p.age)
ORDER BY w.power DESC, p.age DESC;
```
OR
```mysql
SELECT id, age, coins_needed, power
FROM (
    SELECT id, age, coins_needed, power, ROW_NUMBER() OVER (PARTITION BY power, age ORDER BY coins_needed) AS row_num
    FROM Wands w
    JOIN Wands_Property p ON w.code = p.code
    WHERE is_evil = 0) wp
WHERE row_num = 1
ORDER BY power DESC, age DESC;
```
OR
```mysql
SELECT id, age, coins_needed, power
FROM (
    SELECT id, age, coins_needed, power, MIN(coins_needed) OVER (PARTITION BY power, age ORDER BY coins_needed) AS min_coins 
    FROM Wands w 
    JOIN Wands_Property p ON w.code=p.code
    WHERE is_evil = 0) wp
WHERE coins_needed = min_coins 
ORDER BY power DESC, age DESC;
```






- 9. [Challenges](https://www.hackerrank.com/challenges/challenges/problem?isFullScreen=false): Julia asked her students to create some coding challenges. Write a query to print the hacker_id, name, and the total number of challenges created by each student. Sort your results by the total number of challenges in descending order. If more than one student created the same number of challenges, then sort the result by hacker_id. If more than one student created the same number of challenges and the count is less than the maximum number of challenges created, then exclude those students from the result.
```mysql
WITH cnt AS (
SELECT h.hacker_id, name, COUNT(challenge_id) AS challenges_created
FROM Hackers h
JOIN Challenges c ON c.hacker_id = h.hacker_id
GROUP BY h.hacker_id, name)

SELECT hacker_id, name, challenges_created
FROM cnt
WHERE challenges_created=(SELECT MAX(challenges_created) FROM cnt) 
OR challenges_created IN (SELECT challenges_created FROM cnt
                          GROUP BY challenges_created
                          HAVING COUNT(challenges_created)=1)
ORDER BY challenges_created DESC, hacker_id;
```



- 10. [Contest Leaderboard](https://www.hackerrank.com/challenges/contest-leaderboard/problem?isFullScreen=false): You did such a great job helping Julia with her last coding contest challenge that she wants you to work on this one, too! The total score of a hacker is the sum of their maximum scores for all of the challenges. Write a query to print the hacker_id, name, and total score of the hackers ordered by the descending score. If more than one hacker achieved the same total score, then sort the result by ascending hacker_id. Exclude all hackers with a total score of 0 from your result.
```mysql
SELECT hacker_id, name, SUM(score_max) AS total
FROM Hackers h
JOIN (SELECT hacker_id, MAX(score) AS score_max
      FROM Submissions s
      GROUP BY hacker_id, challenge_id) t
USING(hacker_id)
GROUP BY hacker_id, name
HAVING total != 0
ORDER BY total DESC, hacker_id;
```
OR
```mysql
SELECT hacker_id, name, SUM(score_max) AS total
FROM (SELECT hacker_id, name, MAX(score) AS score_max
      FROM Hackers h
      JOIN Submissions s USING(hacker_id)
      GROUP BY hacker_id, name, challenge_id) t
GROUP BY hacker_id, name
HAVING total != 0
ORDER BY total DESC, hacker_id;
```



- 11. [SQL Project Planning](https://www.hackerrank.com/challenges/sql-projects/problem?isFullScreen=false): You are given a table, Projects, containing three columns: Task_ID, Start_Date and End_Date. It is guaranteed that the difference between the End_Date and the Start_Date is equal to 1 day for each row in the table. If the End_Date of the tasks are consecutive, then they are part of the same project. Samantha is interested in finding the total number of different projects completed. Write a query to output the start and end dates of projects listed by the number of days it took to complete the project in ascending order. If there is more than one project that have the same number of completion days, then order by the start date of the project.
```mysql
SELECT Start_Date, MIN(End_Date)
FROM
(SELECT Start_Date FROM Projects WHERE Start_Date NOT IN (SELECT End_Date FROM Projects)) A,
(SELECT End_Date FROM Projects WHERE End_Date NOT IN (SELECT Start_Date FROM Projects)) B
WHERE Start_Date < End_Date
GROUP BY Start_Date
ORDER BY MIN(End_Date)-Start_Date, Start_Date;
```
OR
```mysql
SET @r1=0, @r2=0;

SELECT Start_Date, End_Date
FROM (SELECT @r1:=@r1+1 AS rn, Start_Date
      FROM Projects
      WHERE Start_Date NOT IN (SELECT End_Date FROM Projects)) A
JOIN (SELECT @r2:=@r2+1 AS rn, End_Date
      FROM Projects
      WHERE End_Date NOT IN (SELECT Start_Date FROM Projects)) B
USING(rn)
ORDER BY End_Date-Start_Date, Start_Date;
```




- 12. [Placements](https://www.hackerrank.com/challenges/placements/problem?isFullScreen=false): You are given three tables: Students, Friends and Packages. Students contains two columns: ID and Name. Friends contains two columns: ID and Friend_ID (ID of the ONLY best friend). Packages contains two columns: ID and Salary (offered salary in thousands per month). Write a query to output the names of those students whose best friends got offered a higher salary than them. Names must be ordered by the salary amount offered to the best friends. It is guaranteed that no two students got same salary offer.
```mysql
SELECT name
FROM Students s
JOIN Friends f ON s.ID  = f.ID 
JOIN Packages ps ON s.ID  = ps.ID
JOIN Packages pf ON f.Friend_ID  = pf.ID
WHERE ps.salary < pf.salary
ORDER BY pf.salary;
```




- 13. [Symmetric Pairs](https://www.hackerrank.com/challenges/symmetric-pairs/problem?isFullScreen=false): You are given a table, Functions, containing two columns: X and Y. Two pairs (X1, Y1) and (X2, Y2) are said to be symmetric pairs if X1 = Y2 and X2 = Y1. Write a query to output all such symmetric pairs in ascending order by the value of X. List the rows such that X1 ??? Y1.
```mysql
SELECT f1.X, f1.Y 
FROM Functions f1
JOIN Functions f2 ON f1.X=f2.Y AND f1.Y=f2.X
GROUP BY f1.X, f1.Y
HAVING COUNT(f1.X)>1 OR f1.X<f1.Y
ORDER BY f1.X
```


## Assessment Test 8


- 1. [Interviews](https://www.hackerrank.com/challenges/interviews/problem?isFullScreen=false): Samantha interviews many candidates from different colleges using coding challenges and contests. Write a query to print the contest_id, hacker_id, name, and the sums of total_submissions, total_accepted_submissions, total_views, and total_unique_views for each contest sorted by contest_id. Exclude the contest from the result if all four sums are . Note: A specific contest can be used to screen candidates at more than one college, but each college only holds  screening contest.
```mysql
SELECT contest_id, hacker_id, name, SUM(total_submissions_agg), SUM(total_accepted_submissions_agg), SUM(total_views_agg), SUM(total_unique_views_agg)
FROM contests c
JOIN colleges cl USING(contest_id)
JOIN challenges ch USING(college_id)
LEFT JOIN (SELECT challenge_id, SUM(total_views) AS total_views_agg, SUM(total_unique_views) AS total_unique_views_agg
           FROM view_stats 
           GROUP BY challenge_id) v USING(challenge_id)
LEFT JOIN (SELECT challenge_id, SUM(total_submissions) AS total_submissions_agg, SUM(total_accepted_submissions) AS total_accepted_submissions_agg
           FROM submission_stats 
           GROUP BY challenge_id) s USING(challenge_id)
GROUP BY contest_id, hacker_id, name
HAVING SUM(total_submissions_agg)*SUM(total_accepted_submissions_agg)*SUM(total_views_agg)*SUM(total_unique_views_agg)!=0
ORDER BY contest_id;
```


- 2. [15 Days of Learning SQL](https://www.hackerrank.com/challenges/15-days-of-learning-sql/problem?isFullScreen=false): Julia conducted a 15 days of learning SQL contest. The start date of the contest was March 01, 2016 and the end date was March 15, 2016. Write a query to print total number of unique hackers who made at least 1 submission each day (starting on the first day of the contest), and find the hacker_id and name of the hacker who made maximum number of submissions each day. If more than one such hacker has a maximum number of submissions, print the lowest hacker_id. The query should print this information for each day of the contest, sorted by the date.
```mysql
WITH T1 AS (
SELECT submission_date, COUNT(hacker_id) cnt
FROM (SELECT hacker_id, submission_date, 
      ROW_NUMBER() OVER (PARTITION BY hacker_id ORDER BY submission_date) date_rn
      FROM Submissions 
      GROUP BY submission_date, hacker_id
      HAVING COUNT(submission_id) > 0) T
WHERE date_rn >= DAY(submission_date)
GROUP BY submission_date),
T2 AS (
SELECT submission_date, hacker_id, sub_cnt,
       RANK() OVER (PARTITION BY submission_date ORDER BY sub_cnt DESC, hacker_id) rank
FROM (SELECT submission_date, hacker_id, COUNT(submission_id) sub_cnt
      FROM Submissions
      GROUP BY submission_date, hacker_id) T)

SELECT T1.submission_date, T1.cnt, T2.hacker_id, H.name
FROM T1
JOIN T2 ON T1.submission_date = T2.submission_date
JOIN Hackers H ON H.hacker_id = T2.hacker_id
WHERE T2.rank = 1
```





## Assessment Test 9


- 1. 
```mysql

```




## For More Information

For any additional questions, please contact **Ning Chen???chen.ning345@gmail.com**.

## Repository Structure

Description of the structure of the repository and its contents:
```
????????? README.md                           <- Solution of SQL Problem Set
????????? data                                <- SQL database
????????? images                              <- logo
```