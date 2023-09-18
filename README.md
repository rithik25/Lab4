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

## Youtube Screen Recordings

The file explorer is accessible using the button in left corner of the navigation bar. You can create a new file by clicking the **New file** button in the file explorer. You can also create folders by clicking the **New folder** button.
