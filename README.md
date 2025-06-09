# Rockbuster: Rockbuster Stealth Data Analysis Project.

## Executive Summary:
Rockbuster Stealth LLC is a movie rental company that used to have stores around the world. Facing stiff competition from streaming services such as Netflix and Amazon Prime, the Rockbuster Stealth management team is planning to use its existing movie licenses to launch an online video rental service in order to stay competitive. To guide this strategic shift, a comprehensive data analysis was conducted on customer demographics, revenue sources, and film performance.

#### Key Questions:
● Which movies contributed the most/least to revenue gain?  
● What was the average rental duration for all videos?  
● Which countries are Rockbuster customers based in?  
● Where are customers with a high lifetime value based?  
● Do sales figures vary between geographic regions?

#### Objectives:
Help with the launch strategy for the new online video service.

#### Data Set:
This Analysis is based on Rockbuster’s film inventory, customers, and payments data.  

#### Analysis and Methods Used:
● Data Cleaning.  
● Writing moderately complex SQL queries to answer business questions:  
     Creating CTEs, table joins,  and subqueries.  
● Descriptive Analysis using SQL.  
● ERD Creation.  
● Data Dictionary Creation.  
● Creating visualizations in Excel and Tableau.

## Analysis
##### Where are Top customers based?
![Screenshot 2025-05-06 151339](https://github.com/user-attachments/assets/db789552-1657-47f3-805a-65c7bbbc8147)  
● _Countries with the most of customers include India, China, United States, Mexico and so on._  
● _Most of the revenue also come from the top countries where most of the customers are._

##### SQL Querry: 
<pre><code>SELECT
    D.country,
    COUNT(DISTINCT A.customer_id),
    SUM(E.amount)
FROM customer A
FULL JOIN address B ON A.address_id = B.address_id
FULL JOIN city C ON B.city_id = C.city_id
FULL JOIN country D ON C.country_id = D.country_id
FULL JOIN payment E ON E.customer_id = A.customer_id
GROUP BY D.country
ORDER BY COUNT(A.customer_id) DESC
LIMIT 10;</code></pre>

##### Who are the top 10 customers?
![Screenshot 2025-05-06 154016](https://github.com/user-attachments/assets/6271d864-3fd9-4910-8711-166af716aa70)  
● _Customers that have paid the most come from different countries._  
● _TOP paying customers also come from countries with less amount of customers._  
##### SQL Querry: 
<pre><code>SELECT
    A.customer_id,
    A.first_name,
    A.last_name,
    D.country,
    C.city,
    SUM(Z.amount) AS total_amount_paid
FROM payment Z
INNER JOIN customer A ON Z.customer_id = A.customer_id
INNER JOIN address B ON A.address_id = B.address_id
INNER JOIN city C ON B.city_id = C.city_id
INNER JOIN country D ON C.country_id = D.country_id
WHERE A.customer_id IN (
    SELECT customer_id
    FROM payment
    GROUP BY customer_id
    ORDER BY SUM(amount) DESC)
GROUP BY
    A.customer_id,
    A.first_name,
    A.last_name,
    D.country,
    C.city
ORDER BY total_amount_paid DESC
LIMIT 5;</code></pre>

##### Best and Worst Performing Films
![image](https://github.com/user-attachments/assets/4049c97b-b1d6-4230-8135-1ecffcf17043)  
● _The Top 5 films revenue range from $190 to $215 with “Telegraph Voyage” on the lead_  
##### SQL Querry: 
<pre><code>SELECT DISTINCT
    film.film_id,
    film.title,
    category.name AS category,
    SUM(payment.amount) AS revenue
FROM payment
INNER JOIN rental ON payment.rental_id = rental.rental_id
INNER JOIN inventory ON rental.inventory_id = inventory.inventory_id
INNER JOIN film ON inventory.film_id = film.film_id
INNER JOIN film_category ON film_category.film_id = film.film_id
INNER JOIN category ON category.category_id = film_category.category_id
GROUP BY
    film.title,
    film.film_id,
    category
ORDER BY revenue DESC
LIMIT 5;</code></pre>  
![image](https://github.com/user-attachments/assets/b9cd408e-cf31-4509-b82e-a2028e18005f)  
● _The least popular movies brought between $6 to $7 per film._  
##### SQL Querry: 
<pre><code>SELECT DISTINCT
    film.film_id,
    film.title,
    category.name AS category,
    SUM(payment.amount) AS revenue
FROM payment
INNER JOIN rental ON payment.rental_id = rental.rental_id
INNER JOIN inventory ON rental.inventory_id = inventory.inventory_id
INNER JOIN film ON inventory.film_id = film.film_id
INNER JOIN film_category ON film_category.film_id = film.film_id
INNER JOIN category ON category.category_id = film_category.category_id
GROUP BY
    film.title,
    film.film_id,
    category
ORDER BY revenue ASC
LIMIT 5;</code></pre>

##### Rental Rates and Replacement Costs
![Screenshot 2025-05-06 170331](https://github.com/user-attachments/assets/a7283e07-4018-41f8-b057-678df826eeda)  
● _Majority of the films Rockbuster provides are affordable._  
● _The number of films that are expensive are the second highest._  
##### SQL Querry: 
<pre><code>SELECT
    rental_rate,
    COUNT(film_id) AS count_of_films
FROM film
GROUP BY rental_rate;</code></pre>

![Screenshot 2025-05-06 170315](https://github.com/user-attachments/assets/6cc833a7-0c93-411e-8414-f096a5749e0c)  
● _Replacement cost of films are on the higher side having majority of the films above $20._  
● _There are only a few film that are as cheap or less than $10._  
##### SQL Querry: 
<pre><code>SELECT
    COUNT(CASE WHEN replacement_cost BETWEEN 0 AND 10 THEN 1 END) AS replacement_cost_below10,
    COUNT(CASE WHEN replacement_cost BETWEEN 10 AND 20 THEN 1 END) AS replacement_cost10to20,
    COUNT(CASE WHEN replacement_cost > 20 THEN 1 END) AS replacement_cost_above20
FROM film;</code></pre>

## Recommendations:
● Focus marketing effort on Top 5-10 countries as they bring the most revenue Majority of the Top countries are rich countries.  
● To get more customers, decrease the renting price and replacement based on the country. eg. discord’s Nitro subscription.  
● Make the replacement cost cheaper and in turn increase the rental price to increase customer loyalty.  

## Next Steps:
● Develop targeted marketing campaigns. for Top customers and Top regions with the marketing team.  
● Explore partnerships and licensing deals in highest traffic locations.  

## Sources And Links:
[Rockbuster Data Zip file](http://www.postgresqltutorial.com/wp-content/uploads/2019/05/dvdrental.zip)  
[Tableau Dashboard](https://public.tableau.com/app/profile/faisal.malekzada/viz/RockbusterFilms/RockbusterCustomerDemographics)
