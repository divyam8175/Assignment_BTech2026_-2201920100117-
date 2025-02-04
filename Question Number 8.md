
# Assignment_BTech2026_-2201920100117-

This is repository is for daily basis update on assignment

## Question Number 8






## Problem Statement
Bank Account System
A bank requires a system to manage different types of accounts. The bank offers Savings Account and Current Account. Both account types share attributes like accountNumber, accountHolderName, and balance. Savings accounts earn monthly interest, while current accounts allow overdraft up to a certain limit. Customers should be able to deposit and withdraw money from their accounts. For withdrawals, ensure that savings accounts do not allow overdrafts, while current accounts enforce the overdraft limit. Additionally, generate monthly statements showing all transactions for a specific account. Implement this system using a base class BankAccount and derived classes SavingsAccount and CurrentAccount. Use encapsulation to manage account attributes and methods for transaction handling.
## Approach

Class Structure and Design
BankAccount (Base Class) → Represents common properties and behaviors of all accounts.

Attributes: accountNumber, accountHolderName, balance

Methods:

deposit(amount): Adds money to the account.
withdraw(amount): Defined in derived classes for specific withdrawal rules.
displayAccount(): Displays account details.



SavingsAccount (Derived Class) → Inherits BankAccount and adds interest.

Additional attribute: interestRate
Additional method:
applyMonthlyInterest(): Adds interest to the balance.



CurrentAccount (Derived Class) → Inherits BankAccount and allows overdraft.

Additional attribute: overdraftLimit
Overridden method:
withdraw(amount): Allows overdraft but enforces the limit.




Transaction Handling

Maintain a record of deposits and withdrawals for monthly statements.
## Solution
```bash
#include <iostream>
#include <vector>
using namespace std;

// Base class: BankAccount
class BankAccount {
protected:
    int accountNumber;
    string accountHolderName;
    double balance;
    vector<string> transactionHistory;  // Store transaction details

public:
    // Constructor
    BankAccount(int accNo, string accHolder, double bal) 
        : accountNumber(accNo), accountHolderName(accHolder), balance(bal) {}

    // Deposit function
    virtual void deposit(double amount) {
        balance += amount;
        transactionHistory.push_back("Deposited: ₹" + to_string(amount));
        cout << " ₹" << amount << " deposited successfully.\n";
    }

    // Virtual withdraw function (to be implemented in derived classes)
    virtual bool withdraw(double amount) = 0;

    // Display account details
    void displayAccount() {
        cout << " Account Number: " << accountNumber
             << "\n Holder: " << accountHolderName
             << "\n Balance: ₹" << balance << "\n";
    }

    // Show monthly statement
    void showStatement() {
        cout << "\n Monthly Statement for Account: " << accountNumber << "\n";
        for (const auto &transaction : transactionHistory) {
            cout << transaction << "\n";
        }
    }
};

// Derived class: SavingsAccount
class SavingsAccount : public BankAccount {
private:
    double interestRate;

public:
    // Constructor
    SavingsAccount(int accNo, string accHolder, double bal, double rate) 
        : BankAccount(accNo, accHolder, bal), interestRate(rate) {}

    // Apply interest
    void applyMonthlyInterest() {
        double interest = balance * (interestRate / 100);
        balance += interest;
        transactionHistory.push_back("Interest Added: ₹" + to_string(interest));
        cout << " Interest of ₹" << interest << " added.\n";
    }

    // Withdraw function (Savings Account: No Overdraft Allowed)
    bool withdraw(double amount) override {
        if (amount > balance) {
            cout << " Insufficient funds. Withdrawal failed.\n";
            return false;
        }
        balance -= amount;
        transactionHistory.push_back("Withdrawn: ₹" + to_string(amount));
        cout << " ₹" << amount << " withdrawn successfully.\n";
        return true;
    }
};

// Derived class: CurrentAccount
class CurrentAccount : public BankAccount {
private:
    double overdraftLimit;

public:
    // Constructor
    CurrentAccount(int accNo, string accHolder, double bal, double overdraft) 
        : BankAccount(accNo, accHolder, bal), overdraftLimit(overdraft) {}

    // Withdraw function (Current Account: Overdraft Allowed)
    bool withdraw(double amount) override {
        if (amount > balance + overdraftLimit) {
            cout << " Overdraft limit exceeded. Withdrawal failed.\n";
            return false;
        }
        balance -= amount;
        transactionHistory.push_back("Withdrawn: ₹" + to_string(amount));
        cout << " ₹" << amount << " withdrawn successfully.\n";
        return true;
    }
};

// Main function to test the system
int main() {
    // Create accounts
    SavingsAccount savings(101, "Alice", 5000, 5.0);  // 5% Interest Rate
    CurrentAccount current(102, "Bob", 10000, 2000);  // ₹2000 Overdraft Limit

    // Display initial details
    savings.displayAccount();
    current.displayAccount();

    // Perform transactions
    savings.deposit(2000);
    savings.withdraw(3000);
    savings.applyMonthlyInterest();
    
    current.withdraw(11000);  // Allowed due to overdraft
    current.withdraw(1500);   // Overdraft limit exceeded

    // Show Statements
    savings.showStatement();
    current.showStatement();

    return 0;
}


```
## Explanation


1️-BankAccount (Base Class)
Stores common account attributes like accountNumber, accountHolderName, and balance.
Implements deposit functionality.
Defines a pure virtual method (withdraw), making BankAccount an abstract class.
Records transactions for monthly statements.

2️-SavingsAccount (Derived Class)
Does NOT allow overdrafts → Withdrawal fails if amount > balance.
Applies monthly interest to increase balance.

3️-CurrentAccount (Derived Class)
Allows overdrafts but limits withdrawals to balance + overdraftLimit.
Implements withdrawal logic to enforce overdraft rules.

4️-Main() Function
Creates one Savings Account & one Current Account.
Performs deposits, withdrawals, and interest calculations.
Displays monthly statements showing all transactions.


