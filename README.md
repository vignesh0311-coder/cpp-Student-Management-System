# cpp-Student-Management-System
Student Management System
#include <iostream>
#include <fstream>
#include <string>
#include <sstream>
using namespace std;

class Student {
public:
    int id;
    string name;
    int age;
    string course;

    void input() {
        cout << "\nEnter Student ID: ";
        cin >> id;
        cin.ignore();

        cout << "Enter Name: ";
        getline(cin, name);

        cout << "Enter Age: ";
        cin >> age;
        cin.ignore();

        cout << "Enter Course: ";
        getline(cin, course);
    }

    void display() {
        cout << "-----------------------------------\n";
        cout << "ID     : " << id << endl;
        cout << "Name   : " << name << endl;
        cout << "Age    : " << age << endl;
        cout << "Course : " << course << endl;
    }
};

// Add Student
void addStudent() {
    Student s;
    s.input();

    ofstream file("students.txt", ios::app);
    file << s.id << "," << s.name << "," << s.age << "," << s.course << endl;
    file.close();

    cout << "\nStudent Added Successfully!\n";
}

// Display Students
void displayStudents() {
    ifstream file("students.txt");
    string line;

    cout << "\n===== Student Records =====\n";

    while (getline(file, line)) {
        stringstream ss(line);
        Student s;
        string temp;

        getline(ss, temp, ',');
        s.id = stoi(temp);

        getline(ss, s.name, ',');

        getline(ss, temp, ',');
        s.age = stoi(temp);

        getline(ss, s.course);

        s.display();
    }

    file.close();
}

// Search Student
void searchStudent() {
    int searchID;
    cout << "Enter Student ID to Search: ";
    cin >> searchID;

    ifstream file("students.txt");
    string line;
    bool found = false;

    while (getline(file, line)) {
        stringstream ss(line);
        Student s;
        string temp;

        getline(ss, temp, ',');
        s.id = stoi(temp);

        getline(ss, s.name, ',');

        getline(ss, temp, ',');
        s.age = stoi(temp);

        getline(ss, s.course);

        if (s.id == searchID) {
            s.display();
            found = true;
            break;
        }
    }

    file.close();

    if (!found)
        cout << "Student Not Found!\n";
}

// Update Student
void updateStudent() {
    int updateID;
    cout << "Enter Student ID to Update: ";
    cin >> updateID;
    cin.ignore();

    ifstream file("students.txt");
    ofstream tempFile("temp.txt");

    string line;
    bool found = false;

    while (getline(file, line)) {
        stringstream ss(line);
        Student s;
        string temp;

        getline(ss, temp, ',');
        s.id = stoi(temp);

        getline(ss, s.name, ',');

        getline(ss, temp, ',');
        s.age = stoi(temp);

        getline(ss, s.course);

        if (s.id == updateID) {
            found = true;

            cout << "\nEnter New Details\n";

            cout << "Name: ";
            getline(cin, s.name);

            cout << "Age: ";
            cin >> s.age;
            cin.ignore();

            cout << "Course: ";
            getline(cin, s.course);
        }

        tempFile << s.id << "," << s.name << "," << s.age << "," << s.course << endl;
    }

    file.close();
    tempFile.close();

    remove("students.txt");
    rename("temp.txt", "students.txt");

    if (found)
        cout << "Student Updated Successfully!\n";
    else
        cout << "Student Not Found!\n";
}

// Delete Student
void deleteStudent() {
    int deleteID;
    cout << "Enter Student ID to Delete: ";
    cin >> deleteID;

    ifstream file("students.txt");
    ofstream tempFile("temp.txt");

    string line;
    bool found = false;

    while (getline(file, line)) {
        stringstream ss(line);
        Student s;
        string temp;

        getline(ss, temp, ',');
        s.id = stoi(temp);

        getline(ss, s.name, ',');

        getline(ss, temp, ',');
        s.age = stoi(temp);

        getline(ss, s.course);

        if (s.id == deleteID) {
            found = true;
            continue;
        }

        tempFile << s.id << "," << s.name << "," << s.age << "," << s.course << endl;
    }

    file.close();
    tempFile.close();

    remove("students.txt");
    rename("temp.txt", "students.txt");

    if (found)
        cout << "Student Deleted Successfully!\n";
    else
        cout << "Student Not Found!\n";
}

// Main Function
int main() {
    int choice;

    do {
        cout << "\n========== Student Management System ==========\n";
        cout << "1. Add Student\n";
        cout << "2. Display Students\n";
        cout << "3. Search Student\n";
        cout << "4. Update Student\n";
        cout << "5. Delete Student\n";
        cout << "6. Exit\n";
        cout << "Enter Choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            addStudent();
            break;

        case 2:
            displayStudents();
            break;

        case 3:
            searchStudent();
            break;

        case 4:
            updateStudent();
            break;

        case 5:
            deleteStudent();
            break;

        case 6:
            cout << "\nThank You!\n";
            break;

        default:
            cout << "Invalid Choice!\n";
        }

    } while (choice != 6);

    return 0;
}
