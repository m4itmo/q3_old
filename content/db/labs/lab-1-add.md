---
title: Доп задания к лабе 1
draft: false
tags:
  - 4o
---

>[!warning] В разработке
>пупупу

[[lab-1|Лабораторная 1]]
[[lab1-info|Доп инфа]]

### 1. Найти и вывести на экран названия продуктов и их цену, у которых цена выше средней цены всех продуктов.

```sql
SELECT name, listprice
FROM production.product
WHERE listprice > (SELECT AVG(listprice) FROM production.product);
```
[[task-0|Пояснение к задаче]]

### 2. Найти и вывести на экран названия и цвета продуктов, у которых цена находится в диапазоне от 200 до 500 включительно и цвет не определен.

```sql
SELECT name, color
FROM production.product
WHERE listprice BETWEEN 200 AND 500 AND color IS NULL;
```

### 3. Найти и вывести на экран названия продуктов, которые были проданы после 2015 года, и у которых цена менее 300.

```sql
SELECT name
FROM production.product
WHERE sellstartdate > '2015-01-01' AND listprice < 300;
```

### 4. Найти и вывести на экран названия и цвет продуктов, которые не имеют установленного размера.

```sql
SELECT name, color
FROM production.product
WHERE size IS NULL;
```

### 5. Найти и вывести на экран первые пять самых дешевых продуктов с определенным цветом и упорядочить по цене по возрастанию.
> Вывести `name`, `color`, `listprice`

```sql
SELECT name, color, listprice
FROM production.product
WHERE color IS NOT NULL
ORDER BY listprice ASC
LIMIT 5;
```

### 6. Найти и вывести на экран продукты, которые имеют уникальный цвет.
> Вывести `name`, `color`

```sql
SELECT DISTINCT ON (color) name, color
FROM production.product
WHERE color IS NOT NULL
ORDER BY color, name;
```

### 7. Найти и вывести на экран названия всех продуктов, у которых длина имени больше 10 символов.

```sql
SELECT name
FROM production.product
WHERE LENGTH(name) > 10;
```

### 8. Найти и вывести на экран названия продуктов, которые начинаются на букву "S" и их цена больше 50.

```sql
SELECT name
FROM production.product
WHERE name LIKE 'S%' AND listprice > 50;
```

### 9. Найти и вывести на экран названия продуктов, которые были в продаже до января 2018 года, но стоимость которых больше 1000.

```sql
SELECT name
FROM production.product
WHERE sellstartdate < '2018-01-01' AND listprice > 1000;
```

### 10. Найти и вывести на экран все продукты, у которых в названии есть слово "Mountain" и их цена превышает среднюю цену всех продуктов.

```sql
SELECT name
FROM production.product
WHERE name LIKE '%Mountain%' AND listprice > (SELECT AVG(listprice) FROM production.product);
```
>Средняя цена считается аналогично задаче 1 [[task-0|Пояснени к задаче 0]]

### 11. Найти и вывести на экран уникальные комбинации цветов и размеров продуктов, которые имеют цену менее 300.

```sql
SELECT DISTINCT color, size
FROM production.product
WHERE listprice < 300;
```

### 12. Найти и вывести на экран названия продуктов, у которых есть размер, и сортировать по цене по убыванию.

```sql
SELECT name
FROM production.product
WHERE size IS NOT NULL
ORDER BY listprice DESC;
```

### 13. Найти и вывести на экран названия продуктов, которые были проданы между 2010 и 2015 годами и цена которых находится между 100 и 400.

```sql
SELECT name
FROM production.product
WHERE sellstartdate BETWEEN '2010-01-01' AND '2015-12-31' AND listprice BETWEEN 100 AND 400;
```

### 14. Найти и вывести на экран названия всех подкатегорий товаров, где количество продуктов больше 5.

> [!info] в процессе

%%
```sql
SELECT product_subcategory.name
FROM production.product
JOIN production.product_subcategory ON production.product.productsubcategoryid = production.product_subcategory.productsubcategoryid
GROUP BY product_subcategory.name
HAVING COUNT(productid) > 5;
```
%%

### 15. Найти и вывести на экран максимальную, минимальную и среднюю цену продуктов для каждого цвета.

```sql
SELECT color, MAX(listprice) AS max_price, MIN(listprice) AS min_price, AVG(listprice) AS avg_price
FROM production.product
GROUP BY color;
```

### 16. Найти и вывести на экран названия продуктов, которые имеют уникальную цену.

> [!info] в процессе

%%
```sql
SELECT name, listprice
FROM production.product
GROUP BY name, listprice
HAVING COUNT(listprice) = 1;
```
%%

### 17. Найти и вывести на экран названия продуктов, которые были в продаже только в 2008 году.

```sql
SELECT name
FROM production.product
WHERE EXTRACT(YEAR FROM sellstartdate) = 2008;
```

### 18. Найти и вывести на экран названия и размеры продуктов, у которых цена больше 50 и размер начинается на "L".

```sql
SELECT name, size
FROM production.product
WHERE listprice > 50 AND size LIKE 'L%';
```

### 19. Найти и вывести на экран названия и цвет продуктов, которые начинаются на "B" и цена которых меньше 150.

```sql
SELECT name, color
FROM production.product
WHERE name LIKE 'B%' AND listprice < 150;
```

### 20. Найти и вывести на экран названия продуктов, у которых цвет "Red", упорядочить по дате продажи по убыванию и вывести первые 10 записей.

```sql
SELECT name
FROM production.product
WHERE color = 'Red'
ORDER BY sellstartdate DESC
LIMIT 10;
```