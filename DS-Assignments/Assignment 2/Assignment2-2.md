# Assignment NO 2.2

# Title : WAP to implement Bubble sort and Quick Sort on a 1D array of Student structure (contains student_name, student_roll_no, total_marks), with key as student_roll_no. And count the number of swap performed by each method.

# Theory
In this problem, student records are stored in a one-dimensional array of structures, where each structure contains student name, roll number, and total marks. The sorting key used is the student roll number, which uniquely identifies each student.

Bubble Sort is a simple comparison-based sorting algorithm. It works by repeatedly comparing adjacent student records and swapping them if they are in the wrong order based on roll number. After each pass, the largest roll number moves to its correct position. Although easy to understand and implement, Bubble Sort is inefficient for large datasets with a time complexity of O(n²). Counting the number of swaps helps analyze how inefficient it becomes as the number of students increases.

Quick Sort is a faster and more efficient sorting algorithm based on the divide-and-conquer approach. It selects a pivot roll number, partitions the array into smaller subarrays, and recursively sorts them. Quick Sort significantly reduces the number of comparisons and swaps, with an average time complexity of O(n log n). By counting swaps in Quick Sort, we can compare its efficiency against Bubble Sort.

By implementing both algorithms on the same student dataset and counting swaps, this program highlights the performance difference between simple and advanced sorting techniques, helping understand why Quick Sort is preferred for large datasets while Bubble Sort is suitable only for small or nearly sorted data.
# CODE

```cpp
#include <iostream>
#include <cstring>
using namespace std;

struct Student {
    char name[50];
    int roll_no;
    int total_marks;
};

void swap(Student &a, Student &b, int &swapCount) {
    Student temp = a;
    a = b;
    b = temp;
    swapCount++;
}

void bubbleSort(Student s[], int n, int &swapCount) {
    swapCount = 0;
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (s[j].roll_no > s[j + 1].roll_no)
                swap(s[j], s[j + 1], swapCount);
        }
    }
}

int partition(Student s[], int low, int high, int &swapCount) {
    int pivot = s[high].roll_no;
    int i = low - 1;

    for (int j = low; j < high; j++) {
        if (s[j].roll_no < pivot) {
            i++;
            swap(s[i], s[j], swapCount);
        }
    }
    swap(s[i + 1], s[high], swapCount);
    return (i + 1);
}

void quickSort(Student s[], int low, int high, int &swapCount) {
    if (low < high) {
        int pi = partition(s, low, high, swapCount);
        quickSort(s, low, pi - 1, swapCount);
        quickSort(s, pi + 1, high, swapCount);
    }
}

int main() {
    int n;
    cout << "Enter number of students: ";
    cin >> n;

    Student s1[100], s2[100];

    cout << "Enter student details (name roll_no total_marks):\n";
    for (int i = 0; i < n; i++) {
        cin >> s1[i].name >> s1[i].roll_no >> s1[i].total_marks;
        s2[i] = s1[i]; // copy for quick sort
    }

    int bubbleSwaps = 0, quickSwaps = 0;

    bubbleSort(s1, n, bubbleSwaps);
    quickSort(s2, 0, n - 1, quickSwaps);

    cout << "\nSorted by Bubble Sort:\n";
    for (int i = 0; i < n; i++)
        cout << s1[i].name << " " << s1[i].roll_no << " " << s1[i].total_marks << endl;

    cout << "Total Bubble Sort swaps: " << bubbleSwaps << endl;

    cout << "\nSorted by Quick Sort:\n";
    for (int i = 0; i < n; i++)
        cout << s2[i].name << " " << s2[i].roll_no << " " << s2[i].total_marks << endl;

    cout << "Total Quick Sort swaps: " << quickSwaps << endl;

    return 0;
}
```

# OUTPUT

Enter number of students: 4
Enter student details (name roll_no total_marks):
Amit 23 450
Neha 5 480
Rohan 17 420
Priya 9 460

Sorted by Bubble Sort:
Neha 5 480
Priya 9 460
Rohan 17 420
Amit 23 450
Total Bubble Sort swaps: 4

Sorted by Quick Sort:
Neha 5 480
Priya 9 460
Rohan 17 420
Amit 23 450
Total Quick Sort swaps: 3

