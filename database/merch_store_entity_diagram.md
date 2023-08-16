## Design a database schema for an online merch store.

Designing a database schema for an online merch store requires considering various entities like products, users, orders, and reviews. Here's a simple schema for such a store:

1. **Users**
    - `user_id` (Primary Key)
    - `username`
    - `email`
    - `password_hash`
    - `first_name`
    - `last_name`
    - `address`
    - `city`
    - `state`
    - `postal_code`
    - `country`
    - `phone_number`
    - `date_created`
    - `last_login`

2. **Products**
    - `product_id` (Primary Key)
    - `product_name`
    - `description`
    - `price`
    - `stock_quantity`
    - `category_id` (Foreign Key)
    - `image_url`
    - `date_added`

3. **Product_Categories**
    - `category_id` (Primary Key)
    - `category_name`
    - `description`

4. **Orders**
    - `order_id` (Primary Key)
    - `user_id` (Foreign Key)
    - `total_price`
    - `order_date`
    - `shipping_address`
    - `shipping_city`
    - `shipping_state`
    - `shipping_postal_code`
    - `shipping_country`
    - `order_status`

5. **Order_Items**
    - `order_item_id` (Primary Key)
    - `order_id` (Foreign Key)
    - `product_id` (Foreign Key)
    - `quantity`
    - `price_each`

6. **Reviews**
    - `review_id` (Primary Key)
    - `product_id` (Foreign Key)
    - `user_id` (Foreign Key)
    - `rating` (e.g., 1-5)
    - `review_text`
    - `date_posted`

7. **Cart**
    - `cart_id` (Primary Key)
    - `user_id` (Foreign Key)
    - `date_created`

8. **Cart_Items**
    - `cart_item_id` (Primary Key)
    - `cart_id` (Foreign Key)
    - `product_id` (Foreign Key)
    - `quantity`

**Relationships:**

- A `User` can have multiple `Orders` but each `Order` belongs to a single `User`.
- A `Product` can belong to one `Product_Category`, and each `Product_Category` can have multiple `Products`.
- An `Order` can have multiple `Order_Items`, but each `Order_Item` belongs to one `Order`.
- A `Product` can have multiple `Reviews`, but each `Review` belongs to one `Product`.
- A `User` can create multiple `Reviews`, but each `Review` is created by one `User`.
- A `User` can have one `Cart`, and a `Cart` can have multiple `Cart_Items`.

This schema is a basic representation, and depending on the specific requirements and complexities of the online store, there might be a need for additional tables or fields. Proper indexing, constraints like uniqueness, and setting up appropriate data validation rules are also vital for the store's performance and data integrity.
