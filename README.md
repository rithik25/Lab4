# Coding 6 Assignment
This assignment involved updating the code from coding assignment 3 to make it RESTful. The following are the updates and additions:

## Server: ProductServer.java:
The `ProductServer` program is a server application responsible for handling client connections, processing requests, and interacting with the SQLite database.

### RequestHandler Class:
The RequestHandler class implements the HttpHandler interface to handle incoming HTTP requests for the "/product" endpoint.

### saveProduct() Function:
The function first calls the loadProduct() function to see if the product already exists in the database. If the product already exists, the function sends an "UPDATE_PRODUCT" request to the server and then subsequently sends the product information to the server to update. If the product does not exist, the function sends an "INSERT_PRODUCT" request to the server and then subsequently sends the product information to the server to insert into the database.

### handle() Function:
This function handles the API request by checking the method (either GET or PUT) and calls the appropriate function (handleGetRequest or handleUpdateRequest) to process the request.

### handleGetRequest Function:
This function handles a GET API request to get the information of a particular product depending on the product ID. It extracts the product ID from the request's URI and queries the database to receive the name, quantity, and price of a product. It formats this data either as JSON or HTML depending on the format type requested in the header and sends it back to the client.

### handleupdateRequest Function:
This function handles a PUT API request to update the information of a particular product. It extracts the product ID, name, quantity, and price from the request and updates the corresponding product's information. It sends an appropriate response depending on the success or failure of the operation.

## Clients:

###  QueryClient.java
The `QueryClient` program is a client application that allows users to retrieve product information from the ProductServer.

## UpdateClient.java
The `UpdateClient` program is a client application for updating product information on the ProductServer.

## YouTube Video:
https://youtu.be/gNjJx8KWJc4

