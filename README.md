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

### EXERCISE 3 

Group by query:

``` sql
select offices.officeCode, sum(orderdetails.priceEach), max(payments.amount)
from offices
inner join employees on offices.officeCode = employees.officeCode
inner join customers on customers.salesRepEmployeeNumber = employees.employeeNumber
inner join payments on payments.customerNumber = customers.customerNumber
inner join orders on orders.customerNumber = customers.customerNumber
inner join orderdetails on orders.orderNumber = orderdetails.orderNumber
group by offices.officeCode
order by offices.officeCode;
```
Execution Plan:

![alt text](https://github.com/FarkIst/DBAssignment6_QueryPerformance/blob/master/Exercise_3_Execution_Plan_Group_By.png)

Windowing query:

``` sql
select distinct offices.officeCode, sum(orderdetails.priceEach) over (partition by offices.officeCode),max(payments.amount) over (partition by offices.officeCode)
from offices
inner join employees on offices.officeCode = employees.officeCode
inner join customers on customers.salesRepEmployeeNumber = employees.employeeNumber
inner join payments on payments.customerNumber = customers.customerNumber
inner join orders on orders.customerNumber = customers.customerNumber
inner join orderdetails on orders.orderNumber = orderdetails.orderNumber
order by offices.officeCode;
```
Execution Plan: 
* Currently we're getting a BUG when we attempt to enter the Execution Plan, not certain what the issue is here, the bug is as followed: Exception: 'NoneType' object has no attribute 'do_relayout'

Instead we've decided to provide the Tubular Plan, as followed: 

![alt text](https://github.com/FarkIst/DBAssignment6_QueryPerformance/blob/master/Exercise_3_Execution_Plan_Tubular.png)

### EXERCISE 4 

Query to return displayName and title of all posts which with the word "grounds" in the title: 

``` sql
SELECT displayName, title 
FROM users INNER JOIN posts ON posts.OwnerUserId = users.id 
WHERE title '%grounds%';
```
