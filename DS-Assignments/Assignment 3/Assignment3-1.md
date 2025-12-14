 # Assignment No  3.1

 # Title : Implementation of Singly Linked List to Manage ‘Vertex Club’ Membership Records.
The Department of Computer Engineering has a student club named ‘Vertex Club’ for second, third, and final year students. The first member is the President and the last member is the Secretary. Write a C++ program to:
●	Add/delete members (including President/Secretary)
●	Count members
●	Display members
●	Concatenate two division lists
 Also implement: reverse, search by PRN, and sort by PRN operations.


# Theory 
A Singly Linked List is a dynamic data structure where each node contains student information and a pointer to the next node. It is suitable for managing club membership records because members can be added or removed easily without shifting elements, unlike arrays.

In the Vertex Club, members belong to different academic years, and the list maintains an order where the first node represents the President and the last node represents the Secretary. Using a singly linked list allows insertion and deletion at the beginning, end, or any position, which is required to handle changes in leadership and membership.

The linked list supports counting members by traversing nodes sequentially, displaying members in order, and concatenating two division lists by linking the last node of one list to the first node of another. This structure efficiently handles growing or shrinking membership records while maintaining proper order and roles within the club.

# CODE
```cpp
#include <iostream>
using namespace std;

struct Node {
    int PRN;
    string name;
    Node* next;
};

class VertexClub {
public:
    Node* head;

    VertexClub() {
        head = NULL;
    }

    Node* createNode(int prn, string name) {
        Node* temp = new Node;
        temp->PRN = prn;
        temp->name = name;
        temp->next = NULL;
        return temp;
    }

    void addPresident(int prn, string name) {
        Node* temp = createNode(prn, name);
        temp->next = head;
        head = temp;
    }

    void addSecretary(int prn, string name) {
        Node* temp = createNode(prn, name);

        if (!head) {
            head = temp;
            return;
        }

        Node* trav = head;
        while (trav->next != NULL)
            trav = trav->next;

        trav->next = temp;
    }

    void addMember(int afterPRN, int prn, string name) {
        Node* trav = head;

        while (trav && trav->PRN != afterPRN)
            trav = trav->next;

        if (!trav) {
            cout << "PRN not found.\n";
            return;
        }

        Node* temp = createNode(prn, name);
        temp->next = trav->next;
        trav->next = temp;
    }

    void deletePresident() {
        if (!head)
            return;

        Node* temp = head;
        head = head->next;
        delete temp;
    }

    void deleteSecretary() {
        if (!head)
            return;

        if (!head->next) {
            delete head;
            head = NULL;
            return;
        }

        Node* trav = head;
        while (trav->next->next != NULL)
            trav = trav->next;

        delete trav->next;
        trav->next = NULL;
    }

    void deleteByPRN(int prn) {
        if (!head)
            return;

        if (head->PRN == prn) {
            deletePresident();
            return;
        }

        Node* trav = head;
        while (trav->next && trav->next->PRN != prn)
            trav = trav->next;

        if (!trav->next) {
            cout << "Member not found.\n";
            return;
        }

        Node* temp = trav->next;
        trav->next = temp->next;
        delete temp;
    }

    void display() {
        Node* trav = head;

        if (!trav) {
            cout << "List empty\n";
            return;
        }

        cout << "\n--- Vertex Club Members ---\n";
        while (trav) {
            cout << trav->PRN << " - " << trav->name << endl;
            trav = trav->next;
        }
    }

    int count() {
        int cnt = 0;
        Node* trav = head;
        while (trav) {
            cnt++;
            trav = trav->next;
        }
        return cnt;
    }

    bool search(int prn) {
        Node* trav = head;
        while (trav) {
            if (trav->PRN == prn)
                return true;
            trav = trav->next;
        }
        return false;
    }

    void reverse() {
        Node *prev = NULL, *curr = head, *next;

        while (curr) {
            next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }
        head = prev;
    }

    void sortPRN() {
        if (!head) return;

        for (Node* i = head; i->next != NULL; i = i->next) {
            for (Node* j = head; j->next != NULL; j = j->next) {
                if (j->PRN > j->next->PRN) {
                    swap(j->PRN, j->next->PRN);
                    swap(j->name, j->next->name);
                }
            }
        }
    }

    void concatenate(VertexClub &obj) {
        if (!head) {
            head = obj.head;
            return;
        }

        Node* trav = head;
        while (trav->next != NULL)
            trav = trav->next;

        trav->next = obj.head;
    }
};

int main() {
    VertexClub divA, divB;

    // Division A
    divA.addPresident(1, "Aarav");
    divA.addMember(1, 5, "Rohan");
    divA.addSecretary(10, "Suresh");

    cout << "\nDivision A:";
    divA.display();

    // Division B
    divB.addPresident(2, "Neha");
    divB.addSecretary(20, "Pooja");

    cout << "\nDivision B:";
    divB.display();

    cout << "\nAfter Concatenation:";
    divA.concatenate(divB);
    divA.display();

    cout << "\nTotal Members: " << divA.count() << endl;

    cout << "\nSorting by PRN...";
    divA.sortPRN();
    divA.display();

    cout << "\nReversing list...";
    divA.reverse();
    divA.display();

    cout << "\nSearching for PRN 5: ";
    cout << (divA.search(5) ? "Found" : "Not Found") << endl;

    return 0;
}
```
# OUTPUT 
Division A:

--- Vertex Club Members ---
1 - Aarav
5 - Rohan
10 - Suresh

Division B:

--- Vertex Club Members ---
2 - Neha
20 - Pooja

After Concatenation:

--- Vertex Club Members ---
1 - Aarav
5 - Rohan
10 - Suresh
2 - Neha
20 - Pooja

Total Members: 5

Sorting by PRN...

--- Vertex Club Members ---
1 - Aarav
2 - Neha
5 - Rohan
10 - Suresh
20 - Pooja

Reversing list...

--- Vertex Club Members ---
20 - Pooja
10 - Suresh
5 - Rohan
2 - Neha
1 - Aarav

Searching for PRN 5: Found
