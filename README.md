# DBAssignment6_QueryPerformance

### EXERCISE 2 

The query with better performance. Please reformat this text. (Arek or Vlad)

``` sql

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
