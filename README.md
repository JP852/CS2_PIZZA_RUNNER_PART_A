# CS2_PIZZA_RUNNER_PART_A

#### Link to Challenge

https://blog.devgenius.io/danny-mas-sql-case-study-2-pizza-runner-solutions-38c90e3bb6ad

#### SQL Techniques Used

- Table Joins
- Aggregation
- Group By
- Order By
- Limit
- Where Clause
- Is Not Null Function

## Questions

### Question 1 - How many pizzas were ordered?

```
SELECT 
    COUNT(ORDER_ID) AS PIZZAS_ORDERED
        FROM CUSTOMER_ORDERS

```

### Question 2 - How many unique customer orders were made?

```
SELECT 
    COUNT(DISTINCT order_id) as unique_orders 
        FROM customer_orders

```

### Question 3 - How many successful orders were delivered by each runner?

```
SELECT 
    COUNT (DISTINCT RO.ORDER_ID) AS SUCCESSFUL_ORDERS
    ,RO.RUNNER_ID
        FROM RUNNER_ORDERS AS RO
            INNER JOIN CUSTOMER_ORDERS AS CO
            ON CO.ORDER_ID = RO.ORDER_ID
                WHERE PICKUP_TIME != 'null'
                    GROUP BY RO.RUNNER_ID

```

### Question 4 - How many of each type of pizza was delivered?

```
SELECT 
    COUNT(CO.PIZZA_ID) AS SUCCESSFUL_ORDERS
    ,PN.PIZZA_NAME
        FROM RUNNER_ORDERS AS RO
            INNER JOIN CUSTOMER_ORDERS AS CO
            ON CO.ORDER_ID = RO.ORDER_ID
                INNER JOIN PIZZA_NAMES AS PN
                ON CO.PIZZA_ID = PN.PIZZA_ID
                    WHERE PICKUP_TIME != 'null'
                        GROUP BY PN.PIZZA_NAME

```

### Question 5 - How many Vegetarian and Meatlovers were ordered by each customer?

```
SELECT 
    COUNT(CO.PIZZA_ID) AS CUSTOMER_ORDERS
    ,PN.PIZZA_NAME
    ,CO.CUSTOMER_ID
        FROM CUSTOMER_ORDERS AS CO
            INNER JOIN PIZZA_NAMES AS PN
            ON CO.PIZZA_ID = PN.PIZZA_ID
                GROUP BY PN.PIZZA_NAME
                ,CO.CUSTOMER_ID

```

### Question 6 - What was the maximum number of pizzas delivered in a single order?

```
SELECT 
    COUNT(CO.PIZZA_ID) AS NUMBER_OF_PIZZAS
    ,CO.ORDER_ID
        FROM RUNNER_ORDERS AS RO
            INNER JOIN CUSTOMER_ORDERS AS CO
            ON CO.ORDER_ID = RO.ORDER_ID
                WHERE PICKUP_TIME != 'null'
                    GROUP BY CO.ORDER_ID
                        ORDER BY COUNT(CO.PIZZA_ID) DESC
                            LIMIT 1

```

### Question 7 - For each customer, how many delivered pizzas had at least 1 change and how many had no changes?

```
SELECT
    CO.CUSTOMER_ID
    ,SUM(CASE WHEN((EXCLUSIONS<>'null' AND LENGTH(EXCLUSIONS)>0) OR (EXTRAS<>'null' AND EXTRAS IS NOT NULL AND LENGTH(EXTRAS)>0))=TRUE THEN 1 ELSE 0 END) AS CHANGES
    ,SUM(CASE WHEN((EXCLUSIONS<>'null' AND LENGTH(EXCLUSIONS)>0) OR (EXTRAS<>'null' AND EXTRAS IS NOT NULL AND LENGTH(EXTRAS)>0))=TRUE THEN 0 ELSE 1 END) AS NO_CHANGES
        FROM RUNNER_ORDERS AS RO
            INNER JOIN CUSTOMER_ORDERS AS CO
            ON CO.ORDER_ID = RO.ORDER_ID
                WHERE PICKUP_TIME != 'null'
                    GROUP BY CO.CUSTOMER_ID

```

### Question 8 - How many pizzas were delivered that had both exclusions and extras?

```
SELECT 
    CO.ORDER_ID
    ,CO.CUSTOMER_ID
    ,EXCLUSIONS
    ,EXTRAS
    ,(EXCLUSIONS<>'null' AND LENGTH(EXCLUSIONS)>0) AND (EXTRAS<>'null' AND EXTRAS IS NOT NULL AND LENGTH(EXTRAS)>0) AS BOTH_OF
        FROM RUNNER_ORDERS AS RO
            INNER JOIN CUSTOMER_ORDERS AS CO
            ON CO.ORDER_ID = RO.ORDER_ID
                WHERE PICKUP_TIME != 'null'
                AND BOTH_OF = TRUE

```

### Question 9 - What was the total volume of pizzas ordered for each hour of the day?

```
SELECT 
    COUNT(PIZZA_ID) AS NUMBER_OF_PIZZAS
    ,(DATE_PART('hour',ORDER_TIME)) AS HOUR
        FROM CUSTOMER_ORDERS
            GROUP BY HOUR

```

### Question 10 - What was the volume of orders for each day of the week?

```
SELECT 
    COUNT(ORDER_ID) AS NUMBER_OF_ORDERS
        ,DAYNAME(ORDER_TIME) AS DAY
            FROM CUSTOMER_ORDERS
                GROUP BY DAY

```

