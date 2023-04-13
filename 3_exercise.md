## Exercise 3-1
Retrieve the actor ID, first name, and last name for all actors. Sort by last name and then by first name.
```sql
SELECT actor_id, first_name, last_name FROM actor ORDER BY last_name, first_name;
```
## Exercise 3-2
Retrieve the actor ID, first name, and last name for all actors whose last name equals 'WILLIAMS' or 'DAVIS'.
```sql
SELECT actor_id, first_name, last_name 
FROM actor 
WHERE (last_name = 'WILLIAMS' or last_name = 'DAVIS');
```
## Exercise 3-3
Write a query against the rental table that returns the IDs of the customers who rented a film on July 5, 2005 (use the rental.rental_date column, and you can use the date() function to ignore the time component). Include a single row for each distinct customer ID.
```sql
SELECT DISTINCT customer_id
FROM rental
WHERE date(return_date) = '2005-06-05';
```
## Exercise 3-4
Fill in the blanks (denoted by <#>) for this multitable query to achieve the following results:
```sql
SELECT c.email, r.return_date
FROM customer c
  INNER JOIN rental r
  ON c.customer_id = r.customer_id
WHERE date(r.rental_date) = '2005-06-14'
ORDER BY r.return_date DESC;
```