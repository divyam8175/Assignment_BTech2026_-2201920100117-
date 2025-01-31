
# Assignment_BTech2026_-2201920100117-

This is repository is for daily basis update on assignment

## Question Number 4






## Problem Statement
A library needs a system to keep track of its books, members, and borrowing activities. Each book has attributes like bookID, title, author, genre, and availabilityStatus. Members have attributes such as memberID, name, and the list of books they have currently borrowed. The system should allow members to borrow books for a fixed number of days, after which they incur a fine for each overdue day. The librarian should also be able to add new books to the library or remove outdated ones. Members should have access to their borrowing history, while the library staff should be able to generate reports of overdue books, fine amounts, and books borrowed by specific members. Implement this system using OOP concepts with classes like Book, Member, and Library.
## Approach



1. Identify Key Entities (Classes)

Book: Represents a book in the library.
Member: Represents a library member.
Library: Manages books, members, and borrowing activities.

2. Define Class Attributes & Methods
Book Class

Attributes: bookID, title, author, genre, availabilityStatus
Methods: borrowBook(), returnBook(), displayBook()

Member Class
Attributes: memberID, name, borrowedBooks, borrowingHistory
Methods: borrowBook(), returnBook(), viewBorrowingHistory()

Library Class
Attributes: books, members, borrowedRecords, finePerDay
Methods: addBook(), removeBook(), borrowBook(), returnBook(), generateReports()

3. Implement Borrowing & Fine Logic

Check if Book is Available
Assign Book to Member
Track Borrowing Date
Calculate Fine for Late Returns
Generate Reports for Overdue Books & Borrowing History

## Solution
```bash
#include <iostream>
#include <vector>
#include <map>
#include <ctime>
using namespace std;

class Book {
public:
    int bookID;
    string title, author, genre;
    bool isAvailable;

    Book(int id, string t, string a, string g) : bookID(id), title(t), author(a), genre(g), isAvailable(true) {}

    void displayBook() {
        cout << bookID << "  " << title << "  by " << author << " (" << genre << ") - " << (isAvailable ? "Available" : "Borrowed") << endl;
    }
};

class Member {
public:
    int memberID;
    string name;
    map<int, time_t> borrowedBooks;
    vector<int> borrowingHistory;

    Member(int id, string n) : memberID(id), name(n) {}

    void borrowBook(int bookID) {
        time_t now = time(0);
        borrowedBooks[bookID] = now;
        borrowingHistory.push_back(bookID);
    }

    void returnBook(int bookID, double finePerDay) {
        time_t now = time(0);
        double daysLate = difftime(now, borrowedBooks[bookID]) / (60 * 60 * 24) - 7;
        if (daysLate > 0) {
            cout << "Late return! Fine: $" << daysLate * finePerDay << endl;
        }
        borrowedBooks.erase(bookID);
    }

    void viewBorrowingHistory() {
        cout << "Borrowing History for " << name << ":\n";
        for (int bookID : borrowingHistory) {
            cout << "Book ID: " << bookID << endl;
        }
    }
};

class Library {
public:
    map<int, Book> books;
    map<int, Member> members;
    double finePerDay = 1.0;

    void addBook(Book book) { books[book.bookID] = book; }

    void removeBook(int bookID) { books.erase(bookID); }

    void registerMember(Member member) { members[member.memberID] = member; }

    void borrowBook(int memberID, int bookID) {
        if (books[bookID].isAvailable) {
            books[bookID].isAvailable = false;
            members[memberID].borrowBook(bookID);
            cout << members[memberID].name << " borrowed " << books[bookID].title << endl;
        } else {
            cout << "Book not available.\n";
        }
    }

    void returnBook(int memberID, int bookID) {
        books[bookID].isAvailable = true;
        members[memberID].returnBook(bookID, finePerDay);
        cout << members[memberID].name << " returned " << books[bookID].title << endl;
    }

    void generateOverdueReport() {
        cout << "Overdue Books Report:\n";
        for (auto &member : members) {
            for (auto &entry : member.second.borrowedBooks) {
                double daysLate = difftime(time(0), entry.second) / (60 * 60 * 24) - 7;
                if (daysLate > 0) {
                    cout << member.second.name << " has overdue book ID " << entry.first << " (Fine: $" << daysLate * finePerDay << ")\n";
                }
            }
        }
    }
};

int main() {
    Library lib;
    lib.addBook(Book(101, "1984", "George Orwell", "Dystopian"));
    lib.addBook(Book(102, "To Kill a Mockingbird", "Harper Lee", "Classic"));

    lib.registerMember(Member(1, "Alice"));
    lib.registerMember(Member(2, "Bob"));

    lib.borrowBook(1, 101);
    lib.borrowBook(2, 102);

    lib.generateOverdueReport();

    return 0;
}
```
## Explanation

Book Class: Represents books with availability status.
Member Class: Handles borrowing & returning books.

Library Class:
Manages books and members.
Implements borrowing, returning, and fine calculation.
Generates overdue reports.


