# Coding 6 Assignment
This assignment involved updating the code from coding assignment 3 to make it RESTful. The following are the updates and additions:

## Server: ProductServer.java:
The `ProductServer` program is a server application responsible for handling client connections, processing requests, and interacting with the SQLite database.

### RequestHandler Class:
The RequestHandler class implements the HttpHandler interface to handle incoming HTTP requests for the "/product" endpoint.

### handle() Function:
This function handles the API request by checking their method (either GET or PUT) and calls the appropriate function (handleGetRequest or handleUpdateRequest) to process the request.

### handleGetRequest() Function:
This function handles a GET API request to get the information of a particular product depending on the product ID. It extracts the product ID from the request's URI and queries the database to receive the name, quantity, and price of a product. It formats this data either as JSON or HTML depending on the format type requested in the header and sends it back to the client.

### handleupdateRequest() Function:
This function handles a PUT API request to update the information of a particular product. It extracts the product ID, name, quantity, and price from the request and updates the corresponding product's information. It sends an appropriate response depending on the success or failure of the operation.

### sendResponse() Function:
After the response is processed by handleGetRequest or handleUpdateRequest, this function sends an HTTP response back to the client.

## Clients:

###  QueryClient.java
The `QueryClient` program is a client application that allows users to retrieve product information from the ProductServer. The user inputs the product ID and content type for the response and receives either JSON or HTML output.

## UpdateClient.java
The `UpdateClient` program is a client application for updating product information on the ProductServer. The user inputs the product information either in JSON or HTML format and the program receives status if the update was successful or not.

## YouTube Video:
https://youtu.be/gNjJx8KWJc4

