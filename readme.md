# E-Commerse Database System with EER Model

The project implements the **Amazon's e-commerce database system** which fulfils all the functional requirements for an e-commerce website. The database is created by designing the **Extended Entity Relation (EER)** model of Amazon as an E-commerce website. EER is then converted to **relational tables** using the set of well defined rules and after applying the **normalisation techniques**. Project utilizes **PLSQL** and it's **stored procedures, triggeres and cursors** to create tables and maintain the consistency within the relational database. The database system fulfils the following functional requirements -

**1. A user can register**   – A user can be a buyer or a seller.

**2. A user can place order** - each order contains multiple products.

**3. A seller can add products** - this contains the details of a product.

**4. A buyer can give review** - a user can write reviews about a product.

**5. A user can add products to a Wishlist** - user can store the products which they like but not yet ready to buy.

**6. A user can add products to a shopping cart** - users add to the shopping cart the products they want to buy.

**7. Product can be of multiple category** - it defines the category of a product like clothing, electronics etc.

**8. Product can be carried by different carrier** - the shipping service through which a product can be shipped.

**9. A user can have multiple address and contact details** - users address and phone number saved in the account.

**10. A user can have multiple Card info** - users saved card in the account

**11. A buyer can add images to its review** - each review can have images associated with it.

**12. A seller can add images to its product** - each product can have multiple images associated with it, so taken in separate table.

## ENTITIES AND RELATIONSHIPS

**1. User-contact\_details:** each user can have multiple contact details saved, while each contact detail will have only 1 user linked. Thus, cardinality is 1: N

**2. Buyer-card\_info:** each buyer can have many cards, while each card is associated with 1 buyer.  Thus, cardinality is 1: N

**3. Buyer-order:** buyer places order. A buyer can place many orders, while each order is linked with only 1 buyer. Thus, cardinality is 1: N

**4. Order-product:** each order can contain many products, and each product can come in many orders. Thus, cardinality is M: N

**5. Seller-product:** seller sells products. Each seller can sell many products, while each product has only 1 seller. Thus, cardinality is 1: N

**6. Buyer-reviews:** buyer can write a review. Each buyer can write multiple reviews, while each review has only 1 buyer. Thus, cardinality is 1: N

**7. Review-products:** each review is for 1 product while each product can have many reviews. Thus, cardinality is 1: N

**8. Buyer-wishlist:** each buyer can have only 1 Wishlist, and each Wishlist has 1 buyer. Thus, cardinality is 1:1.

**9. Buyer- shopping cart:** each buyer has only 1 shopping cart while each shopping cart is associated with 1 buyer. Thus, cardinality is 1:1.

**10. Wishlist-product:** each Wishlist contains many products, and each product can be in many wish lists. Thus, cardinality is N:M

**11. Shopping\_cart-product:** each cart contains many products, and each product can be in many carts. Thus, cardinality is N:M

**12. Product-category:** each product has only 1 category while each category has many products. Thus, cardinality is 1: N

**13. Product-carrier:** each product is shipped by 1 carrier, while each carrier ships many products.

**14. Review-review\_images:** each review has many review images while each review image is linked with 1 review. Thus, cardinality is 1: N

**15. Product-product\_images:** each product has many product images while each product image is linked with 1 product. Thus, cardinality is 1: N

**16. Order-card\_info:** each order has a card linked with it, while each card can be used for many orders. Thus, cardinality is 1: N

**17. Order-contact details:** each order has 1 contact detail (address, phone), while each contact detail is linked with multiple orders.  Thus, cardinality is 1: N

- Number 1:1 relationship = 2
- Number of N: M relationships = 3
- Number of 1: N relationships = 12
- Total Relationships = 17

## RELATIONAL SCHEMA

To map ER diagram into a relational schema, we considered the following mapping rules.

For each 1: 1 binary relationship, in the total participation entity add the primary key of the other entity as the foreign key.
For 1: N binary relationship, add to the entity on the N side the primary key of the other entity as the foreign key.
For M: N binary relationship, make a new entity with foreign key as the primary key of the two participating entities. Their combination forms the new primary key.

- In buyer table we have user\_id as foreign key.
- In seller table we have user\_id as foreign key.
- In product table we have seller\_id, carrier\_id and category\_id as foreign keys.
- In order table we have buyer\_id as foreign key.
- In card\_info we have buyer\_id as foreign key.
- In reviews we have buyer\_id, product\_id as foreign keys.
- In wishlist we have buyer\_id as foreign key.
- In shopping cart we have buyer\_id as foreign key.
- In contact\_details we have user\_id as foreign key.
- In review\_images we have review\_id as foreign key.
- In product\_images we have product\_id as foreign key.
- We make a new table name product\_wishlist which as product\_id and wishlist\_id as foreign key.
- We make a new table name product\_shopping\_cart which as product\_id and buyer\_id as foreign key.
- We make a new table name product\_order which as product\_id and order\_id as foreign key.


## SIGNIFICANT PROCEDURES

**1. Register buyer:** invoked by buyer responsible for
- registering user given email, fname, lname and password.
- registering the buyer itself setting its prime membership as false by default and 
    prime member expiry date as Null.

**2. Register seller** has 2 responsibilities
- registering user given email, fname, lname and password.
- registering the seller itself given company name, url, description, setting its average rating  as 2.5 default.

**3. Place order:** Given a buyer id, place order is responsible for the following –
- To iterate over the shopping cart of a particular buyer and remove each item from buyers cart.
- While removing each product, sum up the price of each product to the total price for the order.
- If the user is a prime user, do not include shipping charges for that order.
- Fetch the default address set by the buyer from the list of addresses for that buyer from the contact\_details tables.
- Fetch the default card details set by the buyer from the list of card details for that buyer from the card\_info table.
- To make sure not to include certain products from the shopping car in the order table who's available unity is zero (in\_stock bit is set to 0).
- To add an entry in the order table, containing details of the invoice (total price, total quantity of products in order, tax, shipping charge, card\_details used for the order, delivery contact details used for the order)
- To invokes a trigger responsible for updating the available\_units of each product being bought in that order.

**4. Give review:** A buyer can add a review by providing product\_id, buyer\_id, review, rating, and image\_url (if any). It adds a review in the review table and image of it in the image table. After execution It invokes two triggers.    
- update\_product\_rating
- updte\_seller\_rating.

**5. Add\_contact\_details:** adds details about address and the users number. A user can add multiple contact details and can set a single to contact\_details to be used by default.

**6. Add\_card\_info:** add cards information like card number, expiry\_date etc. A user can add mupltiple card details and can set a single to card\_info to be used by default.

**7. Add\_prodcut:** adds a product sold a by a seller. It also asks for the image URL if any, A seller can upload multiple images for a product.

**8. Add\_to\_shopping\_cart:** adds a product in the shopping cart for a buyer, given the buyer Id

**9. Add\_to\_wishlist:** adds a product to the wishlist of a buyer, given the buyer id.

**10. Update\_membership:** update users prime membership information.

**11. Cancel\_membership:** cancel users prime membership

**12. Populate\_product\_categories:** adds all the available categories of product in it

**13. Populate\_carrier\_categories:** adds all the available carrier serviced responsible for delivering products.


## IMPORTANT TRIGGERS

**1. Update available units:**
- Trigger is invoked whenever a user places an order. Update available unitsis responsible for updating the value of available units for each product in the product table that is being ordered by a buyer.
- The procedures iterate over all the entries of order\_product table for a specific order and iteratively updates each products quantity in the product table.
- If the quantity reaches 0, the product is marked as out of stock, setting its in\_stock value as 0.

**2. Update\_product\_rating:**
- It is responsible for updating the rating of a product every time a buyer gives a review by averaging the earlier rating with this buyers rating.
- It also updates the count of rating given for that particular product.

**3. Update\_seller\_rating**
- It is responsible for updating the rating of a seller every time a buyer gives a review to a product by averaging the earlier rating with this buyers rating.

**4. Remove\_products\_from\_cart**
- After placing an order by a buyer, the trigger is responsible for removing all the ordered products from the shopping cart of that particular buyer.