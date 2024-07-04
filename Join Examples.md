~~~SQL
-- This query looks for the top 10 countries with the highest revenue
SELECT DISTINCT country, SUM(amount) AS total_amount_paid
FROM
payment A
INNER JOIN customer B ON A.customer_id = B.customer_id
INNER JOIN address C ON B.address_id = C.address_id
INNER JOIN city D ON C.city_id = D.city_id
INNER JOIN country E ON D.country_id = E.country_id
GROUP BY country
ORDER BY total_amount_paid DESC
LIMIT 10
~~~
~~~SQL
-- This query lists all genres and their revenue
SELECT E.name, SUM(amount) AS total_amount_paid
FROM
payment A
INNER JOIN rental B ON A.rental_id = B.rental_id
INNER JOIN inventory C ON B.inventory_id = C.inventory_id
INNER JOIN film_category D ON C.film_id = D.film_id
INNER JOIN category E ON D.category_id = E.category_id
GROUP BY E.name
ORDER BY total_amount_paid DESC
~~~
~~~SQL
-- This query looks for the top 10 cities in the top 10 countries with the highest customer counts
SELECT C.city, D.country,
COUNT (A.customer_id) AS number_of_customers
FROM customer A
INNER JOIN address B ON A.address_id = B.address_id
INNER JOIN city C ON B.city_id = C.city_id
INNER JOIN country D ON C.country_id = D.country_id
WHERE 
D.country IN
(SELECT D.country 
FROM customer A
INNER JOIN address B ON A.address_id = B.address_id
INNER JOIN city C ON B.city_id = C.city_id
INNER JOIN country D ON C.country_id = D.country_id
GROUP BY D.country
ORDER BY COUNT(A.customer_id) DESC
LIMIT 10)
GROUP BY C.city, D.country
ORDER BY number_of_customers DESC
LIMIT 10
~~~
