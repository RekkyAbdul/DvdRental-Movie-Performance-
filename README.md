# DvdRental-Movie-Performance-

### Project Overview
Dvdrental is an establishment for renting movies of various genres. This project aims to gain an overview of the sales performance, customer behaviours, and product movement of
the company by exploring and analyzing the various components of the business data. 

### Tool Use - PostgreSQL

### Data Source 

The dataset used for this analysis is "dvdrental.tar". the dataset contains 15 tables ( film, rental, customer, city, inventory, store, payment, staff, film_category, actor, film_actor, country,
language, category, and address)

### Data Cleaning/Preparation

In this phase, the following tasks were performed
- Data loading and Inspection: A new database was created with a similar name as the dataset file "dvdrental". Then the dataset was loaded into the database by locating and
selecting the file directory.
- Checking the data ensuring relevant data points have the correct data type.

### EDA-Exploratory Data Analysis

In the EDA phase, I explore the data to answer key questions, such as
  1. what is the most rented movie?
  2. What are the movies rented so far?
  3. Display movies not rented so far?
  4. who are the top 10 Customers with Highest total payment?
  5. What is the number of times each movie  was rented?
  6. Display the firstname and lastname of actors and the number of movies acted
  7. Display the names of actors who acted in more than 20 movie
  8. What is the number of rented movies under each rating?
  9. Profit generated from each store(store1 and store2)
  10. Movies offered for rent in both stores
  11. Movies offered for rent in store 1 and not in store 2
  12. Movies not offered for rent in any of the stores yet

### Data Analysis

 ```SQL

 (1) Top 10 Customers with Highest total payment
SELECT CONCAT (cust.first_name, '  ', cust.last_name) As Customer_name, SUM(pay.amount) AS total_pay
FROM customer AS cust
INNER JOIN payment AS pay 
ON cust.customer_id = pay.customer_id
GROUP BY 1
ORDER BY total_pay DESC Limit 10;


--(2) Most rented movie
SELECT film.title, COUNT(ren.rental_id) AS rent_count
FROM rental AS ren
INNER JOIN inventory As inv
ON ren.inventory_id = inv.inventory_id
INNER JOIN film
ON film.film_id = inv.film_id
GROUP BY Film.title
ORDER BY rent_count DESC
LIMIT 1;


--(3)    Display movies rented so far
SELECT DISTINCT film.film_id, film.title
FROM film
INNER JOIN inventory inv
ON film.film_id = inv.film_id
INNER JOIN rental ren
ON inv.inventory_id = ren.inventory_id
ORDER BY Film.film_id;


--(4)   Display movies not rented so far
SELECT film.title
FROM film
LEFT JOIN inventory inv
ON film.film_id	= inv.film_id
LEFT JOIN rental ren
ON inv.inventory_id = ren.inventory_id
WHERE inv.inventory_id IS NULL;


--(5)Number times each movie  was rented
SELECT film.title, COUNT(ren.rental_id) AS rent_count
FROM rental AS ren
INNER JOIN inventory As inv
ON ren.inventory_id = inv.inventory_id
INNER JOIN film
ON film.film_id = inv.film_id
GROUP BY Film.title
ORDER BY rent_count DESC;


--(6) Show the firstname and lastname of actors and number of movie acted
SELECT actor.first_name, actor.last_name, COUNT(FA.film_id) AS number_of_film_acted
FROM actor
INNER JOIN film_actor AS FA
ON actor.actor_id = FA.actor_id
GROUP BY 1,2
ORDER BY actor.first_name;


--(7) Display the names of actor that acted in more than 20 movie
SELECT CONCAT(actor.first_name,'    ', actor.last_name) As full_name,
COUNT(FA.film_id) AS number_of_film_acted
FROM actor
INNER JOIN film_actor AS FA
ON actor.actor_id = FA.actor_id
GROUP BY full_name
HAVING COUNT(FA.film_id)>20
ORDER BY number_of_film_acted;


--(8) The number of rented movies under each rating
SELECT film.rating, COUNT(rental_id) AS rented_count
FROM film
INNER JOIN inventory AS inv
ON film.film_id = inv.film_id
INNER JOIN rental 
ON inv.inventory_id = rental.inventory_id
GROUP BY film.rating;


--(9)  Profit generated from each store(store1 and store2)
SELECT store_id, SUM(amount)
FROM inventory AS inv
INNER JOIN rental
ON inv.inventory_id = rental.inventory_id
INNER JOIN payment AS pay
ON rental.rental_id = pay.rental_id
GROUP BY inv.store_id;


--(10) Movies offered for rent in both stores
SELECT  DISTINCT film.title, inv.store_id
FROM film 
INNER JOIN inventory AS inv
ON film.film_id = inv.film_id
WHERE inv.store_id = 1 OR inv.store_id = 2
GROUP By film.title, inv.store_id
ORDER BY film.title;


--(11) Movies offered for rent in store 1 and not in store 2
SELECT film.title, COUNT(inv.inventory_id)
FROM film
INNER JOIN inventory AS inv
ON film.film_id = inv.film_id
WHERE inv.store_id = 1 and inv.film_id NOT IN
   (SELECT film_id from inventory
    WHERE store_id = 2)
GROUP BY 1
ORDER BY film.title;


--(12)   Movies not offered for rent in any of the store yet

SELECT film.title
FROM film
LEFT JOIN inventory AS inv
ON film.film_id = inv.film_id
LEFT JOIN store
ON inv.store_id = store.store_id
WHERE inv.inventory_id IS NULL AND store.store_id IS NULL
ORDER BY film.title;

```

 ### Findings/Insights
 
 - The Most rented movie is "Bucket Brotherhood" with a rental count of 34 and the least rented movies are "Hardly Robbers"
   "Mixed Doors", and "Train Bunch" each with a rental count of 4
 - The Top 1 customer is "Eleanor  Hunt" with a total pay of $211.55
 - The total number of unique movies rented so far is 958
 - The number of unique movies not rented yet is 42.
 - The movies rented most are movies acted by Susan Davis, Gina Degeneres, Walter Torn, Mary Keitel, and Matthew Carrey.
 - Movies rated PG-13 are the most rented movies with a rental count of 3585
 - The profit generated by both stores is almost the same. This shows that both stores are properly managed.
 - The total profit generated by the business is $61,312.04
 - The total movie offer for rent in both stores is 1521.
 - The total rental count of all Movies is 16,044.
 - Movies offered for rent  in store 1 and not in store 2 are 196.

   ![rating - Excel 28_02_2024 03_09_44 (2)](https://github.com/RekkyAbdul/DvdRental-Movie-Performance-/assets/149950425/a07cc4ae-6f3b-4460-a392-bac4034ee995)
   

   ![top customers - Excel 28_02_2024 03_13_50 (2)](https://github.com/RekkyAbdul/DvdRental-Movie-Performance-/assets/149950425/5b5bdb78-d1ee-4fb3-8937-9fc4ee311ed3)


    ![store - Excel 28_02_2024 03_18_38 (2)](https://github.com/RekkyAbdul/DvdRental-Movie-Performance-/assets/149950425/cd369cb2-0d6e-40c1-95f2-bd61195957ba)


   



#### Recommendations

- For the business to generate more revenue, they should invest more in movies rated PG-13 and featured by actors/actresses such as
  Susan Davis, Gina Degeneres, Walter Torn, Mary Keitel, and Matthew Carrey.
- Implementing an effective inventory management system to balance products that are in high demand and low demand. Focusing more resources on
  movies with high demand.
