# Assignment NO 2.5

# Write a program to arrange the list of employees as per the average of their height and weight by using Merge and Selection sorting method. Analyse their time complexities and conclude which algorithm will take less time to sort the list. 

# Theory 
In this problem, employees are arranged based on the average of height and weight, which acts as the sorting key. Two comparison-based sorting algorithms are used: Selection Sort and Merge Sort.

Selection Sort works by repeatedly selecting the smallest element (based on the key) from the unsorted portion of the list and placing it at the correct position. It performs comparisons even if the list is already sorted.

Merge Sort follows the divide and conquer approach. The list is recursively divided into smaller sublists until each contains one element, then these sublists are merged back in sorted order based on the average value.

# CODE

```cpp
#include <iostream>
#include <cstring>
using namespace std;

struct Employee {
    char name[50];
    float height, weight, avg;
};


void selectionSort(Employee e[], int n) {
    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;
        for (int j = i + 1; j < n; j++) {
            if (e[j].avg < e[minIndex].avg)
                minIndex = j;
        }
        Employee temp = e[i];
        e[i] = e[minIndex];
        e[minIndex] = temp;
    }
}


void merge(Employee e[], int low, int mid, int high) {
    Employee temp[100];
    int i = low, j = mid + 1, k = low;

    while (i <= mid && j <= high) {
        if (e[i].avg <= e[j].avg)
            temp[k++] = e[i++];
        else
            temp[k++] = e[j++];
    }

    while (i <= mid)
        temp[k++] = e[i++];
    while (j <= high)
        temp[k++] = e[j++];

    for (int x = low; x <= high; x++)
        e[x] = temp[x];
}


void mergeSort(Employee e[], int low, int high) {
    if (low < high) {
        int mid = (low + high) / 2;
        mergeSort(e, low, mid);
        mergeSort(e, mid + 1, high);
        merge(e, low, mid, high);
    }
}

int main() {
    int n;

    cout << "Enter number of employees: ";
    cin >> n;

    Employee e1[100], e2[100];

    cout << "\nEnter Employee Details (Name Height Weight):\n";
    for (int i = 0; i < n; i++) {
        cin >> e1[i].name >> e1[i].height >> e1[i].weight;
        e1[i].avg = (e1[i].height + e1[i].weight) / 2;
        e2[i] = e1[i];   
    }

    
    selectionSort(e1, n);

    cout << "\nSorted List using Selection Sort:\n";
    for (int i = 0; i < n; i++)
        cout << e1[i].name << " " << e1[i].avg << endl;

    
    mergeSort(e2, 0, n - 1);

    cout << "\nSorted List using Merge Sort:\n";
    for (int i = 0; i < n; i++)
        cout << e2[i].name << " " << e2[i].avg << endl;

    return 0;
}
```
# OUTPUT

Enter number of employees: 3

Enter Employee Details (Name Height Weight):
Amit 170 70
Neha 160 60
Rohit 180 90

Sorted List using Selection Sort:
Neha 110
Amit 120
Rohit 135

Sorted List using Merge Sort:
Neha 110
Amit 120
Rohit 135
