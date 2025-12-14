# Assignment NO 5.4

# Title : You are given a string containing only parentheses characters: '(', ')', '{', '}', '[', and ']'. Your task is to check whether the parentheses are balanced or not. A string is considered balanced if: 1.	Every opening bracket has a corresponding closing bracket of the same type 2.	Brackets are closed in the correct order.

# Theory  

A string is said to have balanced parentheses if every opening bracket such as (, {, or [ has a corresponding closing bracket ), }, or ] of the same type, and if the brackets are closed in the correct order.

A stack is used to solve this problem efficiently. Whenever an opening bracket is encountered, it is pushed onto the stack. When a closing bracket is encountered, the stack is checked. If the stack is empty or the top of the stack does not match the type of the closing bracket, the string is considered unbalanced.

After processing the entire string, if the stack is empty, all brackets were properly matched and the string is balanced. If the stack is not empty, it indicates that some opening brackets were not closed.

This approach ensures correct validation of nested and sequential brackets and is widely used in syntax checking, expression evaluation, and compiler design.

# CODE
```cpp
#include <iostream>
#include <stack>
using namespace std;

bool isMatching(char open, char close) {
    return (open == '(' && close == ')') ||
           (open == '{' && close == '}') ||
           (open == '[' && close == ']');
}

bool isBalanced(string s) {
    stack<char> st;

    for (char ch : s) {
        
        if (ch == '(' || ch == '{' || ch == '[') {
            st.push(ch);
        }
        else if (ch == ')' || ch == '}' || ch == ']') {
            if (st.empty())          
                return false;

            char top = st.top();
            st.pop();

            if (!isMatching(top, ch))
                return false;        
        }
    }

    return st.empty();
}

int main() {
    string s;
    cout << "Enter parentheses string: ";
    cin >> s;

    if (isBalanced(s))
        cout << "Balanced\n";
    else
        cout << "Not Balanced\n";

    return 0;
}

# OUTPUT

Input:

({[})


| Character | Action                  | Result         |
| --------- | ----------------------- | -------------- |
| `(`       | push                    | (              |
| `{`       | push                    | ({             |
| `[`       | push                    | ({[            |
| `}`       | expected `]` → mismatch | ❌ Not Balanced |

