# Assignment NO 1.4
# Develop a program to identify and efficiently store a sparse matrix using compact representation and perform basic operations like display and simple transpose.

# THeory 
A sparse matrix is a matrix in which most of the elements are zero. Storing all elements of such a matrix using a normal 2D array wastes memory and processing time. To overcome this, a compact representation also called triplet form is used, where only non-zero elements are stored along with their row and column indices.

In compact representation, the first row stores the total number of rows, columns, and non-zero elements. Each subsequent row stores a non-zero element in the form: row index, column index, and value. This representation significantly reduces memory usage and improves efficiency.

Displaying a sparse matrix using compact form involves printing only the stored non-zero entries, making it easier to analyze matrix contents. Simple transpose of a sparse matrix is performed by swapping the row and column values of each non-zero element in the compact representation, without reconstructing the full matrix.

Thus, compact storage and simple transpose operations make sparse matrix handling memory-efficient and faster, especially in scientific computing, image processing, and large data applications.

# Code 
```cpp
#include <iostream>
using namespace std;

int main() {
    int m, n, matrix[50][50];
    int compact[200][3], trans[200][3];
    int i, j, k = 1, nonZero = 0;

    cout << "Enter rows and columns: ";
    cin >> m >> n;

    cout << "Enter matrix elements:\n";
    for (i = 0; i < m; i++)
        for (j = 0; j < n; j++) {
            cin >> matrix[i][j];
            if (matrix[i][j] != 0)
                nonZero++;
        }

    compact[0][0] = m;
    compact[0][1] = n;
    compact[0][2] = nonZero;

    k = 1;
    for (i = 0; i < m; i++)
        for (j = 0; j < n; j++)
            if (matrix[i][j] != 0) {
                compact[k][0] = i;
                compact[k][1] = j;
                compact[k][2] = matrix[i][j];
                k++;
            }

    trans[0][0] = compact[0][1]; 
    trans[0][1] = compact[0][0];
    trans[0][2] = compact[0][2];

    int t = 1;
    for (int col = 0; col < n; col++)
        for (int x = 1; x <= nonZero; x++)
            if (compact[x][1] == col) {
                trans[t][0] = compact[x][1];
                trans[t][1] = compact[x][0];
                trans[t][2] = compact[x][2];
                t++;
            }

    cout << "\nOriginal Matrix:\n";
    for (i = 0; i < m; i++) {
        for (j = 0; j < n; j++)
            cout << matrix[i][j] << " ";
        cout << endl;
    }

    cout << "\nCompact (Triplet) Representation:\n";
    for (i = 0; i <= nonZero; i++)
        cout << compact[i][0] << " " << compact[i][1] << " " << compact[i][2] << endl;

    cout << "\nSimple Transpose (Triplet):\n";
    for (i = 0; i <= nonZero; i++)
        cout << trans[i][0] << " " << trans[i][1] << " " << trans[i][2] << endl;

    return 0;
}

````
# OUTPUT
Enter rows and columns: 3 3
Enter matrix elements:
0 0 5
0 8 0
3 0 0

Original Matrix:
0 0 5
0 8 0
3 0 0

Compact (Triplet) Representation:
3 3 3
0 2 5
1 1 8
2 0 3

Simple Transpose (Triplet):
3 3 3
2 0 5
1 1 8
0 2 3

