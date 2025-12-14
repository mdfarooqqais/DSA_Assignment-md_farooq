# Assignment NO 4.2

# Title : WAP to perform addition o f two polynomials using singly linked list.

# Theory 

Polynomial addition involves combining two polynomials by adding the coefficients of terms that have the same exponent. Each term of a polynomial consists of a coefficient and an exponent.

A singly linked list is used to represent a polynomial because it allows dynamic storage of terms. Each node in the linked list stores one term of the polynomial and a pointer to the next node.

While performing addition, both polynomial linked lists are traversed together. If the exponents of the current terms are equal, their coefficients are added and the result is stored in a new linked list. If the exponents are different, the term with the higher exponent is copied directly to the result list.

This method is efficient and flexible, as it avoids unnecessary memory usage and easily handles polynomials with different numbers of terms.

# CODE

```cpp
#include <iostream>
using namespace std;

struct Node {
    int coef, exp;
    Node* next;
};

Node* createNode(int c, int e) {
    Node* temp = new Node;
    temp->coef = c;
    temp->exp = e;
    temp->next = NULL;
    return temp;
}

Node* insertEnd(Node* head, int c, int e) {
    Node* temp = createNode(c, e);
    if (!head) return temp;

    Node* p = head;
    while (p->next)
        p = p->next;

    p->next = temp;
    return head;
}

void display(Node* head) {
    if (!head) { cout << "0"; return; }

    Node* p = head;
    while (p) {
        cout << p->coef << "x^" << p->exp;
        if (p->next) cout << " + ";
        p = p->next;
    }
    cout << endl;
}

Node* addPolynomial(Node* P1, Node* P2) {
    Node *ptr1 = P1, *ptr2 = P2;
    Node* result = NULL;

    while (ptr1 && ptr2) {
        if (ptr1->exp == ptr2->exp) {
            result = insertEnd(result, ptr1->coef + ptr2->coef, ptr1->exp);
            ptr1 = ptr1->next;
            ptr2 = ptr2->next;
        }
        else if (ptr1->exp > ptr2->exp) {
            result = insertEnd(result, ptr1->coef, ptr1->exp);
            ptr1 = ptr1->next;
        }
        else {
            result = insertEnd(result, ptr2->coef, ptr2->exp);
            ptr2 = ptr2->next;
        }
    }

    while (ptr1) {
        result = insertEnd(result, ptr1->coef, ptr1->exp);
        ptr1 = ptr1->next;
    }

    while (ptr2) {
        result = insertEnd(result, ptr2->coef, ptr2->exp);
        ptr2 = ptr2->next;
    }

    return result;
}

int main() {
    Node *P1 = NULL, *P2 = NULL, *RES = NULL;

    int n1, n2, c, e;

    cout << "Enter number of terms in Polynomial 1: ";
    cin >> n1;
    cout << "Enter coef & exp: \n";
    for (int i = 0; i < n1; i++) {
        cin >> c >> e;
        P1 = insertEnd(P1, c, e);
    }

    cout << "\nEnter number of terms in Polynomial 2: ";
    cin >> n2;
    cout << "Enter coef & exp: \n";
    for (int i = 0; i < n2; i++) {
        cin >> c >> e;
        P2 = insertEnd(P2, c, e);
    }

    cout << "\nPolynomial 1: ";
    display(P1);

    cout << "Polynomial 2: ";
    display(P2);

    RES = addPolynomial(P1, P2);

    cout << "\nResult (P1 + P2): ";
    display(RES);

    return 0;
}
```
# OUTPUT

Enter number of terms in Polynomial 1: 3
Enter coef & exp: 
5 3
4 2
2 1

Enter number of terms in Polynomial 2: 3
Enter coef & exp: 
6 3
3 2
5 0

Polynomial 1: 5x^3 + 4x^2 + 2x^1
Polynomial 2: 6x^3 + 3x^2 + 5x^0

Result (P1 + P2): 11x^3 + 7x^2 + 2x^1 + 5x^0

