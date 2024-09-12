---
title: БД Лаба 1
draft: false
tags:
  - qa_passed
---

[[db-task-lab-1|Полный текст]]
[[db-lab-1-add|Доп задания]]
[[db-lab1-info|Доп инфа]]

### 1. Найти и вывести на экран названия продуктов, их цвет и размер.

```sql
SELECT name, color, size
FROM production.product;
```

### 2. Найти и вывести на экран названия, цвет и размер таких продуктов, у которых цена более 100.

```sql
SELECT name, color, size
FROM production.product
WHERE standardcost > 100;
```

### 3. Найти и вывести на экран название, цвет и размер таких продуктов, у которых цена менее 100 и цвет Black.

```sql
SELECT name, color, size
FROM production.product
WHERE standardcost < 100 AND color = 'Black';
```
>`standardcost` вместо `price`

### 4. Найти и вывести на экран название, цвет и размер таких продуктов, у которых цена менее 100 и цвет Black, упорядочив вывод по возрастанию стоимости продуктов.

```sql
SELECT name, color, size
FROM production.product
WHERE standardcost < 100 AND color = 'Black'
ORDER BY standardcost ASC;
```

### 5. Найти и вывести на экран название и размер первых трех самых дорогих товаров с цветом Black.

```sql
SELECT name, size
FROM production.product
WHERE color = 'Black'
ORDER BY standardcost DESC
LIMIT 3;
```

### 6. Найти и вывести на экран название и цвет таких продуктов, для которых определен и цвет, и размер.

```sql
SELECT name, color
FROM production.product
WHERE color IS NOT NULL AND size IS NOT NULL;
```

### 7. Найти и вывести на экран не повторяющиеся цвета продуктов, у которых цена находится в диапазоне от 10 до 50 включительно.

```sql
SELECT DISTINCT color
FROM production.product
WHERE standardcost BETWEEN 10 AND 50;
```

### 8. Найти и вывести на экран все цвета таких продуктов, у которых в имени первая буква ‘L’ и третья ‘N’.

```sql
SELECT color
FROM production.product
WHERE name LIKE 'L_N%';
```

> При использовании данной команды в бд курса результат будет содержать 0 строк из-за отсутствия имён в которой третья буква это заглавная N а субд является case sensitive то есть учитывает регистр
> Для получения ожидаемых результатов можно использовать `'L_n%'`

```sql
SELECT color FROM production.product WHERE name LIKE 'L_n%';
```

### 9. Найти и вывести на экран названия таких продуктов, которые начинаются либо на букву ‘D’, либо на букву ‘M’, и при этом длина имени – более трех символов.

```sql
SELECT name
FROM production.product
WHERE (name LIKE 'D%' OR name LIKE 'M%')
  AND LENGTH(name) > 3;
```

### 10. Вывести на экран названия продуктов, у которых дата начала продаж – не позднее 2012 года.

```sql
SELECT name
FROM production.product
WHERE sellstartdate <= '2012-12-31';
```

### 11. Найти и вывести на экран названия всех подкатегорий товаров.

```sql
SELECT name
FROM production.product_subcategory;
```

### 12. Найти и вывести на экран названия всех категорий товаров.

```sql
SELECT name
FROM production.product_category;
```

### 13. Найти и вывести на экран имена всех клиентов из таблицы Person, у которых обращение (Title) указано как «Mr.».

```sql
SELECT firstname, lastname
FROM person.person
WHERE title = 'Mr.';
```

### 14. Найти и вывести на экран имена всех клиентов из таблицы Person, для которых не определено обращение (Title).

```sql
SELECT firstname, lastname
FROM person.person
WHERE title IS NULL;
```

### 15. Получить все названия товаров в системе, в названии которых третий символ – либо буква “s”, либо буква “r”. Решить задачу как минимум двумя способами.

#### Способ 1 (с использованием `LIKE`):

```sql
SELECT name
FROM production.product
WHERE name LIKE '__s%' OR name LIKE '__r%';
```

#### Способ 2 (с использованием `SUBSTRING`):

```sql
SELECT name
FROM production.product
WHERE SUBSTRING(name, 3, 1) IN ('s', 'r');
```

### 16. Получить все названия товаров в системе, в названии которых ровно 5 символов.

```sql
SELECT name
FROM production.product
WHERE LENGTH(name) = 5;
```

### 17. Написать запрос, который возвращает названия товаров, которые были в продаже между мартом 2011 года и мартом 2012 года включительно.

```sql
SELECT name
FROM production.product
WHERE sellstartdate BETWEEN '2011-03-01' AND '2012-03-31';
```

### 18. Найти максимальную стоимость товара (отпускная цена ListPrice) из тех, которые были произведены, начиная с марта 2011 года.

```sql
SELECT MAX(listprice)
FROM production.product
WHERE sellstartdate >= '2011-03-01';
```

[[db-lab-1-add|Доп задания]]