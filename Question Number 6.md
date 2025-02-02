
# Assignment_BTech2026_-2201920100117-

This is repository is for daily basis update on assignment

## Question Number 6






## Problem Statement
University Management System
A university management system is required to handle students, professors, and courses. Each student has attributes such as studentID, name, year, and a list of enrolled courses. Professors have attributes like professorID, name, and the courses they teach. Courses have attributes such as courseID, title, credits, and a list of enrolled students. The system should allow students to enroll in courses and professors to be assigned to specific courses. Each course should have a maximum enrollment limit, and the system should not allow more students than this limit. The university also requires a feature to generate a report showing the schedule of a specific professor or student. Design this system using classes such as Student, Professor, and Course, applying the principles of inheritance and encapsulation.
## Approach

Key Classes and Their Responsibilities
Person (Base Class)

Attributes: id, name
Common functionality for both students and professors.
Student (Derived from Person)

Attributes: studentID, year, enrolledCourses
Methods: enroll_course(), view_schedule()
Professor (Derived from Person)

Attributes: professorID, assignedCourses
Methods: assign_course(), view_schedule()
Course

Attributes: courseID, title, credits, max_students, enrolled_students
Methods: add_student(), remove_student()
## Solution
```bash
#include <iostream>
#include <vector>
using namespace std;

// Base Class: Person
class Person {
protected:
    int id;
    string name;

public:
    Person(int id, string name) : id(id), name(name) {}
    virtual void viewSchedule() = 0; // Pure virtual function
};

// Forward declaration of Course class
class Course;

// Derived Class: Student
class Student : public Person {
private:
    int year;
    vector<Course*> enrolledCourses;

public:
    Student(int studentID, string name, int year) : Person(studentID, name), year(year) {}

    void enrollCourse(Course* course);
    
    void viewSchedule() override {
        cout << "\nSchedule for Student " << name << ":\n";
        for (auto course : enrolledCourses) {
            cout << "- " << course->getTitle() << endl;
        }
    }
};

// Derived Class: Professor
class Professor : public Person {
private:
    vector<Course*> assignedCourses;

public:
    Professor(int professorID, string name) : Person(professorID, name) {}

    void assignCourse(Course* course) {
        assignedCourses.push_back(course);
        cout << name << " assigned to teach " << course->getTitle() << ".\n";
    }

    void viewSchedule() override {
        cout << "\nSchedule for Professor " << name << ":\n";
        for (auto course : assignedCourses) {
            cout << "- " << course->getTitle() << endl;
        }
    }
};

// Class: Course
class Course {
private:
    int courseID;
    string title;
    int credits;
    int max_students;
    vector<Student*> enrolled_students;

public:
    Course(int courseID, string title, int credits, int max_students) 
        : courseID(courseID), title(title), credits(credits), max_students(max_students) {}

    string getTitle() { return title; }

    bool addStudent(Student* student) {
        if (enrolled_students.size() < max_students) {
            enrolled_students.push_back(student);
            return true;
        }
        return false;
    }
};

// Student method to enroll in a course
void Student::enrollCourse(Course* course) {
    if (course->addStudent(this)) {
        enrolledCourses.push_back(course);
        cout << name << " enrolled in " << course->getTitle() << ".\n";
    } else {
        cout << "Enrollment failed! " << course->getTitle() << " is full.\n";
    }
}

// Main Function
int main() {
    // Creating Objects
    Student s1(101, "Alice", 2);
    Student s2(102, "Bob", 3);
    Professor p1(201, "Dr. Smith");
    Course c1(301, "Data Structures", 3, 2);

    // Assigning Professor
    p1.assignCourse(&c1);

    // Students enrolling in the course
    s1.enrollCourse(&c1);
    s2.enrollCourse(&c1);

    // Viewing schedules
    s1.viewSchedule();
    p1.viewSchedule();

    // Trying to enroll a third student beyond limit
    Student s3(103, "Charlie", 2);
    s3.enrollCourse(&c1); // Should print an error message

    return 0;
}

```
## Explanation

1.Encapsulation:

Each class has its own attributes and methods to encapsulate behavior.
Attributes like enrolled_students in Course are hidden from direct modification.

2.Inheritance:

Student and Professor inherit from Person to avoid redundant code.

3.Polymorphism:

view_schedule() method works differently for students and professors.

4.Constraints Handling:

The enroll_course() method ensures that a course does not exceed max_students.


