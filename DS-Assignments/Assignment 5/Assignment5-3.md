# Assignment NO 5.3

# Title : Write a program to implement multiple stack i.e more than two stack using array and perform following operations on it. A. Push B. Pop C. Stack Overflow D. Stack Underflow E. Display

# Theory
A stack is a linear data structure that follows the LIFO (Last In, First Out) principle. In a multiple stack implementation, more than one stack is stored within a single array to utilize memory efficiently. Each stack is assigned a separate region of the array or managed using boundary indices.

The program supports standard stack operations such as push (inserting an element into a specific stack) and pop (removing the top element from a specific stack). Stack overflow occurs when a push operation is attempted on a stack that has reached its maximum allocated size. Stack underflow occurs when a pop operation is attempted on an empty stack.

The display operation allows viewing the elements of a selected stack from top to bottom. Proper management of stack pointers or top indices is essential to ensure that operations on one stack do not affect the others.

Thus, implementing multiple stacks using an array demonstrates efficient memory management and helps understand how multiple logical data structures can share a single physical storage space.

# CODE

```cpp
#include <iostream>
using namespace std;

class MultiStack {
public:
    int *arr;
    int *top;
    int n, k, segment;

    MultiStack(int n, int k) {
        this->n = n;
        this->k = k;

        arr = new int[n];
        top = new int[k];

        segment = n / k;

        for (int i = 0; i < k; i++) {
            top[i] = (i * segment) - 1;
        }
    }

    bool isOverflow(int s) {
        int end = (s * segment) + segment - 1;
        return top[s] == end;
    }

    bool isUnderflow(int s) {
        int start = s * segment;
        return top[s] < start;
    }

    void push(int s, int val) {
        if (isOverflow(s)) {
            cout << "Stack " << s << " Overflow!" << endl;
            return;
        }
        top[s]++;
        arr[top[s]] = val;
        cout << "Pushed " << val << " into Stack " << s << endl;
    }

    void pop(int s) {
        if (isUnderflow(s)) {
            cout << "Stack " << s << " Underflow!" << endl;
            return;
        }
        int value = arr[top[s]];
        cout << "Popped " << value << " from Stack " << s << endl;
        top[s]--;
    }

    void display(int s) {
        if (isUnderflow(s)) {
            cout << "Stack " << s << " is empty!" << endl;
            return;
        }
        cout << "Stack " << s << ": ";
        int start = s * segment;
        for (int i = top[s]; i >= start; i--) {
            cout << arr[i] << " ";
        }
        cout << endl;
    }
};

int main() {
    int n, k;
    cout << "Enter array size: ";
    cin >> n;
    cout << "Enter number of stacks: ";
    cin >> k;

    MultiStack ms(n, k);

    int choice, s, val;

    do {
        cout << "\n1. Push\n2. Pop\n3. Display\n4. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            cout << "Enter stack number (0 to " << k-1 << "): ";
            cin >> s;
            cout << "Enter value: ";
            cin >> val;
            ms.push(s, val);
            break;

        case 2:
            cout << "Enter stack number (0 to " << k-1 << "): ";
            cin >> s;
            ms.pop(s);
            break;

        case 3:
            cout << "Enter stack number (0 to " << k-1 << "): ";
            cin >> s;
            ms.display(s);
            break;

        case 4:
            cout << "Exiting...\n";
            break;
        }
    } while (choice != 4);

    return 0;
}

# OUTPUT

Enter array size: 12
Enter number of stacks: 3

1. Push
2. Pop
3. Display
4. Exit
Enter choice: 1
Enter stack number (0 to 2): 0
Enter value: 10
Pushed 10 into Stack 0

1. Push
2. Pop
3. Display
4. Exit
Enter choice: 1
Enter stack number (0 to 2): 0
Enter value: 20
Pushed 20 into Stack 0

1. Push
2. Pop
3. Display
4. Exit
Enter choice: 1
Enter stack number (0 to 2): 1
Enter value: 30
Pushed 30 into Stack 1

1. Push
2. Pop
3. Display
4. Exit
Enter choice: 1
Enter stack number (0 to 2): 2
Enter value: 40
Pushed 40 into Stack 2

1. Push
2. Pop
3. Display
4. Exit
Enter choice: 3
Enter stack number (0 to 2): 0
Stack 0: 20 10

1. Push
2. Pop
3. Display
4. Exit
Enter choice: 3
Enter stack number (0 to 2): 1
Stack 1: 30

1. Push
2. Pop
3. Display
4. Exit
Enter choice: 2
Enter stack number (0 to 2): 0
Popped 20 from Stack 0

1. Push
2. Pop
3. Display
4. Exit
Enter choice: 3
Enter stack number (0 to 2): 0
Stack 0: 10

1. Push
2. Pop
3. Display
4. Exit
Enter choice: 2
Enter stack number (0 to 2): 0
Popped 10 from Stack 0

1. Push
2. Pop
3. Display
4. Exit
Enter choice: 2
Enter stack number (0 to 2): 0
Stack 0 Underflow!

1. Push
2. Pop
3. Display
4. Exit
Enter choice: 4
Exiting...
