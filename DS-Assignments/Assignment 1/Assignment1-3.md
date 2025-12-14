# Assignment No 1.3

# Implement matrix multiplication and analyse its performance using row-major vs column-major order access patterns to understand how memory layout affects cache performance

# Theory
Matrix multiplication computes a result matrix where each element is obtained by multiplying a row of the first matrix with a column of the second matrix and summing the products. In most programming languages like C and C++, matrices are stored in row-major order, meaning consecutive elements of a row are stored contiguously in memory.

When matrix multiplication is implemented using row-major access, the inner loop accesses memory sequentially. This pattern aligns well with CPU cache behavior, resulting in fewer cache misses and faster execution. In contrast, column-major style access (accessing elements column by column in a row-major stored matrix) causes non-contiguous memory access, leading to frequent cache misses and degraded performance.

Thus, even though both approaches have the same theoretical time complexity of O(n³), their actual execution time differs significantly. Row-major traversal is faster due to better spatial locality, while column-major traversal is slower because it repeatedly loads new cache lines.

This analysis highlights that algorithm efficiency is not determined only by Big-O notation, but also by how data is laid out and accessed in memory. Understanding memory hierarchy and cache behavior is critical for writing high-performance matrix algorithms.

# Code 
```cpp
#include <iostream>
using namespace std;

int main() {
    int rows, cols;
    cout << "Enter number of rows and columns: ";
    cin >> rows >> cols;

    int A[20][20];
    cout << "Enter matrix elements:\n";

    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++)
            cin >> A[i][j];

    int count = 0;
    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++)
            if (A[i][j] != 0)
                count++;

    int SP[100][3];
    SP[0][0] = rows;
    SP[0][1] = cols;
    SP[0][2] = count;

    int k = 1;
    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++)
            if (A[i][j] != 0) {
                SP[k][0] = i;
                SP[k][1] = j;
                SP[k][2] = A[i][j];
                k++;
            }

    cout << "\nSparse Matrix (Triplet Representation):\n";
    cout << "Row  Col  Val\n";
    for (int i = 0; i <= count; i++)
        cout << SP[i][0] << "    " << SP[i][1] << "    " << SP[i][2] << endl;

    int TR[100][3];
    TR[0][0] = SP[0][1]; 
    TR[0][1] = SP[0][0];
    TR[0][2] = SP[0][2];

    int t = 1;
    for (int col = 0; col < cols; col++)
        for (int i = 1; i <= count; i++)
            if (SP[i][1] == col) { 
                TR[t][0] = SP[i][1];
                TR[t][1] = SP[i][0];
                TR[t][2] = SP[i][2];
                t++;
            }

    cout << "\nTranspose (Triplet Representation):\n";
    cout << "Row  Col  Val\n";
    for (int i = 0; i <= count; i++)
        cout << TR[i][0] << "    " << TR[i][1] << "    " << TR[i][2] << endl;

    return 0;
}

````
# OUTPUT
Enter number of rows and columns: 3 3
Enter matrix elements:
1 0 0
0 2 0
0 0 3

Sparse Matrix (Triplet Representation):
Row  Col  Val
3    3    3
0    0    1
1    1    2
2    2    3

Transpose (Triplet Representation):
Row  Col  Val
3    3    3
0    0    1
1    1    2
2    2    3
