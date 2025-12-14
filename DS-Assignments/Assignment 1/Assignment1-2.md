# Assignment No 1.2

# Title : Write a program to construct and verify a magic square of order 'n' (for both even & odd) such that all rows, columns, and diagonals sum to the same value.

# Theory

A magic square is a square matrix of order n × n filled with the numbers from 1 to n² such that the sum of all elements in each row, each column, and both main diagonals is the same. This constant sum is known as the magic constant and is given by the formula:

       Magic Constant=2n(n2+1)​
	​
Magic squares can be constructed for both odd and even values of n, but the construction methods differ.

For odd-order magic squares, a systematic placement method (such as the Siamese method) is used, where numbers are placed by moving upward and diagonally with specific rules to handle boundary conditions and occupied cells.

For even-order magic squares, different strategies are applied depending on whether n is doubly even (divisible by 4) or singly even (divisible by 2 but not by 4), ensuring that all required sums remain equal.

Verification of a magic square involves calculating the sum of each row, each column, and both diagonals, and checking whether they are equal to the magic constant. If all sums match, the square is confirmed to be a valid magic square.

This problem helps in understanding matrix manipulation, conditional logic, and algorithmic problem-solving in programming while reinforcing concepts of arrays and mathematical reasoning.


#  C++ Code

```cpp
#include <iostream>
#include <vector>
using namespace std;


vector<vector<int>> generateOddMagic(int n) {
    vector<vector<int>> magic(n, vector<int>(n, 0));
    int num = 1, i = 0, j = n / 2;

    while (num <= n * n) {
        magic[i][j] = num++;

        int new_i = (i - 1 + n) % n;
        int new_j = (j + 1) % n;

        if (magic[new_i][new_j] != 0)
            i = (i + 1) % n;
        else {
            i = new_i;
            j = new_j;
        }
    }
    return magic;
}

// Doubly even magic square
vector<vector<int>> generateDoublyEvenMagic(int n) {
    vector<vector<int>> magic(n, vector<int>(n));
    int num = 1, rev = n * n;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if ((i % 4 == j % 4) || ((i % 4 + j % 4) == 3))
                magic[i][j] = num;
            else
                magic[i][j] = rev;
            num++;
            rev--;
        }
    }
    return magic;
}

// Verify magic square
bool verifyMagicSquare(vector<vector<int>>& magic, int n) {
    int M = n * (n * n + 1) / 2;

    for (int i = 0; i < n; i++) {
        int rowSum = 0, colSum = 0;
        for (int j = 0; j < n; j++) {
            rowSum += magic[i][j];
            colSum += magic[j][i];
        }
        if (rowSum != M || colSum != M)
            return false;
    }

    int diag1 = 0, diag2 = 0;
    for (int i = 0; i < n; i++) {
        diag1 += magic[i][i];
        diag2 += magic[i][n - i - 1];
    }

    return diag1 == M && diag2 == M;
}

int main() {
    int n;
    cout << "Enter order of the Magic Square (n): ";
    cin >> n;

    vector<vector<int>> magic;

    if (n % 2 == 1)
        magic = generateOddMagic(n);
    else if (n % 4 == 0)
        magic = generateDoublyEvenMagic(n);
    else
        cout << "Singly even magic square generation is not implemented in this code.\n";

    cout << "\nMagic Square of order " << n << ":\n";
    for (auto &row : magic) {
        for (auto &val : row)
            cout << val << "\t";
        cout << endl;
    }

    if (verifyMagicSquare(magic, n))
        cout << "\nVerification Passed: It IS a Magic Square.\n";
    else
        cout << "\nVerification Failed: NOT a Magic Square.\n";

    return 0;
}
```

# Output

Enter order of the Magic Square (n): 3

Magic Square of order 3:
8       1       6
3       5       7
4       9       2

Verification Passed: It is a Magic Square.

Enter order of the Magic Square (n): 4

Magic Square of order 4:
1       15      14      4
12      6       7       9
8       10      11      5
13      3       2       16
