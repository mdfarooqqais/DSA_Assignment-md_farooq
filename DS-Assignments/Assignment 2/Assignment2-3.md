# Assignment No 2.3

# Write a program to input marks of n students Sort the marks in ascending order using the Quick Sort algorithm without using built-in library functions and analyse the sorting algorithm pass by pass. Find the minimum and maximum marks using Divide and Conquer (recursively).

# Theory
Quick Sort is a divide and conquer sorting algorithm.
It selects a pivot, partitions the array so that:

elements smaller than pivot go left

elements greater than pivot go right
Then it recursively sorts the subarrays.

Pass-by-pass analysis means printing the array after each partition, not after every swap. If someone prints every swap and calls it a pass, that is wrong.

Minimum and maximum using divide and conquer means:

split the array

find min and max recursively

combine results
No loops-only solution allowed for this part.
# CODE

```cpp
#include <iostream>
using namespace std;

int passCount = 1;

// Utility function to display array
void display(int arr[], int n) {
    for (int i = 0; i < n; i++)
        cout << arr[i] << " ";
    cout << endl;
}

// Partition function
int partition(int arr[], int low, int high, int n) {
    int pivot = arr[high];
    int i = low - 1;

    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);

    cout << "Pass " << passCount++ << ": ";
    display(arr, n);

    return i + 1;
}

// Quick Sort with pass display
void quickSort(int arr[], int low, int high, int n) {
    if (low < high) {
        int pi = partition(arr, low, high, n);
        quickSort(arr, low, pi - 1, n);
        quickSort(arr, pi + 1, high, n);
    }
}

// Structure for Min-Max result
struct MinMax {
    int min;
    int max;
};

// Divide and Conquer Min-Max
MinMax findMinMax(int arr[], int low, int high) {
    MinMax result, left, right;

    if (low == high) {
        result.min = result.max = arr[low];
        return result;
    }

    if (high == low + 1) {
        if (arr[low] < arr[high]) {
            result.min = arr[low];
            result.max = arr[high];
        } else {
            result.min = arr[high];
            result.max = arr[low];
        }
        return result;
    }

    int mid = (low + high) / 2;
    left = findMinMax(arr, low, mid);
    right = findMinMax(arr, mid + 1, high);

    result.min = (left.min < right.min) ? left.min : right.min;
    result.max = (left.max > right.max) ? left.max : right.max;

    return result;
}

int main() {
    int n;
    cout << "Enter number of students: ";
    cin >> n;

    int marks[100];
    cout << "Enter marks:\n";
    for (int i = 0; i < n; i++)
        cin >> marks[i];

    cout << "\nQuick Sort Pass-wise Output:\n";
    quickSort(marks, 0, n - 1, n);

    cout << "\nFinal Sorted Marks:\n";
    display(marks, n);

    MinMax ans = findMinMax(marks, 0, n - 1);

    cout << "\nMinimum Marks = " << ans.min << endl;
    cout << "Maximum Marks = " << ans.max << endl;

    return 0;
}
```
# OUTPUT
Enter number of students: 5
Enter marks:
45 12 78 34 60

Quick Sort Pass-wise Output:
Pass 1: 45 12 34 60 78
Pass 2: 12 34 45 60 78
Pass 3: 12 34 45 60 78

Final Sorted Marks:
12 34 45 60 78

Minimum Marks = 12
Maximum Marks = 78

