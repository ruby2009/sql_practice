# sql_practice

How many users are there?
50
SELECT COUNT(* ) FROM users


What are the 5 most expensive items?
1) Small Cotton Gloves 2)Small Wooden Computer, 3)Awesome Granite Pants, 4)Sleek Wooden Hat, 5)Ergonomic Steel Car
SELECT * FROM items ORDER BY price DESC LIMIT 5;


What's the cheapest book? (Does that change for "category is exactly 'book'" versus "category contains 'book'"?)

Ergonomic Granite Chair
SELECT * FROM items WHERE category = 'Books' ORDER BY price LIMIT 1;
Ergonomic Granite Chair, so no it does not change
SELECT * FROM items WHERE category LIKE'%book%' ORDER BY price LIMIT 1;

Who lives at "6439 Zetta Hills, Willmouth, WY"? Do they have another address?
Corrine Little
SELECT users.id, users.first_name, users.last_name, addresses.street, addresses.city, addresses.state FROM users JOIN addresses ON addresses.user_id=users.id WHERE street='6439 Zetta Hills';
Yes she has two addresses. The other is 54369 Wolff Forges in Lake Bryon, CA.
SELECT * FROM addresses WHERE user_id = 40;

Correct Virginie Mitchell's address to "New York, NY, 10108".
I used this to collect Virginie Mitchell's user_id.
SELECT users.id, users.first_name, users.last_name, addresses.street, addresses.city, addresses.state FROM users JOIN addresses ON addresses.user_id=users.id WHERE users.first_name='Virginie';
I used this to update her city, state and zip by her user id.
UPDATE addresses SET city='New York', state='NY', zip=10108 WHERE user_id=39;

How much would it cost to buy one of each tool?
$46,477
SELECT SUM(price) FROM items WHERE category LIKE '%tool%';

How many total items did we sell?
2125
SELECT SUM(quantity) FROM orders;

How much was spent on books?
$1,081,352
SELECT SUM(orders.quantity * items.price) FROM orders JOIN items ON items.id=orders.item_id WHERE category LIKE'%book%';

Simulate buying an item by inserting a User for yourself and an Order for that User.
INSERT INTO users (first_name, last_name, email) VALUES
  ('Ben', 'Call', 'bgcall@indiana.edu');
INSERT INTO orders (user_id, item_id, quantity, created_at) VALUES
  ('51', 66, 1000, CURRENT_TIMESTAMP);

What item was ordered most often? Grossed the most money?
Incredible Granite Car|525240
SELECT items.title, SUM(orders.quantity * items.price) AS total FROM orders JOIN items ON items.id=orders.item_id GROUP BY items.title ORDER BY total DESC LIMIT 1;

What user spent the most?
user_id 19 or Hasan Runte
To find user_id
SELECT orders.user_id, SUM(orders.quantity * items.price) AS total FROM orders JOIN items ON items.id=orders.item_id GROUP BY user_id ORDER BY total DESC LIMIT 1;
To find name.
SELECT * from users WHERE id = 19;

What were the top 3 highest grossing categories?
1) Music, Sports & Clothing|525240
2) Beauty, Toys & Sports|449496
3) Sports|448410
SELECT items.category, SUM(orders.quantity * items.price) AS total FROM orders JOIN items ON items.id=orders.item_id GROUP BY category ORDER BY total DESC LIMIT 3;
