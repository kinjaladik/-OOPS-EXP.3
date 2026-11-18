# -OOPS-EXP.3
C++ program demonstrating all types of constructors ( default ,parametrized ,copy ,move ) ,applying the rule of 5 verify 

#include <iostream>
#include <utility> // for std::move

using namespace std;

class Student {
private:
    int *marks;

public:
    // 1. Default Constructor
    Student() {
        marks = new int(0);
        cout << "Default constructor\n";
    }

    // 2. Parameterized Constructor
    Student(int m) {
        marks = new int(m);
        cout << "Parameterized constructor\n";
    }

    // 3. Copy Constructor 
    Student(const Student &s) {
        marks = new int(*(s.marks));
        cout << "Copy constructor\n";
    }

    // 4. Move Constructor
    Student(Student &&s) noexcept {
        marks = s.marks;
        s.marks = nullptr; // Nullify source pointer
        cout << "Move constructor\n";
    }

    // 5. Copy Operator
    Student &operator=(const Student &s) {
        if (this != &s) {
            delete marks; // Prevent memory leak
            marks = new int(*(s.marks));
        }
        cout << "Copy Assignment\n";
        return *this;
    }

    // 6. Move Operator 
    Student &operator=(Student &&s) noexcept {
        if (this != &s) {
            delete marks; // Clean up current resource
            marks = s.marks;
            s.marks = nullptr;
        }
        cout << "Move Assignment\n";
        return *this;
    }

    // Destructor
    ~Student() {
        delete marks;
        cout << "Destructor\n";
    }};

int main() {
    Student s1;             // Default constructor
    Student s2(90);         // Parameterized constructor
    Student s3 = s2;        // Copy constructor
    Student s4 = move(s2);  // Move constructor
    s1 = s3;                // Copy assignment

    return 0;
}
