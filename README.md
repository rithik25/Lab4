# Coding 1 Assignment

## Code changes to display current user logged in, in MainScreen:
In MainScreen.java:
```
private JLabel username = new JLabel("No User");
username.setFont(new Font("Sans Serif", Font.BOLD, 18));
JPanel panelUser = new JPanel();
panelUser.add(username);
this.getContentPane().add(panelUser);

public void updateUsername(String name) {
    username.setText("Welcome " + name);
}
```
In actionPerformed() function in LoginControlView.java:
```
Application.getInstance().getMainScreen().updateUsername(username);
```

## Code changes to add shipping address and credit card to an order:
In Order.java:
```
public void setAddress(String buyerAddress) {
    this.buyerAddress = buyerAddress;
}

public String getAddress() {
    return buyerAddress;
}

public void setCard(String cardNum) {
    this.cardNum = cardNum;
}

public String getCard() {
    return cardNum;
}
```
In makeOrder() function in OderControlView.java:
```
JPanel customerDetailsPanel = new JPanel();
customerDetailsPanel.setLayout(new GridLayout(3, 3));

JTextField customerAddress = new JTextField(30);
customerDetailsPanel.add(new JLabel("Address to ship: "));
customerDetailsPanel.add(customerAddress);

JTextField customerCard = new JTextField(30);
customerDetailsPanel.add(new JLabel("Credit Card Number: "));
customerDetailsPanel.add(customerCard);

int dialogReturn = JOptionPane.showConfirmDialog(null, customerDetailsPanel, "Enter your details: ", JOptionPane.OK_CANCEL_OPTION);

if (dialogReturn == JOptionPane.OK_OPTION) {
    String address = customerAddress.getText();
    String card = customerCard.getText();

    boolean isValid = true;
    for (int i = 0; i < card.length(); i++) {
        if (!Character.isDigit(card.charAt(i))) {
            isValid = false;
        }
    }

    if (address.length() <= 0) {
        JOptionPane.showMessageDialog(null, "Invalid address.");
        return;
    }

    if (order.getLines().size() == 0) {
        JOptionPane.showMessageDialog(null, "Your cart is empty.");
        return;
    }

    if (isValid && card.length() == 16) {
        order.setBuyerID(Application.getInstance().getCurrentUser().getUserID());
        order.setCard(card);
        order.setAddress(address);
        order.setDate(LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyy/MM/dd MM:mm:ss")));
        order.setTotalTax(order.getTotalCost() * 0.08);
        Application.getInstance().getDataAdapter().saveOrder(order, createReceipt());
    } else {
        JOptionPane.showMessageDialog(null, "Invalid credit card number.");
    }
}
```

## Code changes to generate a receipt for each order:
```
public String createReceipt() {
    String receipt = "Order Receipt\n" + "Product ID\t|\t" + "Product Name\t|\t" + "Quantity\t|\t" + "Unit Price\t|\t" + "Total Cost\n";

    for (OrderLine line : order.getLines()) {
        receipt += "\t" + line.getProductID();
        Product prod = Application.getInstance().getDataAdapter().loadProduct(line.getProductID());
        receipt += "\t|\t" + prod.getName();
        receipt += "\t|\t" + line.getQuantity();
        receipt += "\t|\t" + prod.getPrice();
        receipt += "\t|\t" + line.getCost() + "\n";
        prod.setQuantity(prod.getQuantity() - line.getQuantity());
        Application.getInstance().getDataAdapter().saveProduct(prod);
    }

    receipt += "Customer ID: " + order.getBuyerID() + "\n";
    receipt += "Payed with	: " + order.getCard().substring(12, 16) + "\n";
    receipt += "Customer Address: " + order.getAddress() + "\n";
    receipt += "Order Date: " + order.getDate() + "\n";
    receipt += "Subtotal: " + order.getTotalCost() + "\n";
    receipt += "Taxes: " + order.getTotalTax() + "\n";
    receipt += "Total Amount: " + (order.getTotalCost() + order.getTotalTax()) + "\n";
    return receipt;
}
```
## Code changes to save order details to database:
In makeOrder() function in OrderControlView.java:
```
Application.getInstance().getDataAdapter().saveOrder(order, createReceipt());
```
In DataAdapter.java:
```
public boolean saveOrder(Order order, String receipt) {
    try {
        PreparedStatement statement = connection.prepareStatement("INSERT INTO Orders VALUES (?, ?, ?, ?, ?, ?, ?)");
        PreparedStatement getMaxOrderID = connection.prepareStatement("select MAX(orderID) from Orders");
        ResultSet maxOrderID = getMaxOrderID.executeQuery();
        
        statement.setInt(1, maxOrderID.getInt(1) + 1);
        statement.setInt(2, order.getBuyerID());
        statement.setString(3, order.getCard());
        statement.setString(4, order.getAddress());
        statement.setString(5, order.getDate());
        statement.setDouble(6, order.getTotalCost());
        statement.setDouble(7, order.getTotalTax());

        statement.execute();    
        statement.close();

        statement = connection.prepareStatement("INSERT INTO OrderLine VALUES (?, ?, ?, ?)");

        for (OrderLine line: order.getLines()) { 
            statement.setInt(1, maxOrderID.getInt(1) + 1);
            statement.setInt(2, line.getProductID());
            statement.setDouble(3, line.getQuantity());
            statement.setDouble(4, line.getCost());

            statement.execute();    
          
        }
        statement.close();
        
        receipt = "Order ID: " + (maxOrderID.getInt(1) + 1) + "\n" + receipt;
        statement = connection.prepareStatement("INSERT INTO Receipt VALUES (?, ?)");
        statement.setInt(1, maxOrderID.getInt(1) + 1);
        statement.setString(2, receipt);
        statement.execute();
        statement.close();
        JOptionPane.showMessageDialog(null, receipt);
        return true;
    }
    catch (SQLException e) {
        System.out.println("Database access error!");
        e.printStackTrace();
        return false;
    }
}
```
## Data Validation

### Checking if Card Number is valid:
In the makeOrder() function in OrderControlView.java:
```
boolean isValid = true;
for (int i = 0; i < card.length(); i++) {
    if (!Character.isDigit(card.charAt(i))) {
        isValid = false;
    }
}
if (isValid && card.length() == 16) {
    order.setBuyerID(Application.getInstance().getCurrentUser().getUserID());
    order.setCard(card);
    order.setAddress(address);
    order.setDate(LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyy/MM/dd MM:mm:ss")));
    order.setTotalTax(order.getTotalCost() * 0.08);
    Application.getInstance().getDataAdapter().saveOrder(order, createReceipt());
} else {
    JOptionPane.showMessageDialog(null, "Invalid credit card number.");
}
```
### Checking if the Address field is not empty:
In the makeOrder() function in OrderControlView.java:
```
if (address.length() <= 0) {
    JOptionPane.showMessageDialog(null, "Invalid address.");
    return;
}
```
### Checking if Shopping Cart is not empty:
In the makeOrder() function in OrderControlView.java:
```
if (order.getLines().size() == 0) {
    JOptionPane.showMessageDialog(null, "Your cart is empty.");
    return;
}
```
### Checking an OrderLine if it already exists in the UI:
In the addProduct() function in OrderControlView.java:
```
for (OrderLine orderLine : order.getLines()) {
    if (orderLine.getProductID() == product.getProductID()) {
        line = orderLine; 
        line.setQuantity(line.getQuantity() + quantity);
        line.setCost(line.getQuantity() * product.getPrice());
        order.setTotalCost(order.getTotalCost() + (quantity * product.getPrice())); 
        break;
    }
}

if (line == null) {
    line = new OrderLine();
    line.setOrderID(order.getOrderID());
    line.setProductID(product.getProductID());
    line.setQuantity(quantity);
    line.setCost(quantity * product.getPrice());
    order.getLines().add(line);
    order.setTotalCost(order.getTotalCost() + line.getCost());
}


for (int i = 0; i < items.getRowCount(); i++) {
    if (items.getValueAt(i, 0).equals(product.getProductID())) {
        items.setValueAt(line.getQuantity(), i, 3);
        items.setValueAt(line.getCost(), i, 4);
        labTotal.setText("Total: $" + order.getTotalCost());
        return; 
    }
}
```

## YouTube Screen Recording
https://youtu.be/n6I0TfBFjsA
