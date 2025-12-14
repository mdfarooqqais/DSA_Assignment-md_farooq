# Assignment N0 1.1

# Title : Implement basic string operations such as length calculation, copy, reverse, and concatenation using character single dimensional arrays without using built-in string library functions.

# Thoery :

In C++, strings can be handled using character arrays, where a string is stored as a sequence of characters ending with a null character ('\0'). Even though C++ provides the string class and built-in functions, implementing basic string operations manually helps in understanding low-level string handling.

String length calculation is done by counting characters in the array until the null character is found.
String copying involves copying each character from the source array to the destination array, including the null character.
String reversal is performed by swapping characters from the beginning and end of the array until the middle is reached.
String concatenation appends one character array to the end of another by copying characters of the second array after the null character of the first.

These operations demonstrate how strings are manipulated using single-dimensional character arrays and improve understanding of memory usage, indexing, and loop-based processing in C++.


# CODE 
```cpp
#include <iostream>
using namespace std;

// Function to calculate length
int stringLength(char str[]) {
    int i = 0;
    while (str[i] != '\0') {
        i++;
    }
    return i;
}

void stringCopy(char dest[], char src[]) {
    int i = 0;
    while (src[i] != '\0') {
        dest[i] = src[i];
        i++;
    }
    dest[i] = '\0';
}

void stringReverse(char str[]) {
    int start = 0;
    int end = stringLength(str) - 1;
    while (start < end) {
        char temp = str[start];
        str[start] = str[end];
        str[end] = temp;
        start++;
        end--;
    }
}

void stringConcat(char str1[], char str2[]) {
    int len1 = stringLength(str1);
    int i = 0;

    while (str2[i] != '\0') {
        str1[len1 + i] = str2[i];
        i++;
    }
    str1[len1 + i] = '\0';
}

int main() {
    char str1[100], str2[100], copyStr[100];

    cout << "Enter first string: ";
    cin >> str1;

    cout << "Enter second string: ";
    cin >> str2;

    cout << "\nLength of first string: " << stringLength(str1);

    stringCopy(copyStr, str1);
    cout << "\nCopied string: " << copyStr;

    stringReverse(str1);
    cout << "\nReversed first string: " << str1;

    stringConcat(copyStr, str2);
    cout << "\nConcatenated (copyStr + str2): " << copyStr << endl;

    return 0;
}
```

# Output 

Enter first string: Hello
Enter second string: World

Length of first string: 5
Copied string: Hello
Reversed first string: olleH
Concatenated (copyStr + str2): HelloWorld

