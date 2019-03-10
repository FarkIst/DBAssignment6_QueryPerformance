# DBAssignment6_QueryPerformance

### EXERCISE 1

Given query:

The main performance problem is that the there are no indexes on the city columns.

``` sql

select customers.customerName,
 customers.city,
 offices.city
from customers, employees, offices
where customers.salesRepEmployeeNumber = employees.employeeNumber and
 employees.officeCode = offices.officeCode and
 customers.city = offices.city

```

Execution Plan:

![alt text](https://github.com/FarkIst/DBAssignment6_QueryPerformance/blob/master/Exercise_1_Execution_Plan.png)

### EXERCISE 2 

The optimized query:

We create indexes on the city columns of both the customers and offices tables to make the look up faster.

``` sql

create index city on customers(city);

create index city on offices(city);

with customer_rep as (
 select customers.customerName as shopName,
 customers.city as shopCity,
 offices.city as officeCity
 from customers, employees, offices
 where customers.salesRepEmployeeNumber = employees.employeeNumber and
 employees.officeCode = offices.officeCode
)

select *
from customer_rep
where shopCity = officeCity

```
Execution Plan:

![alt text](https://github.com/FarkIst/DBAssignment6_QueryPerformance/blob/master/Exercise_2_Execution_Plan.png)

