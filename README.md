# Trabajos_Stiven
## CONSULTAS 1

#multitablas

```sql
SELECT film.title, category.name
FROM film, category
WHERE film.film_id = category.category_id;


SELECT actor.first_name, actor.last_name, film.title
FROM actor
JOIN film_actor ON actor.actor_id = film_actor.actor_id
JOIN film ON film_actor.film_id = film.film_id;


SELECT customer.first_name, customer.last_name, film.title
FROM customer, rental, inventory, film
WHERE customer.customer_id = rental.customer_id
AND rental.inventory_id = inventory.inventory_id
AND inventory.film_id = film.film_id
LIMIT 20;

SELECT f.title
FROM actor a, film f, film_actor fa
WHERE a.actor_id = fa.actor_id
AND fa.film_id = f.film_id
AND a.first_name = 'JULIA'
AND a.last_name = 'MCQUEEN';
```
