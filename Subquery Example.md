~~~SQL
-- This query looks for the top 5 customers who paid the most, which countries they live in, and how many customers those countries have
SELECT D.country, COUNT(DISTINCT A.customer_id) AS all_customer_count, COUNT(top_5_customers) AS 
top_customer_count 
FROM customer A 
INNER JOIN address B ON A.address_id = B.address_id 
INNER JOIN city C ON B.city_id = C.city_id 
INNER JOIN country D ON C.country_id = D.country_id 
LEFT JOIN  
(SELECT A.customer_id, B.first_name, B.last_name, E.country, D.city, SUM(A.amount) AS 
total_amount_paid 
FROM payment A 
INNER JOIN customer B ON A.customer_id = B.customer_id 
INNER JOIN address C ON B.address_id = C.address_id 
INNER JOIN city D ON C.city_id = D.city_id 
INNER JOIN country E ON D.country_id = E.country_id 
WHERE 
D.city IN 
(SELECT C.city 
FROM customer A 
INNER JOIN address B ON A.address_id = B.address_id 
INNER JOIN city C ON B.city_id = C.city_id 
INNER JOIN country D ON C.country_id = D.country_id 
WHERE D.country IN 
(SELECT D.country 
FROM customer A 
INNER JOIN address B ON A.address_id = B.address_id 
INNER JOIN city C ON B.city_id = C.city_id 
INNER JOIN country D ON C.country_id = D.country_id 
GROUP BY D.country 
ORDER BY COUNT(A.customer_id)DESC 
LIMIT 10)  
GROUP BY D.country, C.city 
ORDER BY COUNT(A.customer_id)DESC 
LIMIT 10) 
GROUP BY A.customer_id, B.first_name, B.last_name, E.country, D.city 
ORDER BY total_amount_paid DESC 
LIMIT 5) AS top_5_customers 
ON A.customer_id = top_5_customers.customer_id 
GROUP BY D.country 
ORDER BY all_customer_count DESC 
LIMIT 5
~~~
