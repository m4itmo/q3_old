---
title: БД Лаба 2
draft: false
created: 2024-09-24T10:52
tags:
  - mwgn
---

> [!warning] Изменение названий
> _"Коллеги, если кто планирует завтра сдавать лабы, учтите то, что названия столбцов сейчас тоже в snake_case (например, вместо businessentityid теперь business_entity_id), только что заметил это в базе данных"_

[[db-task-lab-2|Полный текст]]
[[lab-2-info|Доп инфа]]

### 1. Найти и вывести на экран количество товаров каждого цвета, исключив из поиска товары, цена которых меньше 30.

```sql
SELECT color, COUNT(*)
FROM production.product
WHERE standardcost >= 30
GROUP BY color
```

### 2. Найти и вывести на экран список, состоящий из цветов товаров, таких, что минимальная цена товара данного цвета более 100.

```sql
SELECT color
FROM production.product
GROUP BY color
HAVING MIN(listprice) > 10
```

### 3. Найти и вывести на экран номера подкатегорий товаров и количество товаров в каждой подкатегории.

```sql
SELECT productsubcategoryid, COUNT(*)
FROM production.product
GROUP BY productsubcategoryid
```

### 4. Найти и вывести на экран номера товаров и количество фактов продаж данного товара (используется таблица SalesORDERDetail).

```sql
SELECT productid, COUNT(*)
FROM sales.sales_order_detail
GROUP BY productid
```

### 5. Найти и вывести на экран номера товаров, которые были куплены более пяти раз.

```sql
SELECT productid
FROM sales.sales_order_detail
GROUP BY productid
HAVING COUNT(*) > 5
```

### 6. Найти и вывести на экран номера покупателей, CustomerID, у которых существует более одного чека, SalesORDERID, с одинаковой датой.

```sql
SELECT customerid, orderdate, COUNT(*)
FROM sales.sales_order_header
GROUP BY customerid, orderdate
HAVING COUNT(*) > 1
```

### 7. Найти и вывести на экран все номера чеков, на которые приходится более трех продуктов.

```sql
SELECT salesorderid
FROM sales.sales_order_detail
GROUP BY salesorderid
HAVING COUNT(productid) > 3
```

### 8. Найти и вывести на экран все номера продуктов, которые были куплены более трех раз.

```sql
SELECT productid
FROM sales.sales_order_detail
GROUP BY productid
HAVING COUNT(*) > 3
```

### 9. Найти и вывести на экран все номера продуктов, которые были куплены или три или пять раз.

```sql
SELECT productid
FROM sales.sales_order_detail
GROUP BY productid
HAVING COUNT(*) = 3 OR COUNT(*) = 5
```

### 10. Найти и вывести на экран все номера подкатегорий, в которых относится более десяти товаров.

```sql
SELECT productsubcategoryid
FROM production.product
GROUP BY productsubcategoryid
HAVING COUNT(*) > 10

SELECT productsubcategoryid
FROM production.product
WHERE productsubcategoryid IS NOT NULL
GROUP BY productsubcategoryid
HAVING COUNT(*) > 10;
```

### 11. Найти и вывести на экран номера товаров, которые всегда покупались в одном экземпляре за одну покупку.

```sql
SELECT productid
FROM sales.sales_order_detail
GROUP BY productid
HAVING MAX(orderqty) = 1
```

### 12. Найти и вывести на экран номер чека, SalesORDERID, на который приходится с наибольшим разнообразием товаров купленных на этот чек.

```sql
SELECT salesorderid
FROM sales.sales_order_detail
GROUP BY salesorderid
ORDER BY COUNT(DISTINCT productid) DESC
LIMIT 1
```

### 13. Найти и вывести на экран номер чека, SalesORDERID с наибольшей суммой покупки, исходя из того, что цена товара – это UnitPrice, а количество конкретного товара в чеке – это ORDERQty.

```sql
SELECT salesorderid
FROM sales.sales_order_detail
GROUP BY salesorderid
ORDER BY SUM(unitprice * orderqty) DESC
LIMIT 1
```

### 14. Определить количество товаров в каждой подкатегории, исключая товары, для которых подкатегория не определена, и товары, у которых не определен цвет.

```sql
SELECT productsubcategoryid, COUNT(*)
FROM production.product
WHERE productsubcategoryid IS NOT NULL AND color IS NOT NULL
GROUP BY productsubcategoryid
```

### 15. Получить список цветов товаров в порядке убывания количества товаров данного цвета.

```sql
SELECT color, COUNT(*)
FROM production.product
GROUP BY color
ORDER BY COUNT(*) DESC
```

### 16. Вывести на экран ProductID тех товаров, что всегда покупались в количестве более 1 единицы на один чек, при этом таких покупок было более двух.

```sql
SELECT productid
FROM sales.sales_order_detail
WHERE productid IS NOT NULL AND orderqty > 1
GROUP BY productid
HAVING COUNT(*) > 2
```
