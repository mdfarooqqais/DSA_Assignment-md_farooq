# Assignment NO 5.2

# Title : Convert given infix expression Eg. a-b*c-d/e+f into postfix form using stack and show the operations step by step.

# Theory 
An infix expression is one in which operators are written between operands, such as a - b * c. A postfix expression (also called Reverse Polish Notation) places the operator after the operands, such as a b c * -. Postfix expressions do not require parentheses and are easier for computers to evaluate.

The conversion process uses a stack to temporarily store operators. Operands are directly added to the postfix expression, while operators are pushed onto the stack based on their precedence and associativity. Higher-precedence operators like * and / are handled before lower-precedence operators like + and -.

During the conversion, if an operator with lower or equal precedence is encountered, operators are popped from the stack and added to the postfix expression before pushing the new operator. At the end of the process, any remaining operators in the stack are popped and appended to the postfix expression.

Showing the conversion step by step helps in understanding how the stack is used to manage operators and ensures correct postfix expression generation.

# CODE 
```cpp
#include <iostream>
#include <stack>
#include <string>
#include <cctype>
using namespace std;

int prec(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/') return 2;
    if (op == '^') return 3;
    return 0;
}

bool isOperator(char c) {
    return c=='+'||c=='-'||c=='*'||c=='/'||c=='^';
}

string stackToString(stack<char> st) {
    string s;
    string rev;
    while (!st.empty()) {
        rev.push_back(st.top());
        st.pop();
    }
    for (int i = (int)rev.size()-1; i>=0; --i) s.push_back(rev[i]);
    return s;
}

string convertWithSteps(const string &infix) {
    stack<char> st;
    string output;
    cout << "Token | Action                                | Stack (bottom->top) | Output\n";
    cout << "--------------------------------------------------------------------------\n";

    for (size_t i = 0; i < infix.size(); ++i) {
        char token = infix[i];
        if (isspace(token)) continue;

        if (isalnum(token)) { 
            output.push_back(token);
            cout << "  " << token << "   | append operand                      | "
                 << stackToString(st) << " | " << output << "\n";
        }
        else if (token == '(') {
            st.push(token);
            cout << "  (   | push '('                           | "
                 << stackToString(st) << " | " << output << "\n";
        }
        else if (token == ')') {
            while (!st.empty() && st.top() != '(') {
                output.push_back(st.top()); st.pop();
            }
            if (!st.empty() && st.top() == '(') st.pop(); 
            cout << "  )   | pop until '('\n"
                 << "      |                                    | "
                 << stackToString(st) << " | " << output << "\n";
        }
        else if (isOperator(token)) {
            string action;
            while (!st.empty() && isOperator(st.top()) &&
                   ( prec(st.top()) > prec(token) ||
                    (prec(st.top()) == prec(token) && token != '^') )) {
                output.push_back(st.top());
                char popped = st.top(); st.pop();
                action += string("pop ") + popped + " to output; ";
            }
            st.push(token);
            if (action.empty()) action = string("push ") + token;
            cout << "  " << token << "   | " << action;
            if (action.size() < 36) cout << string(36 - action.size(), ' ');
            cout << " | " << stackToString(st) << " | " << output << "\n";
        }
        else {

        }
    }

    
    while (!st.empty()) {
        if (st.top() == '(' || st.top() == ')') { st.pop(); continue; }
        output.push_back(st.top()); st.pop();
        cout << "end   | pop remaining op to output         | "
             << stackToString(st) << " | " << output << "\n";
    }

    cout << "--------------------------------------------------------------------------\n";
    cout << "Final Postfix: " << output << "\n";
    return output;
}

int main() {
    string expr = "a-b*c-d/e+f";
    cout << "Infix: " << expr << "\n\n";
    convertWithSteps(expr);
    return 0;
}
```

# OUTPUT
`a - b * c - d / e + f`

Final Postfix: abc*-de-f+
