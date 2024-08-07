## SQL Query Explanation

### Objective

The purpose of the SQL query is to identify and list users who have made at least 3 orders in the "Electronics" category and have spent more than $1000 on these orders. The output includes the user's name, email, and the total amount they have spent on Electronics orders, sorted by the total amount spent in descending order.

### Database Schema Assumptions

The database consists of three tables:

1. **`users`**
    - `id` (INTEGER): Unique identifier for each user.
    - `name` (VARCHAR): Name of the user.
    - `email` (VARCHAR): Email address of the user.

2. **`orders`**
    - `id` (INTEGER): Unique identifier for each order.
    - `user_id` (INTEGER): Identifier of the user who placed the order.
    - `product_id` (INTEGER): Identifier of the product ordered.
    - `quantity` (INTEGER): Quantity of the product ordered.
    - `created_at` (TIMESTAMP): Timestamp when the order was created.

3. **`products`**
    - `id` (INTEGER): Unique identifier for each product.
    - `name` (VARCHAR): Name of the product.
    - `price` (DECIMAL): Price of the product.
    - `category` (VARCHAR): Category of the product (e.g., Electronics, Clothing, etc.).

### SQL Query

```sql
SELECT
    u.name,
    u.email,
    SUM(p.price * o.quantity) AS total_spent
FROM
    users u
JOIN
    orders o ON u.id = o.user_id
JOIN
    products p ON o.product_id = p.id
WHERE
    p.category = 'Electronics'
GROUP BY
    u.id, u.name, u.email
HAVING
    COUNT(o.id) >= 3
    AND SUM(p.price * o.quantity) > 1000
ORDER BY
    total_spent DESC;
