# Assignment_BTech2026_-2201920100117-
This is repository is for daily basis update on assignment


OOPs Question No:-1

 Problem Statement:
 A multi-specialty hospital requires a management system to track doctors, patients, and appointments. Each doctor has attributes such as doctorID, name, specialization, and their schedule for each day of the week. Patients have attributes like patientID, name, age, disease, and appointmentDate. The system should allow patients to book appointments with a doctor based on their specialization and availability. Once an appointment is booked, it must be reflected in the doctor's daily schedule. Doctors can also cancel appointments, which will make the slot available for others. Additionally, the system should keep a record of the patient's medical history, including previous diagnoses and prescriptions. The hospital should also generate a report of the appointments scheduled for a specific doctor or patient. Implement the system using OOP principles and create classes for Doctor, Patient, and Appointment.

****Approach****

1. Identify Classes
Key entities in the problem are:

Doctor

Attributes: doctorID, name, specialization, schedule (a daily list of appointments).
Behaviors:
Add appointments.
Cancel appointments.
View appointments.
Each doctor will have a schedule for the week, which updates dynamically.

Patient

Attributes: patientID, name, age, disease, appointmentDate, and medicalHistory (list of past diagnoses/prescriptions).
Behaviors:
Book an appointment.
View medical history.

Appointment

Attributes: appointmentID, doctor, patient, appointmentDate, time.
Behaviors:
Create appointment.
Cancel appointment.

2. Relationships Between Classes
Doctor and Appointment have a one-to-many relationship (a doctor can have multiple appointments in a day).
Patient and Appointment have a one-to-many relationship (a patient can have multiple appointments over time).

3. Core Functionalities
   
Book Appointment:

A patient selects a doctor based on specialization and availability.
The system checks the doctor’s schedule and assigns the earliest available slot.
Appointment details are added to both the doctor’s schedule and the patient’s records.

Cancel Appointment:

The doctor or patient cancels the appointment, freeing up the slot.
The system updates both the doctor’s schedule and the patient’s records.

Medical History:

A patient’s previous diagnoses and prescriptions are stored and retrievable.
Reports:

Generate all appointments for a specific doctor or patient.


Solution:-

#include <iostream>
#include <vector>
#include <map>
#include <string>
using namespace std;

class Appointment;

class Doctor {
public:
    int doctorID;
    string name;
    string specialization;
    map<string, vector<string>> schedule;
    Doctor(int id, string n, string spec) : doctorID(id), name(n), specialization(spec) {
        vector<string> slots = {"9:00 AM", "10:00 AM", "11:00 AM", "12:00 PM", "2:00 PM", "3:00 PM"};
        schedule = {{"Monday", slots}, {"Tuesday", slots}, {"Wednesday", slots},
                    {"Thursday", slots}, {"Friday", slots}};
    }
    void addAppointment(const string &day, const string &slot) {
        auto &slots = schedule[day];
        slots.erase(remove(slots.begin(), slots.end(), slot), slots.end());
    }
    void cancelAppointment(const string &day, const string &slot) {
        schedule[day].push_back(slot);
    }
    void viewSchedule(const string &day) {
        cout << "Schedule for " << name << " on " << day << ":\n";
        for (const auto &slot : schedule[day]) {
            cout << slot << " ";
        }
        cout << endl;
    }
};
class Patient {
public:
    int patientID;
    string name;
    int age;
    string disease;
    vector<string> medicalHistory;
    Patient(int id, string n, int a, string d) : patientID(id), name(n), age(a), disease(d) {}
    void addMedicalHistory(const string &entry) {
        medicalHistory.push_back(entry);
    }
    void viewMedicalHistory() {
        cout << "Medical History for " << name << ":\n";
        for (const auto &entry : medicalHistory) {
            cout << "- " << entry << endl;
        }
    }
};
class Appointment {
public:
    int appointmentID;
    Doctor *doctor;
    Patient *patient;
    string day;
    string time;
    Appointment(int id, Doctor *doc, Patient *pat, string d, string t)
        : appointmentID(id), doctor(doc), patient(pat), day(d), time(t) {}
    void displayAppointment() {
        cout << "Appointment ID: " << appointmentID << endl;
        cout << "Doctor: " << doctor->name << " (" << doctor->specialization << ")" << endl;
        cout << "Patient: " << patient->name << endl;
        cout << "Day: " << day << ", Time: " << time << endl;
    }
};

int main() {
    Doctor doc1(1, "Dr. Smith", "Cardiology");
    Doctor doc2(2, "Dr. Alice", "Dermatology");
    Patient pat1(1, "John Doe", 30, "Heart Disease");
    Patient pat2(2, "Jane Roe", 25, "Skin Allergy");
    Appointment app1(101, &doc1, &pat1, "Monday", "9:00 AM");
    doc1.addAppointment("Monday", "9:00 AM");
    pat1.addMedicalHistory("Consulted Dr. Smith for Heart Disease.");
    Appointment app2(102, &doc2, &pat2, "Tuesday", "10:00 AM");
    doc2.addAppointment("Tuesday", "10:00 AM");
    pat2.addMedicalHistory("Consulted Dr. Alice for Skin Allergy.");
    cout << "Appointment Details:\n";
    app1.displayAppointment();
    cout << endl;
    app2.displayAppointment();
    cout << "\nDoctor's Schedule:\n";
    doc1.viewSchedule("Monday");
    doc2.viewSchedule("Tuesday");
    cout << "\nPatient's Medical History:\n";
    pat1.viewMedicalHistory();
    pat2.viewMedicalHistory();
    cout << "\nCancelling an appointment...\n";
    doc1.cancelAppointment("Monday", "9:00 AM");
    cout << "\nUpdated Doctor's Schedule:\n";
    doc1.viewSchedule("Monday");
    return 0;
}


Explanation

Explanation
Classes and Attributes:

Doctor tracks doctor-specific data and their weekly schedule.
Patient manages patient-specific data, including medical history.
Appointment links a patient and doctor for a specific day and time.
Dynamic Booking:

Appointment booking removes a slot from the doctor’s schedule.
A patient’s medical history is updated after every consultation.
Cancellation:

Cancelling an appointment restores the slot to the doctor’s schedule.
Reports:

The program can display all appointments for a doctor or patient dynamically.
