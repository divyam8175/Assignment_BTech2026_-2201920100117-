
# Assignment_BTech2026_-2201920100117-

This is repository is for daily basis update on assignment

## Question Number 3






## Problem Statement
Design an e-commerce platform for a company where customers can browse products, add them to their shopping cart, and place orders. Each product has attributes like productID, name, description, price, and stockQuantity. Customers should be able to view products based on their categories (e.g., electronics, clothing, books) and check their availability. Once a customer places an order, the product’s stock should be updated accordingly. If a customer tries to order more than the available stock, the system should display an error. Implement classes for Product, Customer, Cart, and Order. Add methods for adding and removing products from the cart, placing orders, and generating invoices. Additionally, apply discount codes and calculate shipping charges dynamically based on the total order value
## Approach



1. Identify Key Entities (Classes)

Product: Represents an item in the store.
Customer: Stores customer details.
Cart: Allows customers to add/remove products.
Order: Handles order placement, stock updates, and invoice generation.

2. Define Class Attributes & Methods
Product Class

Attributes: productID, name, description, category, price, stockQuantity
Methods: displayProduct(), isAvailable(), reduceStock()

Customer Class
Attributes: customerID, name, email
No major methods needed.

Cart Class
Attributes: items (stores productID → quantity)
Methods: addProduct(), removeProduct(), displayCart()

Order Class
Attributes: orderID, customer, orderedItems, totalAmount, discount, shippingCost
Methods: applyDiscount(), calculateShipping(), generateInvoice()

3. Implement Order Processing Logic
Display Products
Customer Adds Items to Cart
Validate Stock Before Checkout
Apply Discounts & Calculate Shipping
Generate Invoice & Reduce Stock
## Solution
```bash
#include <iostream>
#include <vector>
#include <map>
using namespace std;

class Product {
public:
    int productID;
    string name, description, category;
    double price;
    int stockQuantity;

    Product(int id, string n, string d, string c, double p, int stock)
        : productID(id), name(n), description(d), category(c), price(p), stockQuantity(stock) {}

    void displayProduct() {
        cout << productID << "  " << name << "  $" << price << "  Stock: " << stockQuantity << endl;
    }

    bool isAvailable(int qty) { return stockQuantity >= qty; }

    void reduceStock(int qty) { stockQuantity -= qty; }
};

class Customer {
public:
    int customerID;
    string name, email;

    Customer(int id, string n, string e) : customerID(id), name(n), email(e) {}
};

class Cart {
public:
    map<int, int> items;

    void addProduct(int productID, int quantity) { items[productID] += quantity; }

    void removeProduct(int productID) { items.erase(productID); }

    void displayCart(map<int, Product> &products) {
        cout << "Cart Items:\n";
        for (auto &item : items) {
            cout << products[item.first].name << " x" << item.second << " $" << products[item.first].price * item.second << endl;
        }
    }
};

class Order {
public:
    int orderID;
    Customer *customer;
    map<int, int> orderedItems;
    double totalAmount = 0, discount = 0, shippingCost = 0;

    Order(int id, Customer *cust, Cart &cart, map<int, Product> &products) : orderID(id), customer(cust) {
        orderedItems = cart.items;
        for (auto &item : orderedItems) {
            if (products[item.first].isAvailable(item.second)) {
                totalAmount += products[item.first].price * item.second;
                products[item.first].reduceStock(item.second);
            } else {
                cout << "Insufficient stock for " << products[item.first].name << endl;
                orderedItems.erase(item.first);
            }
        }
    }

    void applyDiscount(string code) {
        if (code == "SAVE10") discount = totalAmount * 0.10;
        if (code == "SAVE20") discount = totalAmount * 0.20;
    }

    void calculateShipping() { shippingCost = (totalAmount - discount > 100) ? 0 : 5; }

    void generateInvoice() {
        cout << "Invoice - Order #" << orderID << "\nCustomer: " << customer->name << endl;
        cout << "Total: $" << totalAmount << "  Discount: $" << discount << "  Shipping: $" << shippingCost << endl;
        cout << "Final Amount: $" << totalAmount - discount + shippingCost << endl;
    }
};

int main() {
    map<int, Product> products = {
        {101, Product(101, "Laptop", "Gaming Laptop", "Electronics", 800, 10)},
        {102, Product(102, "T-Shirt", "Cotton T-shirt", "Clothing", 20, 50)}
    };

    Customer cust1(1, "Alice", "alice@example.com");
    Cart cart;
    cart.addProduct(101, 1);
    cart.addProduct(102, 2);
    cart.displayCart(products);

    Order order1(1, &cust1, cart, products);
    order1.applyDiscount("SAVE10");
    order1.calculateShipping();
    order1.generateInvoice();

    return 0;
}

```
## Explanation

Product Class: Defines items with price, stock, and availability check.
Customer Class: Stores customer details.
Cart Class: Manages adding/removing items before purchase.
Order Class: Processes the cart, applies discounts, and generates an invoice.
Main Function:
Creates sample products.
Adds items to a customer's cart.
Places an order and generates an invoice.

