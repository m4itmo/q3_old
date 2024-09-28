---
title: Сливы 2
draft: false
created: 2024-09-27T12:48
tags:
---

1. вывести подкатегории категории, где больше 5 продуктов

    ```sql
    SELECT productsubcategoryid FROM production.product
    GROUP BY productsubcategoryid
    HAVING COUNT(DISTINCT productid) > 5
    ```

2. вывести цвет, если в базе есть от 2 до 5 товаров такого цвета

    ```sql
    SELECT color, COUNT(*) FROM production.product
    GROUP BY color
    HAVING COUNT(*) <= 5 AND COUNT(*) >= 2
    ```

3. вывести подкатегории, в которых как минимум 5 товаров цвета красный

    ```sql
    SELECT productsubcategoryid
    FROM production.product
    WHERE color = 'Red'
    GROUP BY productsubcategoryid
    HAVING COUNT(*) > 5
    ```

4. Вывести самый продаваемый продукт у которого цена < 100