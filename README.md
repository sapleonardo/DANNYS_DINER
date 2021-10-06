# DANNYS_DINER
![Diners_Image](https://user-images.githubusercontent.com/85455439/135335226-bd3bc303-be21-4188-baf6-ef1378d93f85.jpg)

# The Situation: 

###### Brand new restaurant [ Dannys Diner ] needs assistance understanding their data in order to keep their business running 

# The Objectives: 

###### Strengthen the relationship betweeen the business and their customers via returning specific data-based criteria 

###### Analyze the visiting patterns of the customers 

###### How much customers have spent 

###### Which menu items are the most popular

# What are the three tables which will be analyzed to fullfill the business objectives? 

## Table: Sales ##

```sql
SELECT * FROM dannys_diner.sales 
ORDER BY customer_id DESC; 
```

| customer\_id | order\_date              | product\_id |
| ------------ | ------------------------ | ----------- |
| C            | 2021-01-01T00:00:00.000Z | 3           |
| C            | 2021-01-07T00:00:00.000Z | 3           |
| C            | 2021-01-01T00:00:00.000Z | 3           |

| B | 2021-01-02T00:00:00.000Z | 2 |
| - | ------------------------ | - |
| B | 2021-01-01T00:00:00.000Z | 2 |
| B | 2021-01-04T00:00:00.000Z | 1 |

| A | 2021-01-07T00:00:00.000Z | 2 |
| - | ------------------------ | - |
| A | 2021-01-01T00:00:00.000Z | 1 |
| A | 2021-01-01T00:00:00.000Z | 2 |

###### The Sales Table contains three fields: customer_id, order_date and product_id 
###### The column [customer_id] serves as the identifier for the customer who visited the business in question. The customers are either labeled A, B or C  
###### Column order_date shows the exact DATETIME each customer visited this business 
###### Column product_id shows the item purchased by each customer 

## Table: Menu 

```sql
SELECT * FROM dannys_diner.menu;
```

| product\_id | product\_name | price |
| ----------- | ------------- | ----- |
| 1           | sushi         | 10    |
| 2           | curry         | 15    |
| 3           | ramen         | 12    |

###### The Menu Table contains three fields: product_id, product_name and price 
###### The field product_name showcases what items are available for purchase at the business under analysis
###### Field name price, shows the cost of each item 

## Table: Members 

```sql 
| customer\_id | join\_date               |
| ------------ | ------------------------ |
| A            | 2021-01-07T00:00:00.000Z |
| B            | 2021-01-09T00:00:00.000Z |
```

###### The Members Table returns two fields
###### the customer_id which is the customer in question 
###### Join_date which is the date each customer purchased an item at the business 

# Business Questions: 

## What is the total amount each customer spent at the restaurant? 

```sql
SELECT sales.customer_id, sales.product_id, SUM(menu.price) OVER (
  PARTITION BY customer_id
) AS customer_total 
FROM dannys_diner.sales LEFT JOIN dannys_diner.menu 
ON sales.product_id = menu.product_id 
GROUP BY sales.customer_id, sales.product_id, menu.price
ORDER BY customer_total; 
```

| customer\_id | product\_id | customer\_total |
| ------------ | ----------- | --------------- |
| C            | 3           | 12              |

| A | 2 | 37 |
| - | - | -- |
| A | 3 | 37 |
| A | 1 | 37 |

| B | 2 | 37 |
| - | - | -- |
| B | 3 | 37 |
| B | 1 | 37 |

###### The code above returns the tables which showcase the total amount of money spent amongst customers A, B and C at Dannys Diner 
###### Ex: Customer C spent the least amount of money having only spent a grand total of $12 

## How many days has each customer visited the restaurant? 

```sql
SELECT DISTINCT customer_id AS customer, COUNT(DISTINCT order_date) AS days_visited 
FROM dannys_diner.sales
GROUP BY customer 
ORDER BY days_visited DESC; 
```

| customer | days\_visited |
| -------- | ------------- |
| B        | 6             |
| A        | 4             |
| C        | 2             |

###### The above code returns the table which shows the customers who visited Danny's 

## What was the first item ordered by each customer? 

```sql
SELECT DISTINCT(sales.customer_id) AS customer, MIN(sales.order_date) AS first_day, sales.product_id AS item_id, menu.product_name AS item_ordered
FROM dannys_diner.sales LEFT JOIN dannys_diner.menu
ON sales.product_id = menu.product_id
GROUP BY sales.customer_id, sales.product_id, menu.product_name
ORDER BY sales.customer_id DESC; 
```

| customer | first\_day               | item\_id | item\_ordered |
| -------- | ------------------------ | -------- | ------------- |
| C        | 2021-01-01T00:00:00.000Z | 3        | ramen         |
| B        | 2021-01-01T00:00:00.000Z | 2        | curry         |
| B        | 2021-01-04T00:00:00.000Z | 1        | sushi         |
| B        | 2021-01-16T00:00:00.000Z | 3        | ramen         |
| A        | 2021-01-01T00:00:00.000Z | 1        | sushi         |
| A        | 2021-01-01T00:00:00.000Z | 2        | curry         |
| A        | 2021-01-10T00:00:00.000Z | 3        | ramen         |

###### The above code returns the table which showcases the first item ordered by each customer 
###### Ex: The item first ordered by customer C was ramen 

## What was the most popular item on the menu and how many times was it ordered per customer? 





