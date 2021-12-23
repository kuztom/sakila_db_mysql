### Sakila Database
MySQL exercises using [Sakila Sample Database](https://dev.mysql.com/doc/sakila/en/) <br>
DB structure [here](https://dev.mysql.com/doc/sakila/en/sakila-structure-tables.html). <br>
<br>

* 1a. Display the first and last names of all actors from the table `actor`. <br>
  `SELECT first_name, last_name ` <br>`FROM actor;` <br><br>

* 1b. Display the first and last name of each actor in a single column in upper case letters. Name the column `Actor Name`.<br>
  `SELECT CONCAT(first_name, ' ', last_name) as 'Actor Name'` <br>`FROM actor;`<br><br>

* 2a. You need to find the ID number, first name, and last name of an actor, of whom you know only the first name, "Joe." What is one query would you use to obtain this information?
  <br>`SELECT actor_id, first_name, last_name`<br>
  `FROM actor`<br>
  `WHERE first_name LIKE  "Joe";`<br><br>

* 2b. Find all actors whose last name contain the letters `GEN`:<br>
  `SELECT *`<br>
  `FROM actor`<br>
  `WHERE last_name LIKE "%gen%"`<br><br>

* 2c. Find all actors whose last names contain the letters `LI`. This time, order the rows by last name and first name, in that order:<br>
  `SELECT *`<br>
  `FROM actor`<br>
  `WHERE last_name LIKE "%li%"`<br>
  `ORDER BY last_name`<br>
  `AND first_name;`<br><br>

* 2d. Using `IN`, display the `country_id` and `country` columns of the following countries: Afghanistan, Bangladesh, and China:
  <br>`SELECT country_id, country`<br>
  `FROM country`<br>
  `WHERE country IN ('Afghanistan', 'Bangladesh', 'China')`<br><br>

* 3a. Add a `middle_name` column to the table `actor`. Position it between `first_name` and `last_name`. Hint: you will need to specify the data type.
  <br>`ALTER TABLE actor`<br>
  `ADD COLUMN middle_name VARCHAR(255) AFTER first_name`<br><br>

* 3b. You realize that some of these actors have tremendously long last names. Change the data type of the `middle_name` column to `blobs`.
  <br>`ALTER TABLE actor`<br>
  `MODIFY middle_name BLOB`<br><br>

* 3c. Now delete the `middle_name` column.<br>
  `ALTER TABLE actor`<br>
  `DROP middle_name`<br><br>

* 4a. List the last names of actors, as well as how many actors have that last name.<br>
  `SELECT last_name, COUNT(*)`<br>
  `FROM actor`<br>
  `GROUP BY last_name`<br><br>

* 4b. List last names of actors and the number of actors who have that last name, but only for names that are shared by at least two actors<br>
  `SELECT last_name, COUNT(*) AS amount`<br>
  `FROM actor`<br>
  `GROUP BY last_name`<br>
  `HAVING amount > 1`<br><br>

* 4c. Oh, no! The actor `HARPO WILLIAMS` was accidentally entered in the `actor` table as `GROUCHO WILLIAMS`, the name of Harpo's second cousin's husband's yoga teacher. Write a query to fix the record.<br>
`UPDATE actor`<br>
`SET first_name = 'HARPO'`<br>
`WHERE first_name = 'GROUCHO'` <br>`AND last_name = 'Williams'`<br><br>

* 4d. Perhaps we were too hasty in changing `GROUCHO` to `HARPO`. It turns out that `GROUCHO` was the correct name after all! In a single query, if the first name of the actor is currently `HARPO`, change it to `GROUCHO`. Otherwise, change the first name to `MUCHO GROUCHO`, as that is exactly what the actor will be with the grievous error. BE CAREFUL NOT TO CHANGE THE FIRST NAME OF EVERY ACTOR TO `MUCHO GROUCHO`, HOWEVER! (Hint: update the record using a unique identifier.)<br>
`UPDATE actor`<br>
`SET first_name = CASE`<br>
`WHEN first_name = 'HARPO' THEN 'GROUCHO'`<br>
`WHEN first_name = 'GROUCHO' THEN 'MUCHO GROUCHO'`<br>
`END`<br>
`WHERE actor_id = 172`<br><br>

* 5a. You cannot locate the schema of the `address` table. Which query would you use to re-create it?<br>
`DESCRIBE address`<br><br>

* 6a. Use `JOIN` to display the first and last names, as well as the address, of each staff member. Use the tables `staff` and `address`:
  <br>`SELECT staff.first_name, staff.last_name, address.address`<br>
  `FROM staff`<br>
  `JOIN address`<br>
  `ON address.address_id = staff.address_id`<br><br>

* 6b. Use `JOIN` to display the total amount rung up by each staff member in August of 2005. Use tables `staff` and `payment`.
  <br>`SELECT staff.first_name, staff.last_name, SUM(payment.amount) total`<br>
  `FROM staff`<br>
  `JOIN payment`<br>
  `ON staff.staff_id = payment.staff_id`<br>
  `GROUP BY payment.staff_id`<br>
  `ORDER BY total DESC`<br><br>

* 6c. List each film and the number of actors who are listed for that film. Use tables `film_actor` and `film`. Use inner join.
  <br>`SELECT film.title, COUNT(film_actor.actor_id) actors_in_movie`<br>
  `FROM film`<br>
  `JOIN film_actor ON film_actor.film_id = film.film_id`<br>
  `GROUP BY film.title`<br><br>

* 6d. How many copies of the film `Hunchback Impossible` exist in the inventory system?
  <br>`SELECT film.title, COUNT(inventory.film_id) copies_in_inventory`<br>
  `FROM film`<br>
  `JOIN inventory ON inventory.film_id = film.film_id`<br>
  `WHERE film.title = 'Hunchback Impossible'`<br><br>


* 6e. Using the tables `payment` and `customer` and the `JOIN` command, list the total paid by each customer. List the customers alphabetically by last name:
  <br>`SELECT customer.first_name, customer.last_name, SUM(payment.amount) total`<br>
  `FROM customer`<br>
  `JOIN payment`<br>
  `ON payment.customer_id = customer.customer_id`<br>
  `GROUP BY customer.customer_id`<br>
  `ORDER BY last_name`<br>