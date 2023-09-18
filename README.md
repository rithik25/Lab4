# Coding 1 Assignment

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

## Youtube Screen Recordings
