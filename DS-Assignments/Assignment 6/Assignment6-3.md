# Assignment NO 6.3

# Title : Write a program that maintains a queue of passengers waiting to see a ticket agent. The program user should be able to insert a new passenger at the rear of the queue, Display the passenger at the front of the Queue, or remove the passenger at the front of the queue. The program will display the number of passengers left in the queue just before it terminates.

# Theory
A queue is a linear data structure in which elements are inserted at the rear and removed from the front. It is suitable for real-life situations where services are provided in the order of arrival, such as passengers waiting to see a ticket agent.

In this program, when a new passenger arrives, their information is inserted at the rear of the queue. The passenger at the front of the queue is displayed to show who will be served next. When a passenger is served, they are removed from the front of the queue.

The program also keeps track of the number of passengers remaining in the queue. Just before termination, it displays the count of passengers still waiting, ensuring proper monitoring of the queue.

Thus, this system demonstrates how queues efficiently manage waiting lines and ensure fair service without skipping or reordering passengers.

# CODE
```cpp
#include <iostream>
#include <string>
using namespace std;

struct Node {
    string name;
    Node* next;
};

class PassengerQueue {
private:
    Node* front;
    Node* rear;

public:
    PassengerQueue() {
        front = rear = NULL;
    }

    void insertPassenger(string pname) {
        Node* temp = new Node();
        temp->name = pname;
        temp->next = NULL;

        if (front == NULL) {
            front = rear = temp;
        } else {
            rear->next = temp;
            rear = temp;
        }

        cout << pname << " added to queue.\n";
    }

    void frontPassenger() {
        if (front == NULL) {
            cout << "No passengers in queue.\n";
            return;
        }
        cout << "Next passenger: " << front->name << endl;
    }

    void removePassenger() {
        if (front == NULL) {
            cout << "No passenger to remove.\n";
            return;
        }

        Node* temp = front;
        cout << "Serving passenger: " << temp->name << endl;

        front = front->next;
        if (front == NULL)
            rear = NULL;

        delete temp;
    }

    // Count remaining passengers
    int countPassengers() {
        int count = 0;
        Node* temp = front;

        while (temp != NULL) {
            count++;
            temp = temp->next;
        }
        return count;
    }
};

int main() {
    PassengerQueue pq;
    int choice;
    string name;

    do {
        cout << "\n1. Insert Passenger\n2. Front Passenger\n3. Remove Passenger\n4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter passenger name: ";
                cin >> name;
                pq.insertPassenger(name);
                break;

            case 2:
                pq.frontPassenger();
                break;

            case 3:
                pq.removePassenger();
                break;

            case 4:
                cout << "\nTotal passengers left in queue = "
                     << pq.countPassengers() << endl;
                break;
        }
    } while (choice != 4);

    return 0;
}
```
# OUTPUT

1. Insert Passenger
2. Front Passenger
3. Remove Passenger
4. Exit
Enter your choice: 1
Enter passenger name: Suresh
Suresh added to queue.

1. Insert Passenger
2. Front Passenger
3. Remove Passenger
4. Exit
Enter your choice: 1
Enter passenger name: Neha
Neha added to queue.

1. Insert Passenger
2. Front Passenger
3. Remove Passenger
4. Exit
Enter your choice: 1
Enter passenger name: Amit
Amit added to queue.

1. Insert Passenger
2. Front Passenger
3. Remove Passenger
4. Exit
Enter your choice: 2
Next passenger: Suresh

1. Insert Passenger
2. Front Passenger
3. Remove Passenger
4. Exit
Enter your choice: 3
Serving passenger: Suresh

1. Insert Passenger
2. Front Passenger
3. Remove Passenger
4. Exit
Enter your choice: 2
Next passenger: Neha

1. Insert Passenger
2. Front Passenger
3. Remove Passenger
4. Exit
Enter your choice: 3
Serving passenger: Neha

1. Insert Passenger
2. Front Passenger
3. Remove Passenger
4. Exit
Enter your choice: 4

Total passengers left in queue = 1

