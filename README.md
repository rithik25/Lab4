# Coding 6 Assignment
This assignment involved updating the code from coding assignment 2 to use cloud-based NoSQL databases instead of an SQL database.

## Databases

### Redis:
The Redis database was used to store data for users, products, credit cards, and addresses.
The following lines established a connection to the Redis database:
```
Jedis jedis = new Jedis(<cloud-uri>, <port>);
jedis.auth(<password>);
```

### MongoDB Atlas:
The MongoDB Atlas database was used to store data for orders, orderline, and receipts.
The following lines established a connection to the MongoDB Atlas database:
```
com.mongodb.client.MongoClient mongoClient = MongoClients.create("mongodb+srv://<username>:<password>@cluster0.caekn0p.mongodb.net/?retryWrites=true&w=majority");
MongoDatabase database = mongoClient.getDatabase(<database-name>);
```

## Code Changes

### Application.java:
We do not need a connection parameter while creating an instance of the data adapter since we removed the SQL database.
We now have:
```
dataAdapter = new DataAdapter();
```
instead of:
```
connection = DriverManager.getConnection(url);
dataAdapter = new DataAdapter(connection);
```

### DataAdapter.java:
#### loadProduct() Function:
This function takes in an `id` parameter and searches the Redis database for key `product:id`. If the key is found it retrieves the product data from the database and, creates a product object from it and returns the product object. If the key is not found then it returns null.

#### saveProduct() Function:
This function takes in a `product` object and inserts it into the Redis database. If the product with the same ID already exists in the database then it updates the information. If it doesn't exist then it creates a new key and inserts the data under that key.

#### saveOrder() Function:
This function takes in an `order` object and a receipt string. It finds the max orderID and increments it by 1 to find the new order ID. Using the data provided in the order object, it then inserts the data first in the Orders table and then iterates through the orderlines to insert it in the OrderLine table. The receipt string is inserted in the Receipt table. The Orders, OrderLine, and Receipt tables are stored within the MongoDB Atlas database. Finally, the address and credit card information are stored separately in their respective tables within the Redis database.

#### loadUser() Function:
This function takes in `username` and `password` parameters and searches the Redis database for key `user:username`. If the key is found it retrieves the user data from the database and compares the password of the user to the provided one. If passwords match, the function creates a user object from it and returns the user object. 

## YouTube Video:
https://youtu.be/3QAcjRpjt1g

