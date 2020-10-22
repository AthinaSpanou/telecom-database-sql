# telecom-database-sql

## Description of the Case:
A telecom provider (TP) wants to develop a relational database to monitor customers, calls and plans.

Customers of TP are described through a unique identifier, first and last name, date of birth, gender (‘male’ or
‘female’) and live in a city. Cities are described by a unique identifier, name, population and mean income. A
customer has one or more contracts with TP. A contract has a unique identifier, phone number, starting date,
ending date and a description. A contract is also associated to a plan offered by TP (e.g. Red1 of Vodafone). A
plan is described by a unique identifier, name, free-minutes, free-sms and free-MB attributes. Finally, calls made
by a phone number have to be stored, along with a unique identifier, the date/time of the call (hour, minute, day,
month, year), the called phone number and the duration of the call (in seconds).

## Deliverables:
1. Τhe Entity-Relationship Diagram (ERD) to model entities, relationships, attributes, cardinalities,
and all necessary constraints. 

![alt text](https://github.com/AthinaSpanou/telecom-database-sql/blob/main/ERD.png)

2. The relational Schema in MySQL/SQLServer.

![alt text](https://github.com/AthinaSpanou/telecom-database-sql/blob/main/Relationship%20Schema%20Model.png)

3. [SQL Code](https://github.com/AthinaSpanou/telecom-database-sql/blob/main/sql_queries.sql) in order to test some queries.


#### First create tables that have no foreign keys
~~~~mysql
CREATE DATABASE tp;
USE tp;

CREATE TABLE city (
city_id INT NOT NULL AUTO_INCREMENT,
city_name VARCHAR(45) NOT NULL,
population INT NOT NULL,
mean_income FLOAT NOT NULL CHECK (mean_income >= 0),
PRIMARY KEY (city_id)
);

CREATE TABLE customer (
customer_id INT NOT NULL AUTO_INCREMENT,
first_name VARCHAR(45) NOT NULL,
last_name VARCHAR(45) NOT NULL,
date_of_birth DATE NOT NULL,
gender VARCHAR(10) NOT NULL,
city_id INT NOT NULL,
PRIMARY KEY (customer_id),
FOREIGN KEY (city_id) REFERENCES city(city_id)
);

CREATE TABLE plan (
plan_id INT NOT NULL AUTO_INCREMENT,
plan_name VARCHAR(15) NOT NULL,
free_minutes VARCHAR(45) NOT NULL,
free_sms VARCHAR(45) NOT NULL,
free_mbs VARCHAR(45) NOT NULL,
PRIMARY KEY (plan_id)
);

CREATE TABLE contract (
contract_id INT NOT NULL AUTO_INCREMENT,
phone_number VARCHAR(15) NOT NULL,
contract_desc VARCHAR(255) NOT NULL,
start_date DATE NOT NULL,
end_date DATE NOT NULL,
customer_id INT NOT NULL,
plan_id INT NOT NULL,
PRIMARY KEY (contract_id),
FOREIGN KEY (customer_id) REFERENCES customer(customer_id),
FOREIGN KEY (plan_id) REFERENCES plan(plan_id)
);

CREATE TABLE calls (
call_id INT NOT NULL AUTO_INCREMENT,
date_time_of_call DATETIME NOT NULL,
called_phone_number VARCHAR(15) NOT NULL,
duration INT NOT NULL,
contract_id INT NOT NULL,
PRIMARY KEY (call_id),
FOREIGN KEY (contract_id) REFERENCES contract(contract_id)
);
~~~~
#### Insert a few records into the tables to test your queries below.
~~~~mysql
# TABLE CITY
INSERT INTO city(city_id,city_name,population,mean_income) VALUES 
(1, 'Athens',664046,16587),
(2, 'Karpenisi', 13105, 5467),
(3, 'Heraklion', 312514, 12388),
(4, 'Nafplio', 14203, 4385),
(5, 'Corinth', 48132, 11965);

SELECT * FROM city;

# TABLE CUSTOMER
INSERT INTO customer(customer_id, first_name, last_name, date_of_birth,gender,city_id) VALUES
(1,'Daphne', 'Dimitriou','1976/12/31','female',1),
(2,'Ioanna','Stavrou','1969/06/18','female',2),
(3,'Constantinos','Georgiou','1977/06/13','male',3),
(4,'Theodora','Papadopoulou','2000/12/25','female',4),
(5,'Demetrios','Theodorou','1991/02/13','male',5),
(6,'Gregorios','Economou','1992/07/16','male',1),
(7,'Markos','Petrides','1968/12/09','male',2),
(8,'Thalia','Vasiliou','1990/08/12','female',3);

SELECT * FROM customer;

# TABLE PLAN
INSERT INTO plan(plan_id, plan_name, free_minutes, free_sms, free_mbs) VALUES
(1,'Student',1200,1200,600),
(2,'Call+',600,100,500),
(3, 'Datalicious',500,500,2000),
(4, 'Mobile_Plus',2000,600,1000),
(5, 'Freedom',1000,600,800);

SELECT * FROM plan;

# TABLE CONTRACT
INSERT INTO contract(contract_id, phone_number, contract_desc, start_date, end_date, customer_id, plan_id) VALUES
(1,'6945825551','personal','2017/05/24','2019/05/24',1,1),
(2,'6941627544','corporate','2018/01/18','2019/03/27',2,2),
(3,'6912345551','personal','2019/08/02','2020/08/02',3,3),
(4,'6974125558','corporate','2017/01/01','2019/12/15',4,4),
(5,'6945328879','personal','2018/12/18','2019/12/18',5,5),
(6,'6955324428','corporate','2019/12/18','2020/10/12',6,1),
(7,'6970326079','personal','2018/12/18','2019/11/28',7,2),
(8,'6969853264','personal','2018/12/18','2019/12/30',8,3),
(9,'6965987412','corporate','2018/12/18','2019/02/05',1,4),
(10,'6598745236','personal','2018/12/18','2019/04/08',2,5),
(11,'6958741236','corporate','2018/12/18','2019/12/08',3,1);

SELECT * FROM contract;

# TABLE CALLS
INSERT INTO calls(call_id, date_time_of_call, called_phone_number, duration, contract_id) VALUES
(1,'2018/02/14 03:54','6970156809',72000,1),
(2,'2018/06/10 09:45','6974123214',18,2),
(3,'2017/10/09 22:10','6956321563',36000,3),
(4,'2017/02/14 09:38','6985236471',7500,4),
(5,'2018/06/10 09:55','6985236541',8,5),
(6,'2018/01/09 22:14','6974123214',42000,6),
(7,'2017/01/22 18:20','6951234785',1500,7),
(8,'2018/07/23 15:10','6987415236',38000,8),
(9,'2018/06/06 09:09','6985236541',14,9),
(10,'2017/07/17 11:11','6953628745',1530,10),
(11,'2018/10/09 22:20','6974123214',36000,1),
(12,'2018/06/20 10:10','6951236745',28,2),
(13,'2017/11/09 22:14','6974123214',4752,3),
(14,'2017/10/22 18:20','6958231473',42896,4),
(15,'2018/11/05 22:14','6914758239',2333,5),
(16, '2018/06/09 22:20','6974123214',36000,6),
(17,'2018/01/08 11:30','6985236541',33,7),
(18,'2017/07/12 19:10','6987415236',33000,8),
(19,'2018/01/07 04:10','6951234785',2500,9),
(20,'2018/02/14 14:14','6958231473',2365,11);

SELECT * FROM calls;
~~~~

#### A. Show the call id of all calls that were made between 8am and 10am on June 2018 having duration < 30

~~~~mysql
SELECT call_id AS 'Call ID'
FROM calls
WHERE MONTH(date_time_of_call) = 6 AND YEAR(date_time_of_call) = 2018 AND HOUR(date_time_of_call) BETWEEN 8 AND 10 AND duration < 30 ;

~~~~

#### B. Show the first and last name of customers that live in a city with population greater than 20000

~~~~mysql

SELECT first_name AS 'First Name' , last_name AS 'Last Name'
FROM customer , city
WHERE city.population > 20000 AND city.city_id = customer.city_id;
~~~~

#### C. Show the customer id that have a contract in the plan name LIKE 'Freedom' (use nested queries)
~~~~mysql

SELECT customer.customer_id AS 'Customer ID'
FROM customer, contract
WHERE contract.plan_id IN (SELECT plan_id
						   FROM plan
                           WHERE plan_name LIKE '%Freedom%') AND customer.customer_id=contract.customer_id;
~~~~
                 
#### D. For each contract that ends in less than sixty days from today, show the contract id, the phone number, the customer's id, his/her first name and his/her last name.

~~~~mysql

SELECT contract_id AS 'Contract ID', phone_number AS 'Phone Number', customer.customer_id AS 'Customer ID', first_name AS 'First Name', last_name AS 'Last Name'
FROM customer, contract
WHERE DATEDIFF(CURRENT_TIMESTAMP, end_date) < 60 AND customer.customer_id=contract.customer_id
GROUP BY contract_id;
~~~~

### E. For each contract id and each month of 2018, show the average duration of calls
~~~~mysql

SELECT contract_id AS 'Contract ID', MONTH(date_time_of_call) AS 'Month', ROUND(AVG(duration),2) AS 'Average Duration of Calls'
FROM calls
WHERE YEAR(date_time_of_call) = 2018
GROUP BY contract_id, MONTH(date_time_of_call)
ORDER BY contract_id ASC;
~~~~

#### F. Show the total duration of calls in 2018 per plan id
~~~~mysql

SELECT plan.plan_id AS 'Plan ID', SUM(calls.duration) AS 'Total Duration of Calls' 
FROM plan, contract, calls
WHERE plan.plan_id = contract.plan_id AND contract.contract_id=calls.contract_id AND YEAR(date_time_of_call)=2018
GROUP BY plan.plan_id;
~~~~

#### G. Show the top called number among TP's customers in 2018
~~~~mysql

SELECT called_phone_number AS 'Top Called Phone Number', COUNT(called_phone_number) AS 'Times Called'
FROM calls
WHERE YEAR(date_time_of_call) = 2018
GROUP BY called_phone_number
ORDER BY COUNT(called_phone_number) DESC
LIMIT 1;
~~~~

#### H. Show the contract ids and the months where the total duration of the calls was greater than the free minutes offered by the plan of the contract
~~~~mysql

CREATE VIEW total_calls(contract, call_month, duration,plan) AS						
SELECT contract.contract_id, MONTH(date_time_of_call), (duration), contract.plan_id
FROM calls 
INNER JOIN contract ON (calls.contract_id=contract.contract_id);

select * from total_calls;

SELECT total_calls.contract AS 'Contract ID', total_calls.call_month AS 'Month', total_calls.duration AS 'Duration', free_minutes AS 'Free Minutes'
FROM total_calls, plan
WHERE total_calls.plan=plan.plan_id
GROUP BY total_calls.contract,total_calls.call_month
HAVING SUM(total_calls.duration)>(free_minutes*60);

~~~~

#### I. For each month of 2018, show the percentage change of the total duration of calls compared to the same month of 2017
~~~~mysql

SELECT Month2018 AS 'Month', CONCAT(ROUND((duration2018-duration2017)/duration2017*100),'%') AS 'Percent Change'
FROM ( SELECT SUM(duration) AS duration2018, MONTH(date_time_of_call) AS Month2018
	   FROM calls
       WHERE YEAR(date_time_of_call)=2018
	   GROUP BY Month2018) AS calls_2018, 
       (SELECT SUM(duration) AS duration2017, month(date_time_of_call) AS Month2017
       FROM calls
       WHERE YEAR(date_time_of_call)=2017
      GROUP BY Month2017) AS calls_2017
WHERE calls_2018.Month2018=calls_2017.Month2017;
~~~~

#### J. For each city id and calls made in 2018, show the average call duration by females and the average call duration by males (i.e. three columns)
~~~~mysql

CREATE VIEW total_duration(callid,duration, crontractid,customerid) AS	
SELECT call_id, duration, calls.contract_id, contract.customer_id
FROM calls
JOIN contract ON calls.contract_id
WHERE YEAR(date_time_of_call)=2018 AND calls.contract_id=contract.contract_id;
 
 select * from total_duration;
 
CREATE VIEW female(city_id,femaleduration) AS	
SELECT city_id, AVG(duration)
FROM customer, total_duration
WHERE gender LIKE 'female' AND customer.customer_id=total_duration.customerid
GROUP BY city_id
ORDER BY city_id;

select * from female;

CREATE VIEW male(city_id,maleduration) AS	
SELECT city_id, AVG(duration)
FROM customer, total_duration
WHERE gender LIKE 'male' AND customer.customer_id=total_duration.customerid
GROUP BY city_id
ORDER BY city_id;

select * from male;

(SELECT city_id AS 'City ID', ROUND(femaleduration,2) AS 'Average Duration of Females', ROUND(maleduration,2) AS 'Average Duration of Males'
FROM female LEFT JOIN male USING (city_id))
UNION
SELECT city_id, ROUND(femaleduration,2) ,ROUND(maleduration,2) 
FROM male LEFT JOIN female USING (city_id); 

~~~~
#### K.  For each city id, show the city id, the ratio of the total duration of the calls made from customers staying in that city in 2018 over the total duration of all calls made in 2018, and the ratio of the city’s population over the total population of all cities (i.e three columns)

~~~~mysql
# Ratio of the city’s population over the total population of all cities
CREATE VIEW city_population(city_id,population_ratio) AS
SELECT city_a.city_id, city_a.population/SUM(city_b.population)
FROM city AS city_a JOIN city AS city_b
GROUP BY city_a.city_id;

select * from city_population;

# Ratio of the total duration of the calls made from customers staying in that city in 2018 over the total duration of all calls
CREATE VIEW city_call(city_id,calls_ratio) AS
SELECT city.city_id, SUM(a.duration)/(SELECT SUM(duration) FROM calls WHERE YEAR(date_time_of_call)=2018) as duration_ratio
FROM city JOIN customer ON customer.city_id=city.city_id JOIN contract ON contract.customer_id=customer.customer_id JOIN calls AS a ON a.contract_id=contract.contract_id WHERE YEAR(date_time_of_call)=2018
GROUP BY city.city_id;

Select * from city_call;

(SELECT city_id AS 'City ID', ROUND(population_ratio,2)  AS 'Population Ratio', ROUND(calls_ratio,2) AS 'Calls Ratio'
FROM city_population  LEFT JOIN city_call USING (city_id))
UNION
SELECT city_id, ROUND(population_ratio,2) ,ROUND(calls_ratio,2)
FROM city_call  LEFT JOIN  city_population USING (city_id); 
~~~~
