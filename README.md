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

Deliverables:
1. Τhe Entity-Relationship Diagram (ERD) to model entities, relationships, attributes, cardinalities,
and all necessary constraints. 

![alt text](https://github.com/AthinaSpanou/telecom-database-sql/blob/main/ERD.png

2. (10%) Create the relational schema in MySQL/SQLServer and insert a few records into the tables to test
your queries below. You will have to hand in the CREATE TABLE statements.
3. (60%) Write SQL code and test it to your data for the following queries
