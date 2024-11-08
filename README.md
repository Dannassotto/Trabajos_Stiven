## CONSULTAS 1



# multitablas

```sql
SELECT film.title, category.name
FROM film, category
WHERE film.film_id = category.category_id;


SELECT actor.first_name, actor.last_name, film.title
FROM actor
JOIN film_actor ON actor.actor_id = film_actor.actor_id
JOIN film ON film_actor.film_id = film.film_id
LIMIT 20;


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

# INNER JOIN

```sql

SELECT f.title, c.name
FROM film f
INNER JOIN film_category fc ON f.film_id = fc.film_id
INNER JOIN category c ON fc.category_id = c.category_id;


SELECT a.first_name, a.last_name, f.title
FROM actor a
INNER JOIN film_actor fa ON a.actor_id = fa.actor_id
INNER JOIN film f ON fa.film_id = f.film_id;

SELECT f.title, f.release_year
FROM film f
INNER JOIN film_actor fa ON f.film_id = fa.film_id;

SELECT first_name, last_name, COUNT(film_id) AS total_peliculas
FROM actor
INNER JOIN film_actor USING (actor_id)
INNER JOIN film USING (film_id)
GROUP BY actor_id;

SELECT c.first_name, c.last_name, COUNT(*) AS total_alquileres
FROM customer c
JOIN rental r ON c.customer_id = r.customer_id
GROUP BY c.customer_id;
```

# SIMPLES

```sql

SELECT title, release_year
FROM film
ORDER BY release_year DESC;

SELECT first_name, last_name
FROM customer
ORDER BY last_name;

SELECT COUNT(*) AS total_peliculas
FROM film;

SELECT category_id, name
FROM category;

SELECT f.title, COUNT(*)
FROM film f
JOIN inventory i ON f.film_id = i.film_id
JOIN rental r ON i.inventory_id = r.inventory_id
GROUP BY f.film_id
ORDER BY COUNT(*) DESC
LIMIT 5;
```

# Procedimiento

```sql

DELIMITER $$

CREATE PROCEDURE total_alquileres_cliente(IN p_customer_id INT)
BEGIN
  SELECT SUM(f.rental_rate) FROM rental r JOIN inventory i ON r.inventory_id = i.inventory_id JOIN film f ON i.film_id = f.film_id WHERE r.customer_id = p_customer_id;
END $$


CREATE PROCEDURE eliminar_actor(IN p_actor_id INT)
BEGIN
  DELETE FROM actor WHERE actor_id = p_actor_id;
END $$

CREATE PROCEDURE agregar_actor(IN p_first_name VARCHAR(50), IN p_last_name VARCHAR(50))
BEGIN
  INSERT INTO actor VALUES (NULL, p_first_name, p_last_name);
END $$

CREATE PROCEDURE actualizar_direccion_cliente(IN p_customer_id INT, IN p_new_address VARCHAR(255))
BEGIN
  UPDATE address SET address = p_new_address WHERE address_id = (SELECT address_id FROM customer WHERE customer_id = p_customer_id);
END $$

CREATE PROCEDURE peliculas_alquiladas_cliente(IN p_customer_id INT)
BEGIN
  SELECT f.title FROM rental r JOIN inventory i ON r.inventory_id = i.inventory_id JOIN film f ON i.film_id = f.film_id WHERE r.customer_id = p_customer_id;
END $$

DELIMITER ;
```









