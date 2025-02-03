
# Assignment_BTech2026_-2201920100117-

This is repository is for daily basis update on assignment

## Question Number 7






## Problem Statement
Movie Ticket Booking System
A multiplex requires a ticket booking system to manage its daily shows. Each movie has attributes such as movieID, title, duration, showTime, and seatAvailability. Customers can search for movies by their title or showtime and book tickets for available seats. Once a ticket is booked, the seat availability must decrease accordingly. Customers should also have the option to cancel their tickets, which updates the seat availability. The system should calculate the total earnings for a specific movie based on ticket sales. Additionally, the multiplex should generate a report showing the most popular movies and their total revenue. Implement this system using classes like Movie and Customer, along with appropriate methods for ticket booking and revenue calculation.
## Approach

1-Movie Class

Stores details like movieID, title, duration, showTime, seatAvailability, and ticketPrice.

Methods:

bookTicket(int numTickets): Reduces seat availability and increases revenue.

cancelTicket(int numTickets): Increases seat availability and decreases revenue.

calculateRevenue(): Returns total revenue.

2-Customer Class

Stores customer details and interacts with the Movie class to book or cancel tickets.

3-Multiplex Class

Stores a list of movies and provides:

addMovie(): Adds movies to the multiplex.

searchMovie(): Finds a movie by title or showTime.

generateReport(): Displays the most popular movies and their earnings.
## Solution
```bash
#include <iostream>
#include <vector>
#include <string>

using namespace std;


class Movie {
private:
    int movieID;
    string title;
    string duration;
    string showTime;
    int seatAvailability;
    double ticketPrice;
    double totalEarnings;

public:

    Movie(int id, string t, string d, string sTime, int seats, double price) 
        : movieID(id), title(t), duration(d), showTime(sTime), 
          seatAvailability(seats), ticketPrice(price), totalEarnings(0) {}

    void bookTicket(int numTickets) {
        if (numTickets <= seatAvailability) {
            seatAvailability -= numTickets;
            totalEarnings += numTickets * ticketPrice;
            cout << "" << numTickets << " ticket(s) booked for '" << title << "' at " << showTime << ".\n";
        } else {
            cout << " Not enough seats available for '" << title << "'. Available: " << seatAvailability << "\n";
        }
    }

    void cancelTicket(int numTickets) {
        seatAvailability += numTickets;
        totalEarnings -= numTickets * ticketPrice;
        cout << "" << numTickets << " ticket(s) canceled for '" << title << "' at " << showTime << ".\n";
    }

    string getTitle() { return title; }

    string getShowTime() { return showTime; }

    double calculateRevenue() { return totalEarnings; }

    void displayMovie() {
        cout << "movie " << title << " | Duration: " << duration << " | ShowTime: " << showTime 
             << " | Seats Available: " << seatAvailability << " | Price: ₹" << ticketPrice << "\n";
    }
};

class Customer {
private:
    int customerID;
    string name;

public:
    Customer(int id, string n) : customerID(id), name(n) {}

    void bookTicket(Movie &movie, int numTickets) {
        movie.bookTicket(numTickets);
    }

    void cancelTicket(Movie &movie, int numTickets) {
        movie.cancelTicket(numTickets);
    }
};

class Multiplex {
private:
    vector<Movie> movies;

public:
    // Add Movie to Multiplex
    void addMovie(Movie movie) {
        movies.push_back(movie);
    }

    void searchMovie(string title = "", string showTime = "") {
        bool found = false;
        for (auto &movie : movies) {
            if (movie.getTitle() == title || movie.getShowTime() == showTime) {
                movie.displayMovie();
                found = true;
            }
        }
        if (!found) {
            cout << " Movie not found.\n";
        }
    }

    void generateReport() {
        cout << "\n Movie Revenue Report:\n";
        for (auto &movie : movies) {
            cout << movie.getTitle() << " - Total Earnings: ₹" << movie.calculateRevenue() << "\n";
        }
    }
};

int main() {

    Movie movie1(1, "Inception", "2h 28m", "7:00 PM", 100, 250);
    Movie movie2(2, "Avatar", "2h 42m", "9:00 PM", 120, 300);
    Multiplex multiplex;
    multiplex.addMovie(movie1);
    multiplex.addMovie(movie2);
    Customer customer1(101, "Alice");
    customer1.bookTicket(movie1, 2);  

    Customer customer2(102, "Bob");
    customer2.bookTicket(movie2, 3);  
    customer2.cancelTicket(movie2, 1);  

    cout << "\n Searching for 'Inception':\n";
    multiplex.searchMovie("Inception");

    multiplex.generateReport();

    return 0;
}

```
## Explanation


Movie Ticket Booking System – Theoretical Explanation
A Movie Ticket Booking System is designed to manage the booking and cancellation of tickets for movies playing in a multiplex. The system allows customers to search for movies, book tickets, and view showtimes. Additionally, the multiplex can track revenue generation and display popular movies based on ticket sales. The system follows an Object-Oriented Programming (OOP) approach, making it modular, reusable, and efficient.


