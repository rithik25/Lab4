# Coding 1 Assignment

## Prior Changes
Saving a product was giving an error with the original code. This error was because the sellerID was not being added. The following code in `DataAdapter.java` was changed to fix this error:
```
statement = connection.prepareStatement("INSERT INTO Products VALUES (?, ?, ?, ?, ?)");
statement.setString(2, product.getName());
statement.setDouble(3, product.getPrice());
statement.setDouble(4, product.getQuantity());
statement.setInt(1, product.getProductID());
statement.setInt(5, product.getSellerID());
```
## Combining the View and Controller Packages:
1. `LoginScreen.java` and `LoginController.java` were combined to make `LoginControlView.java`.
2. `OrderView.java` and `OrderController.java` were combined to make `OrderControlView.java`.
3. `ProductView.java` and `ProductController.java` were combined to make `ProductControlView.java`.

## Data Validation

All the data validation changes are in ProductControlView.java.
### Checking if Product ID is not negative in saveProduct():
```
int productID;
try {
    productID = Integer.parseInt(txtProductID.getText());
    if (productID < 0) {
        JOptionPane.showMessageDialog(null, "The Product ID cannot be negative.");
        return;
    }
} catch (NumberFormatException e) {
    JOptionPane.showMessageDialog(null, "Invalid product ID! Please provide a valid product ID!");
    return;
}
```
### Checking if Product Price is not negative in saveProduct():
```
double productPrice;
try {
    productPrice = Double.parseDouble(txtProductPrice.getText());
    if (productPrice < 0) {
        JOptionPane.showMessageDialog(null, "The Product Price cannot be negative.");
        return;
    }
} catch (NumberFormatException e) {
    JOptionPane.showMessageDialog(null, "Invalid product price! Please provide a valid product price!");
    return;
}
```
### Checking if Product Quantity is not negative in saveProduct():
```
double productQuantity;
try {
    productQuantity = Double.parseDouble(txtProductQuantity.getText());
    if (productQuantity < 0) {
        JOptionPane.showMessageDialog(null, "The Product Quantity cannot be negative.");
        return;
    }
} catch (NumberFormatException e) {
    JOptionPane.showMessageDialog(null, "Invalid product quantity! Please provide a valid product quantity!");
    return;
}
```
### Checking if Product ID is not negative in loadProduct():
```
int productID = 0;
try {
    productID = Integer.parseInt(txtProductID.getText());
    if (productID < 0) {
        JOptionPane.showMessageDialog(null, "The Product ID cannot be negative.");
        return;
    }
} catch (NumberFormatException e) {
    JOptionPane.showMessageDialog(null, "Invalid product ID! Please provide a valid product ID!");
    return;
}
```

## YouTube Screen Recording
https://youtu.be/n6I0TfBFjsA
