# Assignment NO 4.4

# Title : WAP to create a doubly linked list and perform following operations on it. A) Insert (all cases)  2) Delete (all cases).

# Thoery 
A doubly linked list is a dynamic data structure in which each node contains three parts: data, a pointer to the previous node, and a pointer to the next node. Unlike a singly linked list, it allows traversal in both forward and backward directions.

In this program, a doubly linked list is created to perform insertion and deletion operations in all cases.
Insertion can be done at the beginning, end, or at a specific position in the list by properly updating the previous and next pointers of the affected nodes.
Deletion operations also include removing a node from the beginning, end, or a specific position, ensuring that the links between remaining nodes are maintained correctly.

Using a doubly linked list makes insertion and deletion operations more efficient compared to arrays, as shifting of elements is not required. Proper pointer adjustment is the key aspect while performing all operations.

Thus, doubly linked lists provide flexibility, ease of traversal, and efficient memory usage for managing dynamic data.

# CODE

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
    Node* prev;
};

Node* head = NULL;

void insertBegin(int x) {
    Node* p = new Node();
    p->data = x;
    p->prev = NULL;
    p->next = head;

    if (head != NULL)
        head->prev = p;

    head = p;
}

void insertEnd(int x) {
    Node* p = new Node();
    p->data = x;
    p->next = NULL;

    if (head == NULL) {
        p->prev = NULL;
        head = p;
        return;
    }

    Node* temp = head;
    while (temp->next != NULL)
        temp = temp->next;

    temp->next = p;
    p->prev = temp;
}

void insertAfter(int val, int x) {
    Node* temp = head;

    while (temp != NULL && temp->data != val)
        temp = temp->next;

    if (temp == NULL) {
        cout << "Value not found!\n";
        return;
    }

    Node* p = new Node();
    p->data = x;
    p->next = temp->next;
    p->prev = temp;

    if (temp->next != NULL)
        temp->next->prev = p;

    temp->next = p;
}

void deleteBegin() {
    if (head == NULL) return;

    Node* temp = head;
    head = head->next;

    if (head != NULL)
        head->prev = NULL;

    delete temp;
}

void deleteEnd() {
    if (head == NULL) return;

    if (head->next == NULL) {
        delete head;
        head = NULL;
        return;
    }

    Node* temp = head;
    while (temp->next != NULL)
        temp = temp->next;

    temp->prev->next = NULL;
    delete temp;
}

void deleteValue(int x) {
    Node* temp = head;

    while (temp != NULL && temp->data != x)
        temp = temp->next;

    if (temp == NULL) {
        cout << "Value not found!\n";
        return;
    }

    if (temp == head) {
        deleteBegin();
        return;
    }

    if (temp->next != NULL)
        temp->next->prev = temp->prev;

    temp->prev->next = temp->next;

    delete temp;
}

void display() {
    Node* temp = head;
    cout << "DLL: ";
    while (temp != NULL) {
        cout << temp->data << " <-> ";
        temp = temp->next;
    }
    cout << "NULL\n";
}

int main() {
    insertBegin(30);
    insertBegin(20);
    insertBegin(10);

    cout << "After inserting 10,20,30 at beginning:\n";
    display();

    insertEnd(40);
    insertEnd(50);
    cout << "\nAfter inserting 40,50 at end:\n";
    display();

    insertAfter(30, 35);
    cout << "\nAfter inserting 35 after 30:\n";
    display();

    deleteBegin();
    cout << "\nAfter deleting from beginning:\n";
    display();

    deleteEnd();
    cout << "\nAfter deleting from end:\n";
    display();

    deleteValue(35);
    cout << "\nAfter deleting value 35:\n";
    display();

    return 0;
}
```
# OUTPUT 

# Initial Insertions

# InsertBegin(30)
# InsertBegin(20)
# InsertBegin(10)

# InsertEnd(40), InsertEnd(50)

# InsertAfter(30, 35)

# DeleteBegin()

# DeleteEnd()

# DeleteValue(35)

# Output

After inserting 10,20,30 at beginning:
DLL: 10  20  30  NULL

After inserting 40,50 at end:
DLL: 10  20  30  40  50  NULL

After inserting 35 after 30:
DLL: 10  20  30  35  40  50  NULL

After deleting from beginning:
DLL: 20  30  35 40  50 NULL

After deleting from end:
DLL: 20 30 35 40 NULL

After deleting value 35:
DLL: 20 30 40 NULL
