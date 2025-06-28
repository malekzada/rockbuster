
# 🎬 Rockbuster Stealth | Data Analysis Project

## 📌 Executive Summary
Rockbuster Stealth LLC, a former global DVD rental company, plans to launch an online streaming platform to compete with services like Netflix. To support this shift, data on customer demographics, payments, and film inventory was analyzed using SQL, Excel, and Tableau.

The analysis revealed that the highest revenue came from countries like India, the U.S., and China. Top-paying customers were not always from regions with the most users. Films like Telegraph Voyage generated the most income, while others earned as little as $6. Most films were affordably priced, but replacement costs were often high.

These insights support recommendations to focus marketing on high-value regions, adjust pricing by geography, and reduce replacement costs to improve retention and revenue.

---

## 🧠 Key Questions Explored
- Which movies generated the most/least revenue?
- What is the average rental duration of all movies?
- Where are Rockbuster’s customers located?
- Which regions host high-lifetime-value customers?
- Are there revenue differences across regions?

---

## 📊 Key Insights & Visuals

### 🌍 Top Customer Locations
- Countries with the most customers include **India, China, USA, and Mexico**.
- Revenue trends mirror customer volume.

<img src="https://github.com/user-attachments/assets/db789552-1657-47f3-805a-65c7bbbc8147" width="600"/>

```sql
SELECT D.country, COUNT(DISTINCT A.customer_id), SUM(E.amount)
FROM customer A
FULL JOIN address B ON A.address_id = B.address_id
FULL JOIN city C ON B.city_id = C.city_id
FULL JOIN country D ON C.country_id = D.country_id
FULL JOIN payment E ON E.customer_id = A.customer_id
GROUP BY D.country
ORDER BY COUNT(A.customer_id) DESC
LIMIT 10;
```

---

### 💸 Top Paying Customers
- High-spending customers come from diverse locations.
- Some top contributors are from countries with fewer users.

<img src="https://github.com/user-attachments/assets/6271d864-3fd9-4910-8711-166af716aa70" width="600"/>

```sql
SELECT A.customer_id, A.first_name, A.last_name, D.country, C.city, SUM(Z.amount) AS total_amount_paid
FROM payment Z
INNER JOIN customer A ON Z.customer_id = A.customer_id
INNER JOIN address B ON A.address_id = B.address_id
INNER JOIN city C ON B.city_id = C.city_id
INNER JOIN country D ON C.country_id = D.country_id
GROUP BY A.customer_id, A.first_name, A.last_name, D.country, C.city
ORDER BY total_amount_paid DESC
LIMIT 5;
```

---

### 🎥 Top & Bottom Performing Films

**Best Revenue Generators**  
<img src="https://github.com/user-attachments/assets/4049c97b-b1d6-4230-8135-1ecffcf17043" width="600"/>  

**Lowest Revenue Films**  
<img src="https://github.com/user-attachments/assets/b9cd408e-cf31-4509-b82e-a2028e18005f" width="600"/>

```sql
-- Top 5
SELECT film.title, category.name AS category, SUM(payment.amount) AS revenue
FROM payment
JOIN rental USING (rental_id)
JOIN inventory USING (inventory_id)
JOIN film USING (film_id)
JOIN film_category USING (film_id)
JOIN category USING (category_id)
GROUP BY film.title, category.name
ORDER BY revenue DESC
LIMIT 5;

-- Bottom 5
... ORDER BY revenue ASC LIMIT 5;
```

---

### 💰 Rental & Replacement Costs

**Rental Rates Distribution**
<img src="https://github.com/user-attachments/assets/a7283e07-4018-41f8-b057-678df826eeda" width="500"/>

**Replacement Cost Distribution**
<img src="https://github.com/user-attachments/assets/6cc833a7-0c93-411e-8414-f096a5749e0c" width="500"/>

```sql
-- Rental rate count
SELECT rental_rate, COUNT(film_id) AS count_of_films
FROM film
GROUP BY rental_rate;

-- Replacement cost buckets
SELECT
    COUNT(CASE WHEN replacement_cost <= 10 THEN 1 END) AS "<= $10",
    COUNT(CASE WHEN replacement_cost BETWEEN 10 AND 20 THEN 1 END) AS "$10–$20",
    COUNT(CASE WHEN replacement_cost > 20 THEN 1 END) AS "> $20"
FROM film;
```

---

## ✅ Recommendations
- Focus marketing campaigns in high-revenue countries.
- Adjust pricing strategy based on geography (e.g., dynamic pricing).
- Consider increasing rental fees while lowering replacement cost for user retention.

---

## 🚀 Next Steps
- Launch targeted email or ad campaigns for high-LTV customers.
- Expand catalog partnerships in top-performing regions.
- Optimize movie pricing tiers based on performance metrics.

---
## 🗃️ Dataset
The analysis uses Rockbuster’s internal data on:
- Film inventory
- Customer demographics
- Payments and transactions

📥 [Download dataset (.zip)](http://www.postgresqltutorial.com/wp-content/uploads/2019/05/dvdrental.zip)

---

## 🧰 Tools & Techniques Used

| Tools              | Description                                           |
|-------------------|-------------------------------------------------------|
| **SQL (PostgreSQL)**   | Data cleaning, CTEs, joins, aggregations, subqueries |
| **Excel**          | Pivot tables, basic visualizations                   |
| **Tableau**        | Interactive dashboards, geo heatmaps                 |
| **ERD + Data Dictionary** | Schema understanding and documentation         |

---

## 📊 Interactive Dashboard  
🔗 [Tableau Dashboard – Rockbuster Analysis](https://public.tableau.com/app/profile/faisal.malekzada/viz/RockbusterFilms/RockbusterCustomerDemographics)

