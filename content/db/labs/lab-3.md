---
title: БД Лаба 3
draft: false
created: 2024-09-28T17:47
description: dmemdlem
date: 2024-10-03
tags:
  - mwgn
---

[[db-task-lab-3|Полный текст]]

### 1. Найти и вывести на экран название продуктов и название категорий товаров, к которым относится этот продукт, с учетом того, что в выборку попадут только товары с цветом Red и ценой не менее 100.

```sql
SELECT p.name, c.name
FROM production.product AS p

JOIN production.product_subcategory AS s
ON p.product_subcategory_id = s.product_subcategory_id
JOIN production.product_category AS c
ON s.product_category_id = c.product_category_id

WHERE p.color = 'Red' AND p.list_price > 100
```

### 2. Вывести на экран названия подкатегорий с совпадающими именами.

```sql
SELECT name, COUNT(*)
FROM production.product_subcategory
GROUP BY name
HAVING COUNT(*) > 1
```

#### С использованием JOIN

```sql
SELECT s1.name AS subcategory_name
FROM production.product_subcategory AS s1
JOIN production.product_subcategory AS s2
ON s1.name = s2.name AND s1.product_subcategory_id <> s2.product_subcategory_id
GROUP BY s1.name
```

> `name = NULL` не учитывается для добавления расширьте `OR s1.name IS NULL`

### 3. Вывести на экран название категорий и количество товаров в данной категории.

```sql
SELECT c.name AS category_name, COUNT(p.product_id) AS product_count
FROM production.product AS p
JOIN production.product_subcategory AS s
ON p.product_subcategory_id = s.product_subcategory_id
JOIN production.product_category AS c
ON s.product_category_id = c.product_category_id
GROUP BY c.name
```

### 4. Вывести на экран название подкатегории, а также количество товаров в данной подкатегории с учетом ситуации, что могут существовать подкатегории с одинаковыми именами.

```sql
SELECT s.name AS subcategory_name, COUNT(p.product_id) AS product_count
FROM production.product AS p
JOIN production.product_subcategory AS s
ON p.product_subcategory_id = s.product_subcategory_id
GROUP BY s.name, s.product_subcategory_id
```

### 5. Вывести на экран название первых трех подкатегорий с небольшим количеством товаров.

```sql
SELECT s.name AS subcategory_name, COUNT(p.product_id) AS product_count
FROM production.product AS p
JOIN production.product_subcategory AS s ON p.product_subcategory_id = s.product_subcategory_id
GROUP BY s.name
ORDER BY COUNT(p.product_id) ASC
LIMIT 3
```

### 6. Вывести на экран название подкатегории и максимальную цену продукта с цветом Red в этой подкатегории.

```sql
SELECT s.name AS subcategory_name, MAX(p.list_price) AS max_price
FROM production.product AS p
JOIN production.product_subcategory AS s ON p.product_subcategory_id = s.product_subcategory_id
WHERE p.color = 'Red'
GROUP BY s.name
```

### 7. Вывести на экран название поставщика и количество товаров, которые он поставляет.

```sql
SELECT v.name AS vendor_name, COUNT(pv.product_id) AS product_count
FROM purchasing.vendor AS v
JOIN purchasing.product_vendor AS pv
ON pv.business_entity_id = v.business_entity_id
GROUP BY v.name
```

### 8. Вывести на экран название товаров, которые поставляются более чем одним поставщиком.

```sql
SELECT p.name AS product_name
FROM production.product AS p
JOIN purchasing.product_vendor AS pv
ON p.product_id = pv.product_id
GROUP BY p.name
HAVING COUNT(*) > 1
```

### 9. Вывести на экран название самого продаваемого товара.

```sql
SELECT p.name AS product_name
FROM sales.sales_order_detail AS sod
JOIN production.product AS p
ON sod.product_id = p.product_id
GROUP BY p.name
ORDER BY SUM(sod.order_qty) DESC
LIMIT 1
```

### 10. Вывести на экран название категории, товары из которой продаются наиболее активно.

```sql
SELECT c.name AS category_name
FROM production.product_category AS c

JOIN production.product_subcategory AS s
ON c.product_category_id = s.product_subcategory_id
JOIN production.product AS p
ON s.product_subcategory_id = p.product_subcategory_id

JOIN sales.sales_order_detail AS sod
ON p.product_id = sod.product_id
GROUP BY c.name
ORDER BY SUM(sod.order_qty) DESC
LIMIT 1
```

### 11. Вывести на экран названия категорий, количество подкатегорий и количество товаров в них.

```sql
SELECT c.name AS category_name, COUNT(DISTINCT s.product_subcategory_id) AS subcategory_count, COUNT(p.product_id) AS product_count
FROM production.product_category AS c
JOIN production.product_subcategory AS s
ON c.product_category_id = s.product_category_id
JOIN production.product AS p
ON s.product_subcategory_id = p.product_subcategory_id
GROUP BY c.name
```

### 12. Вывести на экран номер кредитного рейтинга и количество товаров, поставляемых компаниями, имеющими этот кредитный рейтинг.

### 13. Найти первые 10 процентов самых дорогих товаров, с учетом ситуации, когда цены у некоторых товаров могут совпадать.

>[!info] Не актуальный билет

### 14. Найти первых трех поставщиков, отсортированных по количеству поставляемых товаров, с учетом ситуации, что количество поставляемых товаров может совпадать для разных поставщиков.

```sql
SELECT v.name AS vendor_name, COUNT(pv.product_id) AS product_count
FROM purchasing.vendor AS v
JOIN purchasing.product_vendor AS pv
ON pv.business_entity_id = v.business_entity_id
GROUP BY v.name
ORDER BY product_count DESC
LIMIT 3
```

### 15. Найти для каждого поставщика количество подкатегорий продуктов, к которым относятся продукты, поставляемые им, без учета ситуации, когда продукт не относится ни к какой подкатегории.

```sql
SELECT v.name AS vendor_name, COUNT(DISTINCT p.product_id) AS subcategory_count
FROM purchasing.vendor AS v

JOIN purchasing.product_vendor AS pv
ON v.business_entity_id = pv.business_entity_id
JOIN production.product AS p
ON pv.product_id = p.product_id

GROUP BY v.name
```

### 16. Проверить, есть ли продукты с одинаковым названием, если есть, то вывести эти названия. (Решение через JOIN)

```sql
SELECT p1.name
FROM production.product AS p1
JOIN production.product AS p2 ON p1.name = p2.name AND p1.product_id <> p2.product_id
GROUP BY p1.name
```
