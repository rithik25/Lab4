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
### SQL Statements:
```
PreparedStatement selectStmt = connection.prepareStatement("SELECT Name, Price, Quantity FROM Products WHERE ProductID = ?");
PreparedStatement updateStmt = connection.prepareStatement("UPDATE Products SET Name = ?, Price = ?, Quantity = ? WHERE ProductID = ?");
```
### Infinite loop to handle GET and UPDATE requests:
```
        while (true) {
            Socket clientSocket = serverSocket.accept();
            System.out.println("Connection successful");
            System.out.println("Waiting for input.....");

            BufferedReader in = new BufferedReader(new InputStreamReader(clientSocket.getInputStream()));
            PrintWriter out = new PrintWriter(clientSocket.getOutputStream(), true);

            String operation = in.readLine();
            
            if (operation != null) {
                if (operation.equalsIgnoreCase("GET")) {
                    String productIdStr = in.readLine();
                    int productId = Integer.parseInt(productIdStr);
                    
                    handleGetRequest(productId, selectStmt, out);
                } else if (operation.equalsIgnoreCase("UUPDATE")) {
                    String updateRequest = in.readLine();
                    String[] updateData = updateRequest.split(",");
                    int id = Integer.parseInt(updateData[0]);
                    String updatedName = updateData[1];
                    double updatedPrice = Double.parseDouble(updateData[2]);
                    double updatedQuantity = Double.parseDouble(updateData[3]);
                    
                    handleUpdateRequest(id, updatedName, updatedPrice, updatedQuantity, updateStmt, out);
                } else {
                    out.println("Invalid operation.");
                }
            }

            in.close();
            out.close();
            clientSocket.close();
        }
    }
```

## 2. QueryClient.java

The `QueryClient` program is a client application that allows users to retrieve product information from the ProductServer.

### Code to get a product ID from the user and check if it is a valid product ID:
```
        Scanner scanner = new Scanner(System.in);

        System.out.print("Enter the ProductID: ");
        int productID;
        
        try {
        	productID = scanner.nextInt();
        } catch (Exception e) {
        	System.out.print("Product ID has to be a positive integer");
        	return;
        }
        
        if(productID <= 0) {
        	System.out.print("Product ID has to be a positive integer");
        }
```
### Code to get the product information from the server:
```
        try {
            Socket socket = new Socket("localhost", 7777);
            BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            PrintWriter out = new PrintWriter(socket.getOutputStream(), true);

            out.println("GET");

            out.println(Integer.toString(productID));

            System.out.println("Product ID: " + productID);
            
            String productName = in.readLine();
            String productPrice = in.readLine();
            String productQuantity = in.readLine();
            
            System.out.println(productName);
            System.out.println(productPrice);
            System.out.println(productQuantity);

            in.close();
            out.close();
            socket.close();
            
        } catch (IOException e) {
            e.printStackTrace();
        }
```
## 3. UpdateClient.java

The `UpdateClient` program is a client application for updating product information on the ProductServer.

### Code to get the product ID, name, price, and quantity of product from the user and perform validation checks on them.
```
        Scanner scanner = new Scanner(System.in);
        int productID;
        double newPrice;
        double newQuantity;

        System.out.print("Enter the ProductID to update: ");
        try {
        	productID = scanner.nextInt();
        } catch (Exception e) {
        	System.out.print("Product ID has to be a positive integer");
        	return;
        }
        
        if(productID <= 0) {
        	System.out.print("Product ID has to be positive.");
        	return;
        }

        System.out.print("Enter the new product Name: ");
        String newName = scanner.next();

        System.out.print("Enter the new Price: ");
        try {
        	newPrice = scanner.nextDouble();
        } catch (Exception e) {
        	System.out.print("Price has to be a non-negative floating point number");
        	return;
        }
        if(newPrice < 0) {
        	System.out.print("Price cannot be negative.");
        	return;
        }

        System.out.print("Enter the new Quantity: ");
        try {
        	newQuantity = scanner.nextDouble();
        } catch (Exception e) {
        	System.out.print("Quantity has to be a non-negative floating point number");
        	return;
        }
        
        if(newQuantity < 0) {
        	System.out.print("Quantity cannot be negative.");
        	return;
        }
```
### Code to update the product information on the server:
```
        try {
            Socket socket = new Socket("localhost", 7777);
            BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
            PrintWriter out = new PrintWriter(socket.getOutputStream(), true);

            out.println("UPDATE");

            String updateReq = productID + "," + newName + "," + newPrice + "," + newQuantity;
            out.println(updateReq);

            String result = in.readLine();
            System.out.println("Update Response: " + result);

            in.close();
            out.close();
            socket.close();
            
        } catch (IOException e) {
            e.printStackTrace();
        }
```

## 4. WebServer.java

The `WebServer` program acts as a simple web server, allowing users to access product information via web browsers or HTTP clients.

### Code to get the product information from the server:
```
            Socket connectionSocket = listenSocket.accept();
            BufferedReader inFromClient = new BufferedReader(new InputStreamReader(connectionSocket.getInputStream()));
            DataOutputStream outToClient = new DataOutputStream(connectionSocket.getOutputStream());

            String requestMessageLine = inFromClient.readLine();
            System.out.println("Request: " + requestMessageLine);

            StringTokenizer tokenizedLine = new StringTokenizer(requestMessageLine);
            tokenizedLine.nextToken();
            String uri = tokenizedLine.nextToken();

            if (uri.startsWith("/product/")) {
                String[] uriParts = uri.split("/");
                String id = uriParts[2];

                String name = "";
                double price = 0.0;
                double quantity = 0.0;

                Connection connection = DriverManager.getConnection("jdbc:sqlite:store.db");
                PreparedStatement selectStmt = connection.prepareStatement("SELECT Name, Price, Quantity FROM Products WHERE ProductID = ?");
                selectStmt.setInt(1, Integer.parseInt(id));
                ResultSet result = selectStmt.executeQuery();

                if (result.next()) {
                    name = result.getString("Name");
                    price = result.getDouble("Price");
                    quantity = result.getDouble("Quantity");
                }

                connection.close();
```

### Code to display the product information as a html webpage:
```
                String response = "<html><body>";
                response += "<h1>Product Information</h1>";
                response += "<p>Product ID: " + id + "</p>";
                response += "<p>Product Name: " + name + "</p>";
                response += "<p>Price: " + price + "</p>";
                response += "<p>Quantity: " + quantity + "</p>";
                response += "</body></html>";

                outToClient.writeBytes("HTTP/1.1 200 OK\r\n");
                outToClient.writeBytes("Content-Type: text/html\r\n");
                outToClient.writeBytes("Content-Length: " + response.length() + "\r\n");
                outToClient.writeBytes("\r\n");
                outToClient.writeBytes(response);
            } else {
                String notFoundResponse = "<html><body><h1>Error 404</h1></body></html>";
                outToClient.writeBytes("HTTP/1.1 404 Not Found\r\n");
                outToClient.writeBytes("Content-Type: text/html\r\n");
                outToClient.writeBytes("Content-Length: " + notFoundResponse.length() + "\r\n");
                outToClient.writeBytes("\r\n");
                outToClient.writeBytes(notFoundResponse);
            }
```

## YouTube Video:



