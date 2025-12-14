# Assignment No 2.1

# Title: In Computer Engg. Dept. of VIT there are S.Y., T.Y., and B.Tech. students. Assume that all these students are on ground for a function. We need to identify a student of S.Y. div. (X) whose name is "XYZ" and roll no. is "17". Apply appropriate Searching method to identify the required student.

# Theory
In this situation, all students from S.Y., T.Y., and B.Tech. are mixed together on the ground, and there is no guaranteed ordering of records by division, name, or roll number. Therefore, the most appropriate searching technique is Linear Search.

Linear Search works by examining each record one by one until the required student is found or the list ends. Each student record is checked for three conditions simultaneously:

Division must be S.Y. Div X

Name must be "XYZ"

Roll number must be 17

The search starts from the first student and proceeds sequentially. When a record satisfying all conditions is found, the search stops and the student is identified. If no such record exists after checking all students, the result is reported as “student not found”.

Linear Search is suitable here because:

Data is unsorted

Records are heterogeneous (different years and divisions)

Implementation is simple and direct

Although its time complexity is O(n), it is the most practical method in real-life scenarios like this where prior sorting is not feasible.
# Program 

```cpp
#include <iostream>
#include <cstring>
using namespace std;

struct Student {
    char name[50];
    int roll;
    char year[10];
    char div[5];
};

int main() {
    int n;
    cout << "Enter number of students: ";
    cin >> n;

    Student s[100];

    cout << "\nEnter student details:\n";
    for (int i = 0; i < n; i++) {
        cout << "\nStudent " << i + 1 << ":\n";
        cout << "Name: ";
        cin >> s[i].name;
        cout << "Roll: ";
        cin >> s[i].roll;
        cout << "Year (SY/TY/BE): ";
        cin >> s[i].year;
        cout << "Division: ";
        cin >> s[i].div;
    }

    int found = 0;
    for (int i = 0; i < n; i++) {
        if (strcmp(s[i].year, "SY") == 0 &&
            strcmp(s[i].div, "X") == 0 &&
            strcmp(s[i].name, "XYZ") == 0 &&
            s[i].roll == 17) {
            
            found = 1;
            cout << "\nStudent Found!\n";
            cout << "Name: " << s[i].name << endl;
            cout << "Roll No: " << s[i].roll << endl;
            cout << "Year: " << s[i].year << endl;
            cout << "Division: " << s[i].div << endl;
            break;
        }
    }

    if (!found)
        cout << "\nStudent NOT Found.\n";

    return 0;
}
```
# Dry Run

# Input:

Number of students = 4

Student 1: ABC 10 SY X  
Student 2: PQR 12 TY A  
Student 3: XYZ 17 SY X  
Student 4: LMN 22 BE B

# Output:

Student Found!
Name: XYZ
Roll No: 17
Year: SY
Division: X

