# Coding 3 Assignment

## 1. ProductServer.java

The `ProductServer` program is a server application responsible for handling client connections, processing requests, and interacting with the SQLite database.

### Function to handle GET requests by the server:
```
    private static void handleGetRequest(int id, PreparedStatement selectStmt, PrintWriter out) throws SQLException {
        selectStmt.setInt(1, id);
        ResultSet result = selectStmt.executeQuery();

        if (!result.next()) {
            out.println("Invalid Product ID.");
        } else {
            String productName = result.getString("Name");
            double productPrice = result.getDouble("Price");
            double productQuantity = result.getDouble("Quantity");
            out.println("Name: " + productName);
            out.println("Price: " + productPrice);
            out.println("Quantity: " + productQuantity);
        }
    }
```
### Function to handle UPDATE requests by the server:
```
    private static void handleUpdateRequest(int id, String updatedName, double updatedPrice, double updatedQuantity, PreparedStatement updateStmt, PrintWriter out) throws SQLException {
        updateStmt.setString(1, updatedName);
        updateStmt.setDouble(2, updatedPrice);
        updateStmt.setDouble(3, updatedQuantity);
        updateStmt.setInt(4, id);
        int numRows = updateStmt.executeUpdate();

        if (numRows > 0) {
            out.println("Product updated successfully.");
        } else {
            out.println("Failed to update product.");
        }
    }
```


## 2. QueryClient.java

The `QueryClient` program is a client application that allows users to retrieve product information from the ProductServer.

### How it works:
- The client program prompts the user to enter a product ID.
- It establishes a connection with the ProductServer running on the localhost and port 7777.
- The client sends a GET request to the server, along with the product ID.
- The server processes the request and responds with the product's name, price, and quantity.
- The client displays this information to the user.

## 3. UpdateClient.java

The `UpdateClient` program is a client application for updating product information on the ProductServer.

### How it works:
- The client program prompts the user to enter a product ID, the new product name, price, and quantity.
- It establishes a connection with the ProductServer running on the localhost and port 7777.
- The client sends an UPDATE request to the server, along with the formatted string containing the product ID, updated name, price, and quantity.
- The server processes the request and attempts to update the product in the database.
- The client receives a response from the server, indicating whether the update was successful or not.

## 4. WebServer.java

The `WebServer` program acts as a simple web server, allowing users to access product information via web browsers or HTTP clients.

### How it works:
- The server listens on the specified port (default is 8080) for incoming HTTP requests.
- When a request is received, it extracts the URI from the request to determine if it matches the pattern "/product/{productID}".
- If the URI matches, the server queries the SQLite database for the product details based on the product ID provided in the URI.
- It generates an HTML response containing the product's information, such as name, price, and quantity.
- If the product is not found, it returns a 404 error response.


