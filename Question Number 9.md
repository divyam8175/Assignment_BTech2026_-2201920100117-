
# Assignment_BTech2026_-2201920100117-

This is repository is for daily basis update on assignment

## Question Number 9






## Problem Statement
Restaurant Billing System
A restaurant requires a billing system to manage orders. Each menu item has attributes like itemID, name, category, and price. Customers can place orders by selecting multiple items from the menu. The system calculates the total bill, including taxes and service charges. If the customer applies a discount code, the system should deduct the appropriate amount from the total. The restaurant also needs a feature to generate daily sales reports, showing the total revenue and the most ordered items. Implement this system using classes like MenuItem, Order, and Customer.
## Approach

Class MenuItem: Represents each menu item with attributes like itemID, name, category, and price.

Class Order: Manages ordered items, calculates the total bill (including tax and service charges), and applies discount codes.

Class Customer: Stores customer details and manages their orders.

Class Restaurant: Handles menu operations, order processing, and generates daily sales reports.
## Solution
```bash
#include <iostream>
#include <vector>
#include <map>

using namespace std;

class MenuItem {
public:
    int itemID;
    string name, category;
    double price;

    MenuItem(int id, string n, string cat, double p) : itemID(id), name(n), category(cat), price(p) {}

    void display() {
        cout << itemID << ". " << name << " (" << category << ") - $" << price << endl;
    }
};

class Order {
public:
    vector<MenuItem> items;
    double taxRate = 0.08;  // 8% tax
    double serviceCharge = 0.05; // 5% service charge
    double discount = 0.0;

    void addItem(MenuItem item) {
        items.push_back(item);
    }

    void applyDiscount(string code) {
        if (code == "DISCOUNT10") discount = 0.1; // 10% discount
        else if (code == "DISCOUNT20") discount = 0.2; // 20% discount
        else discount = 0.0;
    }

    double calculateTotal() {
        double subtotal = 0;
        for (auto &item : items) subtotal += item.price;
        double tax = subtotal * taxRate;
        double service = subtotal * serviceCharge;
        double discountAmount = subtotal * discount;
        return (subtotal + tax + service - discountAmount);
    }

    void displayBill() {
        cout << "\n------ Bill Summary ------\n";
        for (auto &item : items) item.display();
        cout << "Total (after taxes & service charge): $" << calculateTotal() << endl;
    }
};

class Restaurant {
public:
    vector<MenuItem> menu;
    map<string, int> salesReport;
    double totalRevenue = 0;

    void addMenuItem(MenuItem item) {
        menu.push_back(item);
    }

    void displayMenu() {
        cout << "---- Menu ----\n";
        for (auto &item : menu) item.display();
    }

    void processOrder(Order &order) {
        totalRevenue += order.calculateTotal();
        for (auto &item : order.items) salesReport[item.name]++;
    }

    void generateSalesReport() {
        cout << "\n---- Daily Sales Report ----\n";
        cout << "Total Revenue: $" << totalRevenue << endl;
        cout << "Most Ordered Items:\n";
        for (auto &entry : salesReport)
            cout << entry.first << " - " << entry.second << " orders\n";
    }
};

int main() {
    Restaurant restaurant;
    restaurant.addMenuItem(MenuItem(1, "Burger", "Fast Food", 5.99));
    restaurant.addMenuItem(MenuItem(2, "Pasta", "Italian", 7.99));
    restaurant.addMenuItem(MenuItem(3, "Pizza", "Italian", 8.99));
    
    restaurant.displayMenu();

    Order order;
    order.addItem(MenuItem(1, "Burger", "Fast Food", 5.99));
    order.addItem(MenuItem(2, "Pasta", "Italian", 7.99));

    string discountCode;
    cout << "Enter Discount Code (if any): ";
    cin >> discountCode;
    order.applyDiscount(discountCode);
    
    order.displayBill();
    
    restaurant.processOrder(order);
    restaurant.generateSalesReport();
    
    return 0;
}


```
## Explanation


MenuItem Class:

Stores item details (itemID, name, category, price).
Has a display() method to show item details.


Order Class:

Stores a list of MenuItem objects in items.
Can apply discounts using applyDiscount().
Computes the final bill in calculateTotal() (including tax & service charges).
Displays a formatted bill in displayBill().


Restaurant Class:

Manages a list of MenuItem objects (menu).
Records daily sales in salesReport.
Processes orders and updates revenue.
Generates a sales report to track total earnings & most ordered items.


main() Function:

Initializes the restaurant menu.
Takes an order from the customer.
Applies discount (if any).
Displays the bill.
Generates a sales report.


