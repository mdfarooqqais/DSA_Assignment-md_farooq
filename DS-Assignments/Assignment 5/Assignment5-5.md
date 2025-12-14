# Assignment NO 5.5

# Title : You are given a postfix expression (also known as Reverse Polish Notation) consisting of single-digit operands and binary operators (+, -, *, /). Your task is to evaluate the expression using stack and return its result.

# Theory

A postfix expression, also called Reverse Polish Notation, places operators after their operands. For example, the postfix form of 3 + 4 is 3 4 +. Postfix expressions do not require parentheses and are easy for machines to evaluate.

A stack is used to evaluate the postfix expression efficiently. As the expression is scanned from left to right, operands (digits) are pushed onto the stack. When an operator is encountered, the top two operands are popped from the stack, the operation is performed, and the result is pushed back onto the stack.

This process continues until the entire expression is processed. After evaluation, the stack contains a single value, which represents the final result of the postfix expression.

Using a stack ensures correct order of operations and provides a simple and effective way to evaluate postfix expressions in compilers and calculators.

# CODE
```cpp
#include <iostream>
#include <stack>
#include <cctype>
using namespace std;

int evaluatePostfix(string exp) {
    stack<int> st;

    for (char ch : exp) {
        if (isdigit(ch)) {
            st.push(ch - '0');  // convert char to int
        }
        else {  // operator
            int op2 = st.top(); st.pop();
            int op1 = st.top(); st.pop();

            int result;

            switch (ch) {
                case '+': result = op1 + op2; break;
                case '-': result = op1 - op2; break;
                case '*': result = op1 * op2; break;
                case '/': result = op1 / op2; break;
            }

            st.push(result);
        }
    }

    return st.top();
}

int main() {
    string exp;
    cout << "Enter postfix expression: ";
    cin >> exp;

    int ans = evaluatePostfix(exp);
    cout << "Result = " << ans << endl;

    return 0;
}
```
# OUTPUT

# Input:

Postfix: 53+82-*

# Final Answer:
48