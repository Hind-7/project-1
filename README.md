# project-1
first project in nanodgree
Query1- the query used for first qustion 

SELECT f.title As film_titel, c.name As category_name, count(r.rental_id) As rental_count
FROM film f
JOIN film_category fc
ON fc.film_id = f.film_id 
JOIN inventory i
ON  f.film_id = i.film_id 
JOIN rental r
ON i.inventory_id = r.inventory_id
JOIN category c
ON c.category_id = fc.category_id
WHERE c.name IN('Animation','Children','Classics','Comedy','Family','Music')
GROUP BY film_titel, category_name
ORDER BY category_name, film_titel
limit 12;



Query2- the query used for second qustion 

With t1 as (SELECT f.title film, c.name category, f.rental_duration,NTILE(4)over(PARTITION BY f.rental_duration )AS standard_quartile
FROM film f
JOIN film_category fc
ON fc.film_id = f.film_id
JOIN category c
ON c.category_id = fc.category_id
WHERE( c.name= 'Animation'
OR c.name='Children' 
OR c.name='Classics' 
OR c.name='Comedy' 
OR c.name= 'Family' 
OR c.name='Music')
ORDER BY rental_duration, standard_quartile)

select category, rental_duration,count(*)
from t1 
group by category, rental_duration
order by 1,2

Query3- the query used for third qustion 

SELECT n.name, n.standard_quartile, COUNT(n.standard_quartile)
FROM
(SELECT f.title, c.name , f.rental_duration, NTILE(4) OVER (ORDER BY f.rental_duration) AS standard_quartile
FROM film f
JOIN film_category fc
ON fc.film_id = f.film_id
JOIN category c
ON c.category_id = fc.category_id
WHERE c.name IN('Animation','Children','Classics','Comedy','Family','Music')) 
n
GROUP BY name, standard_quartile
ORDER BY name,standard_quartile
Limit 7;

Query4- the query used for fourth qustion

SELECT DATE_TRUNC('month', p.payment_date)pay_mon, c.first_name || ' ' || c.last_name AS fullname, COUNT(p.amount) AS pay_countpermon, SUM(p.amount) AS pay_amount
FROM customer c
JOIN payment p 
ON p.customer_id = c.customer_id
WHERE c.first_name || ' ' || c.last_name IN
(SELECT n.fullname
FROM
(SELECT c.first_name || ' ' || c.last_name AS fullname, SUM(p.amount) as amount_total, COUNT(p.amount) AS pay_countpermon
FROM customer c
JOIN payment p
ON p.customer_id = c.customer_id
GROUP BY 1
ORDER BY 2 DESC
LIMIT 10) n) AND (p.payment_date BETWEEN '2007-01-01' AND '2008-01-01')
GROUP BY 2, 1
ORDER BY 2, 1, 3 
LIMIT 11;







