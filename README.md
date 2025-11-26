# Домашнее задание к занятию "`SQL. Часть 2`" - `Нижегородов Сергей`

### Задание 1
Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей, и выведите в результат следующую информацию:
- фамилия и имя сотрудника из этого магазина;
- город нахождения магазина;
- количество пользователей, закреплённых в этом магазине.

*Примечание: В тестовой базе данных условие изменено на "более 3 клиентов" из-за малого объема тестовых данных*

**Решение:**
```sql
SELECT 
    s.first_name AS 'Имя сотрудника',
    s.last_name AS 'Фамилия сотрудника', 
    c.city AS 'Город магазина',
    COUNT(cu.customer_id) AS 'Количество клиентов'
FROM store st
JOIN staff s ON st.manager_staff_id = s.staff_id
JOIN address a ON st.address_id = a.address_id
JOIN city c ON a.city_id = c.city_id
JOIN customer cu ON st.store_id = cu.store_id
GROUP BY st.store_id, s.first_name, s.last_name, c.city
HAVING COUNT(cu.customer_id) > 3;
```

**Скриншот результата:**
![Результат задания 1](task1-result.png)

### Задание 2  
Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

**Решение:**
```sql

SELECT AVG(length) AS 'Средняя продолжительность' FROM film;

SELECT COUNT(*) AS 'Количество фильмов'
FROM film
WHERE length > (SELECT AVG(length) FROM film);

**Скриншот результата:**
![Результат задания 2](task2-avg.png)

### Задание 3
Получите информацию, за какой месяц была получена наибольшая сумма платежей.

sql
SELECT 
    MONTH(payment_date) AS 'Месяц',
    YEAR(payment_date) AS 'Год',
    SUM(amount) AS 'Общая сумма',
    COUNT(*) AS 'Количество платежей'
FROM payment
GROUP BY YEAR(payment_date), MONTH(payment_date)
ORDER BY SUM(amount) DESC
LIMIT 1;

**Скриншот результата:**
![Результат задания 3](task3-result.png)