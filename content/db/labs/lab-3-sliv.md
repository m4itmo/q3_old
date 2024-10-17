---
title: Title
draft: false
created: 2024-10-04T15:53
date: 2024-10-04
tags:
---

1. найти сколько различных размеров товаров приходится на каждую категорию товаров

    ```sql
    SELECT pc.name AS category_name, COUNT(DISTINCT p.size) AS distinct_sizes_count
    FROM production.product AS p

    JOIN production.product_subcategory AS psc
    ON p.product_subcategory_id = psc.product_subcategory_id
    JOIN production.product_category AS pc
    ON psc.product_category_id = pc.product_category_id

    GROUP BY pc.name
    ```

2. Найти названия товаров, которые были проданы хотя бы один раз

    ```sql
    SELECT p.name, COUNT(s.product_id) AS amount
    FROM sales.sales_order_detail AS s
    INNER JOIN production.product AS p
    ON p.product_id = s.product_id
    GROUP BY p.name
    HAVING COUNT(s.product_id) >= 1
    ```

3. Вывести названия всех подкатегорий товаров, в которых есть не менее одного красного товара
    ```SQL
    SELECT ps.name, COUNT (DISTINCT product_id) as red_count
    FROM production.product_subcategory AS ps
    INNER JOIN production.product AS p
    ON p.product_subcategory_id = ps.product_subcategory_id
    WHERE p.color = 'Red'
    GROUP BY ps.name
    HAVING COUNT (DISTINCT product_id) > 1;
    ```
