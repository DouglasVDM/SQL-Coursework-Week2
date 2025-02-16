# E-Commerce Database

In this homework, you are going to work with an ecommerce database. In this database, you have `products` that `consumers` can buy from different `suppliers`. Customers can create an `order` and several products can be added in one order.

## Submission

Below you will find a set of tasks for you to complete to set up a database for an e-commerce app.

To submit this homework write the correct commands for each question here:
```sql
1.
select  name , address 
from customers 
where country = 'United States';

2.
select *
from customers
order by name asc;

3.
select *
from products
where lower(product_name) like '%socks%';

select *
from products
where product_name ilike '%socks%';

select *
from products
where product_name ~* 'socks'; 

4.
select prod_id , product_name , unit_price , supp_id 
from product_availability 
inner join products 
on product_availability.prod_id = products.id
inner join suppliers 
on product_availability.supp_id = suppliers.id 
where product_availability.unit_price > 100;

5.
select product_name
from product_availability
inner join products
on product_availability.prod_id = products.id
inner join suppliers 
on product_availability.supp_id = suppliers.id 
order by unit_price desc
limit 5;

6.
select product_name, unit_price, supplier_name
from product_availability
inner join products
on product_availability.prod_id = products.id
inner join suppliers 
on product_availability.supp_id = suppliers.id;

7.
select product_name, supplier_name
from product_availability 
inner join products 
on product_availability.prod_id = products.id
inner join suppliers
on product_availability.supp_id = suppliers.id 
where country = 'United Kingdom';

8.
select order_id , order_reference, order_date, sum(quantity * unit_price) as total_cost 
from order_items 
inner join product_availability
on order_items.product_id = product_availability.prod_id 
inner join customers 
on order_items.product_id = customers.id 
inner join orders  
on order_items.order_id = orders.id
where customers.id = 1
group by order_id, order_reference, order_date;

9.
SELECT *
FROM order_items 
INNER JOIN orders 
ON order_items.id = orders.id 
INNER JOIN customers
ON order_items.order_id = customers.id 
WHERE customers.name = 'Hope Crosby';

10.
SELECT product_name , unit_price, quantity
FROM order_items as oi 
INNER JOIN product_availability as pa 
ON oi.product_id = pa.prod_id
inner join products as p 
ON oi.product_id = p.id 
INNER JOIN orders AS o 
ON oi.order_id = o.id 
WHERE order_reference = 'ORD006';

11.
SELECT product_name AS Product, supplier_name AS Supplier, name AS Customer, order_reference AS Order_Number, order_date, quantity 
FROM public.order_items AS oi
INNER JOIN product_availability as pa 
ON oi.product_id = pa.prod_id
inner join products as p 
ON oi.product_id = p.id 
INNER JOIN orders AS o 
ON oi.order_id = o.id
INNER JOIN customers AS c
ON oi.order_id = c.id
INNER JOIN suppliers AS s
ON oi.supplier_id = s.id ;

12.
SELECT name AS customers 
FROM public.order_items AS oi
INNER JOIN customers AS c
ON oi.order_id = c.id
INNER JOIN suppliers AS s
ON oi.supplier_id = s.id
WHERE s.country = 'China';

13.
SELECT name AS customer_name, order_reference, order_date, sum(quantity * unit_price) AS order_total_amount
FROM order_items AS oi 
INNER JOIN customers AS c 
ON oi.order_id = c.id 
INNER JOIN orders AS o 
ON oi.order_id = o.id 
INNER JOIN product_availability AS pa 
ON oi.product_id = pa.prod_id
GROUP BY c.name, order_reference, order_date
ORDER BY order_total_amount DESC ;
```

When you have finished all of the questions - open a pull request with your answers to the `Databases-Homework` repository.

## Setup

To prepare your environment for this homework, open a terminal and create a new database called `cyf_ecommerce`:

```sql
createdb cyf_ecommerce
```

Import the file [`cyf_ecommerce.sql`](./cyf_ecommerce.sql) in your newly created database:

```sql
psql -d cyf_ecommerce -f cyf_ecommerce.sql
```

Open the file `cyf_ecommerce.sql` in VSCode and examine the SQL code. Take a piece of paper and draw the database with the different relationships between tables (as defined by the REFERENCES keyword in the CREATE TABLE commands). Identify the foreign keys and make sure you understand the full database schema.

## Task

Once you understand the database that you are going to work with, solve the following challenge by writing SQL queries using everything you learned about SQL:

1. Retrieve all the customers' names and addresses who live in the United States
2. Retrieve all the customers in ascending name sequence
3. Retrieve all the products whose name contains the word `socks`
4. Retrieve all the products which cost more than 100 showing product id, name, unit price and supplier id.
5. Retrieve the 5 most expensive products
6. Retrieve all the products with their corresponding suppliers. The result should only contain the columns `product_name`, `unit_price` and `supplier_name`
7. Retrieve all the products sold by suppliers based in the United Kingdom. The result should only contain the columns `product_name` and `supplier_name`.
8. Retrieve all orders, including order items, from customer ID `1`. Include order id, reference, date and total cost (calculated as quantity * unit price).
9. Retrieve all orders, including order items, from customer named `Hope Crosby`
10. Retrieve all the products in the order `ORD006`. The result should only contain the columns `product_name`, `unit_price` and `quantity`.
11. Retrieve all the products with their supplier for all orders of all customers. The result should only contain the columns `name` (from customer), `order_reference`, `order_date`, `product_name`, `supplier_name` and `quantity`.
12. Retrieve the names of all customers who bought a product from a supplier based in China.
13. List all orders giving customer name, order reference, order date and order total amount (quantity * unit price) in descending order of total.

