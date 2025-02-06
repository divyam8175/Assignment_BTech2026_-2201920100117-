
# Assignment_BTech2026_-2201920100117-

This is repository is for daily basis update on assignment

## Question Number 10






## Problem Statement
Flight Reservation System
An airline needs a system to handle flight bookings. Each flight has attributes like flightNumber, source, destination, departureTime, arrivalTime, and seatAvailability. Customers can search for flights by source and destination and book tickets for available seats. The system calculates the ticket price dynamically based on the travel class (e.g., economy, business). Customers can also cancel their bookings, and the system updates the seat availability. The airline requires reports showing the total earnings for a specific flight and the number of passengers. Implement this system using classes such as Flight and Customer.
## Approach

Flight Class:

Stores flight details (flight number, source, destination, departure time, arrival time, seats, prices).

Manages seat availability.
Calculates ticket price dynamically based on class (Economy/Business).

Customer Class:

Stores customer details.
Books a flight.
Cancels a booking.

Reservation System Class:

Manages a collection of flights.
Allows customers to search for flights by source/destination.
Generates reports (total earnings, number of passengers).
## Solution
```bash
#include <iostream>
#include <vector>
#include <unordered_map>
using namespace std;

// Class to represent a Flight
class Flight {
public:
    string flightNumber, source, destination, departureTime, arrivalTime;
    int totalSeats, availableSeats;
    double basePrice;
    double earnings;
    int passengerCount;

    // Constructor
    Flight(string fNum, string src, string dest, string dTime, string aTime, int seats, double price)
        : flightNumber(fNum), source(src), destination(dest), departureTime(dTime),
          arrivalTime(aTime), totalSeats(seats), availableSeats(seats), basePrice(price), 
          earnings(0), passengerCount(0) {}

    // Function to display flight details
    void displayFlightDetails() {
        cout << "Flight: " << flightNumber << " | " << source << " -> " << destination
             << " | Departure: " << departureTime << " | Arrival: " << arrivalTime
             << " | Seats Available: " << availableSeats << endl;
    }

    // Function to calculate ticket price dynamically
    double calculatePrice(string travelClass) {
        if (travelClass == "Business") return basePrice * 1.5;
        return basePrice; // Economy class
    }

    // Function to book a seat
    bool bookSeat(string travelClass) {
        if (availableSeats > 0) {
            double price = calculatePrice(travelClass);
            availableSeats--;
            earnings += price;
            passengerCount++;
            cout << "Booking Confirmed! Price: $" << price << endl;
            return true;
        }
        cout << "Sorry, no seats available on this flight.\n";
        return false;
    }

    // Function to cancel a booking
    bool cancelSeat(string travelClass) {
        if (passengerCount > 0) {
            double price = calculatePrice(travelClass);
            availableSeats++;
            earnings -= price;
            passengerCount--;
            cout << "Booking Canceled. Refund: $" << price << endl;
            return true;
        }
        cout << "No bookings to cancel.\n";
        return false;
    }
};

// Class to represent a Customer
class Customer {
public:
    string name;
    unordered_map<string, string> bookings; // Flight Number -> Class (Economy/Business)

    // Constructor
    Customer(string n) : name(n) {}

    // Function to book a flight
    void bookFlight(Flight &flight, string travelClass) {
        if (flight.bookSeat(travelClass)) {
            bookings[flight.flightNumber] = travelClass;
        }
    }

    // Function to cancel a booking
    void cancelFlight(Flight &flight) {
        if (bookings.find(flight.flightNumber) != bookings.end()) {
            flight.cancelSeat(bookings[flight.flightNumber]);
            bookings.erase(flight.flightNumber);
        } else {
            cout << "No booking found for this flight.\n";
        }
    }
};

// Class to manage the reservation system
class FlightReservationSystem {
private:
    vector<Flight> flights;

public:
    // Function to add flights
    void addFlight(Flight flight) {
        flights.push_back(flight);
    }

    // Function to search flights by source and destination
    void searchFlights(string src, string dest) {
        bool found = false;
        for (auto &flight : flights) {
            if (flight.source == src && flight.destination == dest) {
                flight.displayFlightDetails();
                found = true;
            }
        }
        if (!found) cout << "No flights found from " << src << " to " << dest << ".\n";
    }

    // Function to generate report of earnings and passengers for a flight
    void generateReport(string flightNumber) {
        for (auto &flight : flights) {
            if (flight.flightNumber == flightNumber) {
                cout << "Flight: " << flight.flightNumber << " | Total Earnings: $" 
                     << flight.earnings << " | Total Passengers: " << flight.passengerCount << endl;
                return;
            }
        }
        cout << "Flight not found.\n";
    }
};

// Main function to demonstrate functionality
int main() {
    FlightReservationSystem system;
    
    // Adding flights
    system.addFlight(Flight("AI101", "New York", "London", "10:00 AM", "8:00 PM", 100, 500));
    system.addFlight(Flight("BA202", "Los Angeles", "Tokyo", "12:00 PM", "6:00 AM", 150, 700));
    
    Customer customer1("John Doe");
    Customer customer2("Alice Smith");

    // Searching for flights
    cout << "\nSearching Flights from New York to London:\n";
    system.searchFlights("New York", "London");

    // Booking flights
    cout << "\nBooking Flights:\n";
    customer1.bookFlight(system.flights[0], "Economy"); // John books AI101
    customer2.bookFlight(system.flights[0], "Business"); // Alice books AI101

    // Cancelling a flight
    cout << "\nCanceling Flight Booking:\n";
    customer1.cancelFlight(system.flights[0]); // John cancels AI101

    // Generating report for a flight
    cout << "\nGenerating Flight Report:\n";
    system.generateReport("AI101");

    return 0;
}



```
## Explanation


Flight Class

Stores flight details.
Handles booking, cancellation, and dynamic price calculation.
Keeps track of available seats, earnings, and passengers.

Customer Class

Allows booking and canceling flights.
Maintains a record of booked flights.

FlightReservationSystem Class

Manages flights.
Provides search functionality.
Generates flight reports (earnings and passengers count).

Main Function

Demonstrates system functionalities:
Adding flights
Searching for flights
Booking and canceling
Generating reports