1) How many users are there?

```
sqlite> SELECT COUNT(*) FROM users;

50
```

2) What are the 5 most expensive items?

```
sqlite> SELECT * FROM items ORDER BY price DESC LIMIT 5;

25|Small Cotton Gloves|Automotive, Shoes & Beauty|Multi-layered modular service-desk|9984
83|Small Wooden Computer|Health|Re-engineered fault-tolerant adapter|9859
100|Awesome Granite Pants|Toys & Books|Upgradable 24/7 access|9790
40|Sleek Wooden Hat|Music & Baby|Quality-focused heuristic info-mediaries|9390
60|Ergonomic Steel Car|Books & Outdoors|Enterprise-wide secondary firmware|9341
```

3) What's the cheapest book? (Does that change for "category is exactly 'book'" versus "category contains 'book'"?)

This is the cheapest item whose category contains 'book':

```
sqlite> SELECT * FROM items WHERE category LIKE '%book%' ORDER BY price ASC LIMIT 1;

76|Ergonomic Granite Chair|Books|De-engineered bi-directional portal|1496
```

This is the command for the cheapest item whose category is exactly 'book' (which doesn't exist):

```
sqlite> SELECT * FROM items WHERE category LIKE 'book' ORDER BY price ASC LIMIT 1;
```

This is the cheapest item whose category is exactly 'books':

```
sqlite> SELECT * FROM items WHERE category LIKE 'books' ORDER BY price ASC LIMIT 1;

76|Ergonomic Granite Chair|Books|De-engineered bi-directional portal|1496
```

4) Who lives at "6439 Zetta Hills, Willmouth, WY"? Do they have another address?

Corrine Little lives here:

```
sqlite> SELECT * FROM users WHERE id = (SELECT user_id FROM addresses WHERE street = '6439 Zetta Hills');

40|Corrine|Little|rubie_kovacek@grimes.net
```

And Corrine has two addresses:

```
sqlite> SELECT * FROM addresses WHERE user_id = (SELECT user_id FROM addresses WHERE street = '6439 Zetta Hills');

43|40|6439 Zetta Hills|Willmouth|WY|15029
44|40|54369 Wolff Forges|Lake Bryon|CA|31587
```

5) Correct Virginie Mitchell's address to "New York, NY, 10108".
Correct and then display the new address:

```
sqlite> UPDATE addresses
   ...> SET city = 'New York', state = 'NY', zip = 10108
   ...> WHERE id = (SELECT id FROM users WHERE first_name = 'Virginie');

sqlite> SELECT * FROM addresses WHERE id = (SELECT id FROM users WHERE first_name = 'Virginie');
```

6) How much would it cost to buy one of each tool?
Cost (sum) of each distinct (one of each) tool:

```
sqlite> SELECT DISTINCT SUM(price) FROM items;

467488
```

7) How many total items did we sell?
Sum of quantities from order table:

```
sqlite> SELECT SUM(quantity) FROM orders;

2125
```

8) How much was spent on books?
Join orders with items on items.id / orders.item_id

```
sqlite> SELECT SUM( price * quantity )
   ...> FROM items
   ...> INNER JOIN orders
   ...> ON items.id = orders.item_id;
   
10045128
```

9) Simulate buying an item by inserting a User for yourself and an Order for that User.


```
sqlite> INSERT INTO users (first_name, last_name, email) VALUES ('Bub', 'Von Bubbles' 'bub@von.bubbles');

sqlite> INSERT INTO orders (user_id, item_id, quantity, created_at) VALUES (51, 24, 8, CURRENT_TIMESTAMP);
```

--------ADVENTURER MODE-------if you cross this line you're an adventurer---------------------
10) What item was ordered most often? Grossed the most money?

```
# MOST OFTEN ORDERED:
# select the item based on id number from items via orders
# sorting orders in this way only displays the first item
# set LIMIT to 39 for all of the most-ordered items

sqlite> SELECT * FROM items WHERE id = (SELECT item_id FROM orders ORDER BY quantity DESC LIMIT 1);

66|Gorgeous Granite Car|Tools & Computers|Enhanced encompassing parallelism|2768

# GROSSED THE MOST MONEY:


```

11) What user spent the most?

```

```

12) What were the top 3 highest grossing categories?

```
```