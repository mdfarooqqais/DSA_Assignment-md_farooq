# Assignment No 1.5

# Develop a program to compute the fast transpose of a sparse matrix using its compact (triplet) representation efficiently.

# Theory
A sparse matrix is a matrix in which most of the elements are zero. To save memory and processing time, such matrices are stored in compact (triplet) form, where each non-zero element is represented as a triple: (row, column, value), along with a header storing total rows, columns, and number of non-zero elements.

The fast transpose method is an optimized technique to transpose a sparse matrix efficiently. Instead of scanning the entire matrix repeatedly, it uses additional arrays to count how many non-zero elements occur in each column and calculates the exact position where each transposed element should be placed. This avoids unnecessary comparisons and reduces time complexity.

In fast transpose, the row and column indices of each triplet are swapped, and elements are placed directly into their correct positions in the transposed matrix. This approach significantly improves performance compared to simple transpose methods, especially for large sparse matrices.

Fast transpose is widely used in applications like scientific computing, image processing, and database systems where large sparse data structures are common and efficiency is critical.

# Code 
```cpp
#include <iostream>
using namespace std;

int main() {
    int compact[100][3], trans[100][3];
    int m, n, t;

    cout << "Enter number of rows, columns and non-zero elements: ";
    cin >> m >> n >> t;

    cout << "Enter compact matrix (row col value):\n";
    for (int i = 0; i <= t; i++)
        cin >> compact[i][0] >> compact[i][1] >> compact[i][2];

    int count[50] = {0}, start[50] = {0};

    
    trans[0][0] = compact[0][1];
    trans[0][1] = compact[0][0];
    trans[0][2] = compact[0][2];

    
    for (int i = 1; i <= t; i++)
        count[compact[i][1]]++;

    
    start[0] = 1;
    for (int i = 1; i < n; i++)
        start[i] = start[i - 1] + count[i - 1];

    
    for (int i = 1; i <= t; i++) {
        int col = compact[i][1];
        int pos = start[col]++;

        trans[pos][0] = compact[i][1];
        trans[pos][1] = compact[i][0];
        trans[pos][2] = compact[i][2];
    }

    cout << "\nFast Transpose (Triplet Representation):\n";
    for (int i = 0; i <= t; i++)
        cout << trans[i][0] << " " << trans[i][1] << " " << trans[i][2] << endl;

    return 0;
}
````
# OUTPUT
Enter number of rows, columns and non-zero elements: 3 3 3
Enter compact matrix (row col value):
3 3 3
0 1 5
1 0 8
2 2 6

Fast Transpose (Triplet Representation):
3 3 3
0 1 8
1 0 5
2 2 6
