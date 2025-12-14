# Assignment No 3.5

# Title : Write a C++ program to store a binary number using a doubly linked list. Implement the following functions: a) Calculate and display the 1’s complement and 2’s complement of the stored binary number. b) Perform addition of two binary numbers represented using doubly linked lists and display the result.

# Theory

In this problem, a binary number is stored using a doubly linked list, where each node represents a single binary digit (0 or 1). The use of a doubly linked list allows traversal in both forward and backward directions, which is especially useful for binary arithmetic operations that require carry propagation from the least significant bit to the most significant bit.

The 1’s complement of a binary number is obtained by flipping each bit, that is, converting 0 to 1 and 1 to 0. This operation is straightforward when traversing the linked list node by node. The 2’s complement is computed by adding 1 to the 1’s complement, which requires backward traversal to handle carry efficiently. A doubly linked list simplifies this process compared to a singly linked list.

For binary addition, two binary numbers stored as doubly linked lists are added starting from the least significant bit. Carry generation and propagation are handled by moving backward through the lists, and the result is stored in a new linked list. Overall, using a doubly linked list provides flexibility, efficient traversal, and a clear structural representation for implementing binary complement and addition operations.

# CODE

```cpp
#include <iostream>
using namespace std;

struct Node {
    int bit;        
    Node* next;
    Node* prev;
    Node(int b) { bit = b; next = prev = NULL; }
};

void append(Node*& head, Node*& tail, int b) {
    Node* temp = new Node(b);
    if (!head) {
        head = tail = temp;
    } else {
        tail->next = temp;
        temp->prev = tail;
        tail = temp;
    }
}

void display(Node* head) {
    while (head) {
        cout << head->bit;
        head = head->next;
    }
    cout << endl;
}

Node* copyList(Node* head) {
    Node *newHead = NULL, *newTail = NULL;
    while (head) {
        append(newHead, newTail, head->bit);
        head = head->next;
    }
    return newHead;
}


Node* onesComplement(Node* head) {
    Node *newHead = NULL, *newTail = NULL;
    while (head) {
        append(newHead, newTail, head->bit == 0 ? 1 : 0);
        head = head->next;
    }
    return newHead;
}

Node* twosComplement(Node* head) {
    Node* oneComp = onesComplement(head);

    Node* p = oneComp;
    while (p->next) p = p->next;  

    int carry = 1;
    while (p && carry) {
        int sum = p->bit + carry;
        p->bit = sum % 2;
        carry = sum / 2;
        p = p->prev;
    }

    if (carry == 1) {
        Node *newHead = new Node(1);
        newHead->next = oneComp;
        oneComp->prev = newHead;
        return newHead;
    }

    return oneComp;
}

Node* addBinary(Node* A, Node* B) {
    
    Node *p = A, *q = B;
    while (p->next) p = p->next;
    while (q->next) q = q->next;

    Node *resHead = NULL, *resTail = NULL;
    int carry = 0;

    while (p || q || carry) {
        int bitA = p ? p->bit : 0;
        int bitB = q ? q->bit : 0;

        int sum = bitA + bitB + carry;
        append(resHead, resTail, sum % 2);
        carry = sum / 2;

        if (p) p = p->prev;
        if (q) q = q->prev;
    }

    Node *cur = resHead, *prev = NULL, *next = NULL;
    while (cur) {
        next = cur->next;
        cur->next = prev;
        cur->prev = next;
        prev = cur;
        cur = next;
    }
    return prev;  
}

Node* stringToDLL(string s) {
    Node *head = NULL, *tail = NULL;
    for (char c : s) {
        append(head, tail, c - '0');
    }
    return head;
}

int main() {
    string bin1, bin2;

    cout << "Enter first binary number: ";
    cin >> bin1;
    cout << "Enter second binary number: ";
    cin >> bin2;

    Node* A = stringToDLL(bin1);
    Node* B = stringToDLL(bin2);

    cout << "\nStored Binary A: ";
    display(A);
    cout << "Stored Binary B: ";
    display(B);

    cout << "\n1's Complement of A: ";
    Node* oneA = onesComplement(A);
    display(oneA);

    cout << "2's Complement of A: ";
    Node* twoA = twosComplement(A);
    display(twoA);

    cout << "\nA + B = ";
    Node* sum = addBinary(A, B);
    display(sum);

    return 0;
}
```
# OUTPUT

Enter first binary number: 1011
Enter second binary number: 1101

Stored Binary A: 1011
Stored Binary B: 1101

1's Complement of A: 0100
2's Complement of A: 0101

A + B = 11000
