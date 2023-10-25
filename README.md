# Coding 4 Assignment
This assignment involved updating the code from coding assignment 2 to utilize a client-server system. The following are the updates and additions:

## Changes to DataAdapter.java:
### loadProduct() Function:
The function establishes a connection to the server. It then sends a "GET_PRODUCT" request to receive product information based on an ID. It then receives the product details from the server and creates a Product object with this information, or returns null if the product is not found.

### saveProduct() Function:
The function first calls the loadProduct() function to see if the product already exists in the database. If the product already exists, the function sends an "UPDATE_PRODUCT" request to the server and then subsequently sends the product information to the server to update. If the product does not exist, the function sends an "INSERT_PRODUCT" request to the server and then subsequently sends the product information to the server to insert into the database.

### saveOrder() Function:
The function establishes a connection to the server. It then sends a "SAVE_ORDER" request to the server and then subsequently sends the order information to the server to save in the Order table and the receipt string to save in the Receipt table. It then goes through each orderline and sends a "SAVE_ORDERLINE" request to the server and then subsequently sends the orderline information to be stored in the orderline table.

### loadProduct() Function:
The function establishes a connection to the server. It then sends a "GET_USER" request to receive user information based on a username and a password. It then receives the user details from the server and creates User object with this information, or returns null if the product is not found.

## Addition of Server.java:
Server.java contains the implementation for the following requests:
### GET_PRODUCT
This request expects a product ID and sends back the product name, price, and quantity.
### GET_USER
This request expects a username and password and sends back the user ID, username, password, and display name of the user.
### UPDATE_PRODUCT
This request expects the product name, price, and quantity and updates the information of the product in the database.
### INSERT_PRODUCT
This request expects the product ID, name, price, and quantity and inserts the information of the product in the database.
### SAVE_ORDER
This request expects the buyer ID, card, address, date, cost, and taxes of an order and inserts the information of the order in the database.
### SAVE_ORDERLINE
This request expects the order ID, product ID, quantity, and cost of an orderline and inserts the information of the orderline in the database.

## YouTube Video:
https://youtu.be/jqeAQ9ELHbY


