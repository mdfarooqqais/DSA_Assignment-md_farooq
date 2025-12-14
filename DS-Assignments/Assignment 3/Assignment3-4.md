# Assignment No 3.4

# Title : In the Second Year Computer Engineering class, there are two groups of students based on their favorite sports:  ●	Set A includes students who like Cricket. ●	Set B includes students who like Football. Write a C++ program to represent these two sets using linked lists and perform the following operations: a) Find and display the set of students who like both Cricket and Football. b) Find and display the set of students who like either Cricket or Football, but not both. c) Display the number of students who like neither Cricket nor Football.

# Theory 
In this problem, sets of students are represented using linked lists, where each node stores a student’s identifier (such as roll number or name).
Two separate linked lists are used: Set A for students who like Cricket and Set B for students who like Football. Linked lists are suitable here because the number of students can vary dynamically, and insertions or deletions can be handled efficiently without shifting elements.

To find students who like both Cricket and Football, the intersection operation is performed by comparing elements of Set A with Set B and storing common elements in a new list.
To find students who like either Cricket or Football but not both, the symmetric difference operation is used, which includes students present in exactly one of the two sets.
The count of students who like neither sport is determined by comparing the total class strength with the number of students present in the union of Set A and Set B.

Overall, using linked lists to represent sets allows efficient implementation of classic set operations such as intersection, symmetric difference, and complement, making it a practical approach for managing and analyzing student preference data in real-world academic scenarios.

# CODE

```cpp
#include <iostream>
using namespace std;

struct Node {
    int roll;
    Node* next;
};

void insert(Node*& head, int r) {
    Node* temp = new Node;
    temp->roll = r;
    temp->next = NULL;

    if (!head) head = temp;
    else {
        Node* p = head;
        while (p->next) p = p->next;
        p->next = temp;
    }
}

bool search(Node* head, int r) {
    while (head) {
        if (head->roll == r)
            return true;
        head = head->next;
    }
    return false;
}

void display(Node* head) {
    if (!head) { cout << "Empty\n"; return; }
    while (head) {
        cout << head->roll << " ";
        head = head->next;
    }
    cout << endl;
}

int countNodes(Node* head) {
    int c = 0;
    while (head) {
        c++;
        head = head->next;
    }
    return c;
}

int main() {
    Node *A = NULL, *B = NULL;
    int nA, nB, totalStudents;

    cout << "Enter total number of students in class: ";
    cin >> totalStudents;

    cout << "\nEnter number of students who like Cricket: ";
    cin >> nA;
    cout << "Enter roll numbers:\n";
    for (int i = 0; i < nA; i++) {
        int x; cin >> x;
        insert(A, x);
    }

    cout << "\nEnter number of students who like Football: ";
    cin >> nB;
    cout << "Enter roll numbers:\n";
    for (int i = 0; i < nB; i++) {
        int x; cin >> x;
        insert(B, x);
    }

    Node* both = NULL;
    Node* p = A;

    while (p) {
        if (search(B, p->roll))
            insert(both, p->roll);
        p = p->next;
    }

    cout << "\nStudents who like BOTH Cricket and Football:\n";
    display(both);

    Node* sym = NULL;

    p = A;
    while (p) {
        if (!search(B, p->roll))
            insert(sym, p->roll);
        p = p->next;
    }

    p = B;
    while (p) {
        if (!search(A, p->roll))
            insert(sym, p->roll);
        p = p->next;
    }

    cout << "\nStudents who like EITHER Cricket OR Football but NOT BOTH:\n";
    display(sym);

    int countA = countNodes(A);
    int countB = countNodes(B);
    int countBoth = countNodes(both);

    int countAtLeastOne = countA + countB - countBoth;
    int neither = totalStudents - countAtLeastOne;

    cout << "\nNumber of students who like NEITHER Cricket nor Football: "
         << neither << endl;

    return 0;
}
```

# OUTPUT 

Enter total number of students in class: 10

Enter number of students who like Cricket: 4
Enter roll numbers:
1 2 3 4

Enter number of students who like Football: 3
Enter roll numbers:
3 4 5

Students who like BOTH Cricket and Football:
3 4

Students who like EITHER Cricket OR Football but NOT BOTH:
1 2 5

Number of students who like NEITHER Cricket nor Football: 3

