# Assignment NO 6.5

# Title : In a call center, customer calls are handled on a first-come, first-served basis. Implement a queue system using Linked list where: ●	Each customer call is enqueued as it arrives. ●	Customer service agents dequeue calls to assist customers. ●	If there are no calls, the system waits.


# Theory

This problem models a call center system using a queue implemented with a linked list, following the First-Come, First-Served (FCFS) principle.

In a call center, customer calls must be handled in the exact order in which they arrive. A queue is the most appropriate data structure for this scenario because it allows insertion of calls at the rear and removal from the front.

When a customer makes a call, the call is enqueued into the queue. As customer service agents become available, calls are dequeued from the front of the queue and handled one by one. This ensures fairness and prevents skipping of calls.

If there are no calls in the queue, the system remains idle and waits for new incoming calls. Using a linked list implementation allows the queue to grow or shrink dynamically without wasting memory.

Thus, implementing a call center system using a linked list–based queue provides an efficient and realistic way to manage customer calls in real-world service systems.

# CODE
```cpp
#include <iostream>
#include <string>
using namespace std;

struct Node {
    string callID;
    Node* next;
};

class CallQueue {
private:
    Node* front;
    Node* rear;

public:
    CallQueue() {
        front = rear = NULL;
    }

    void enqueue(string id) {
        Node* temp = new Node();
        temp->callID = id;
        temp->next = NULL;

        if (front == NULL) {
            front = rear = temp;
        } else {
            rear->next = temp;
            rear = temp;
        }

        cout << "New call received: " << id << endl;
    }

    void dequeue() {
        if (front == NULL) {
            cout << "No calls. System waiting...\n";
            return;
        }

        Node* temp = front;
        cout << "Agent handling call: " << temp->callID << endl;

        front = front->next;

        if (front == NULL)
            rear = NULL;

        delete temp;
    }

    
    void display() {
        if (front == NULL) {
            cout << "No pending calls.\n";
            return;
        }

        cout << "Pending Calls: ";
        Node* temp = front;
        while (temp) {
            cout << temp->callID << " -> ";
            temp = temp->next;
        }
        cout << "NULL\n";
    }
};

int main() {
    CallQueue cq;
    int choice;
    string id;

    do {
        cout << "\n1. Receive Call\n2. Handle Call\n3. Display Waiting Calls\n4. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter Call ID: ";
                cin >> id;
                cq.enqueue(id);
                break;

            case 2:
                cq.dequeue();
                break;

            case 3:
                cq.display();
                break;

            case 4:
                cout << "System shutting down...\n";
                break;
        }
    } while (choice != 4);

    return 0;
}
```
# OUTPUT

1. Receive Call
2. Handle Call
3. Display Waiting Calls
4. Exit
Enter choice: 1
Enter Call ID: CALL101
New call received: CALL101

1. Receive Call
2. Handle Call
3. Display Waiting Calls
4. Exit
Enter choice: 1
Enter Call ID: CALL102
New call received: CALL102

1. Receive Call
2. Handle Call
3. Display Waiting Calls
4. Exit
Enter choice: 1
Enter Call ID: CALL103
New call received: CALL103

1. Receive Call
2. Handle Call
3. Display Waiting Calls
4. Exit
Enter choice: 3
Pending Calls: CALL101 -> CALL102 -> CALL103 -> NULL

1. Receive Call
2. Handle Call
3. Display Waiting Calls
4. Exit
Enter choice: 2
Agent handling call: CALL101

1. Receive Call
2. Handle Call
3. Display Waiting Calls
4. Exit
Enter choice: 3
Pending Calls: CALL102 -> CALL103 -> NULL

1. Receive Call
2. Handle Call
3. Display Waiting Calls
4. Exit
Enter choice: 2
Agent handling call: CALL102

1. Receive Call
2. Handle Call
3. Display Waiting Calls
4. Exit
Enter choice: 3
Pending Calls: CALL103 -> NULL

1. Receive Call
2. Handle Call
3. Display Waiting Calls
4. Exit
Enter choice: 2
Agent handling call: CALL103

1. Receive Call
2. Handle Call
3. Display Waiting Calls
4. Exit
Enter choice: 2
No calls. System waiting...

1. Receive Call
2. Handle Call
3. Display Waiting Calls
4. Exit
Enter choice: 4
System shutting down...
