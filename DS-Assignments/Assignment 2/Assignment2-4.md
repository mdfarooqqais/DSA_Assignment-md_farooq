 # Assignment No 2.4 
 
 # Write a program using Bubble sort algorithm, assign the roll nos. to the students of your class as per their previous years result. i.e. topper will be roll no. 1 and analyse the sorting algorithm pass by pass.

# Theory 
Bubble Sort is a simple comparison-based sorting algorithm used to arrange data in a desired order. In this problem, Bubble Sort is applied to arrange students based on their previous year results so that roll numbers can be assigned according to merit. The algorithm works by repeatedly comparing adjacent students’ marks and swapping them if they are in the wrong order. For assigning roll numbers, the sorting is done in descending order of marks so that the student with the highest marks (topper) comes first.

The sorting process is carried out in multiple passes. In each pass, the highest remaining mark “bubbles up” to its correct position in the list. After every pass, the relative order of the top-performing students becomes fixed. Analyzing the algorithm pass by pass helps in understanding how the list gradually becomes sorted.

Once the students are completely sorted based on marks, roll numbers are assigned sequentially. The first student in the sorted list is given roll number 1, the second is given roll number 2, and so on. Thus, Bubble Sort helps in ranking students fairly and transparently based on their academic performance.

# CODE

```cpp
#include <iostream>
#include <cstring>
using namespace std;

struct Student {
    char name[50];
    int marks;
    int roll;
};

void swap(Student &a, Student &b) {
    Student temp = a;
    a = b;
    b = temp;
}

int main() {
    int n;
    cout << "Enter number of students: ";
    cin >> n;

    Student s[100];

    cout << "\nEnter student details (name marks):\n";
    for (int i = 0; i < n; i++)
        cin >> s[i].name >> s[i].marks;

    cout << "\n--- Bubble Sort Pass-by-Pass ---\n";

    
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (s[j].marks < s[j + 1].marks)
                swap(s[j], s[j + 1]);
        }

        // Print pass
        cout << "Pass " << i + 1 << ": ";
        for (int k = 0; k < n; k++)
            cout << s[k].marks << " ";
        cout << endl;
    }

    
    for (int i = 0; i < n; i++)
        s[i].roll = i + 1;

    
    cout << "\n--- Final Assigned Roll Numbers ---\n";
    for (int i = 0; i < n; i++)
        cout << "Roll " << s[i].roll << ": " << s[i].name
             << " (" << s[i].marks << " marks)" << endl;

    return 0;
}
```
# OUTPUT

Enter number of students: 4

Enter student details (name marks):
Amit 78
Riya 92
Neha 85
Rahul 70

--- Bubble Sort Pass-by-Pass ---
Pass 1: 92 85 78 70
Pass 2: 92 85 78 70
Pass 3: 92 85 78 70

--- Final Assigned Roll Numbers ---
Roll 1: Riya (92 marks)
Roll 2: Neha (85 marks)
Roll 3: Amit (78 marks)
Roll 4: Rahul (70 marks)
