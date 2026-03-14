
# Порядок операций в SQL

```
from
where
group by
having
select
order by
limit
```

## базовый запрос

```sql
SELECT *
FROM tickets
```

## фильтрация WHERE фильтрует строки до группировки

```sql
SELECT *
FROM tickets
WHERE status='OPEN'
```

## Вложенный запрос

```sql
SELECT * FROM orders
WHERE customer_id IN (SELECT DISTINCT customer_id FROM orders WHERE event_name='refund')
```

## Неточное соответствие по условию

```sql
SELECT * FROM orders
WHERE event_name LIKE 'purchase%'

SELECT * FROM orders
WHERE device LIKE 'Samsung%'
```

## Исключаем нулевые значения

```sql
SELECT * FROM orders
WHERE event_name IS NOT NULL
```

## Агрегирующие функции (COUNT SUM MIN MAX AVERAGE)

```sql
SELECT COUNT(DISTINCT customer_id)
FROM orders

SELECT AVG(Revenue)
FROM orders
WHERE country IN ('Russia', 'USA', 'Canada')

SELECT country, SUM(Revenue)
FROM orders
GROUP BY country
ORDER BY 2 DESC (Сортировка по 2 столбцу из SELECT в убывающем порядке (обратном))
```

## группировка

```sql
SELECT status, COUNT(*)
FROM tickets
GROUP BY status

SELECT country, COUNT(customer_id) AS Users
FROM orders
GROUP BY country

SELECT country, ROUND(AVG(revenuse)) AS Avg_Revenuse
FROM orders
GROUP BY country

HAVING (используется для фильтрации сгруппированных данных, позволяя применять условия к агрегатным функциям)

SELECT country, SUM(revenue) FROM orders
GROUP BY country
HAVING SUM(Revenue)>1000
ORDER BY 2 DESC
```

## Соединение

```sql
SELECT t.id, u.name
FROM tickets t
JOIN users u ON t.user_id=u.id
```

> Оконные функции (LEAD, LAG, ROW NUMBER, RANK)

```sql
Найти количество заявок каждого пользователя.
есть таблицы
users
tickets

SELECT user_id, COUNT(*)
FROM tickets
GROUP BY user_id
```