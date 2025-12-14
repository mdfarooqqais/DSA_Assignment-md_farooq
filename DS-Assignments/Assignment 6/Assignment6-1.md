# Assignment NO 6.1

# Title : Write a program to keep track of patients as they checked into a medical clinic, assigning patients to doctors on a first-come, first-served basis.

# Theory 
In a medical clinic, patients are attended to in the same order in which they arrive. A queue is the most suitable data structure for this scenario, as it allows insertion of patients at the rear of the queue and removal from the front of the queue.

When a patient checks into the clinic, their details are enqueued into the queue. As doctors become available, patients are dequeued from the front of the queue and assigned for consultation. This ensures fairness and proper order of treatment.

If no patients are present in the queue, the system remains idle until a new patient arrives. Using a queue helps in efficiently managing patient flow without skipping or reordering patients.

Thus, implementing this system using a queue accurately models real-life clinic operations and ensures organized and systematic patient handling.

# CODE 

```cpp
#include <iostream>
#include <string>
using namespace std;

struct Node {
    string name;
    Node* next;
};

class PatientQueue {
private:
    Node* front;
    Node* rear;

public:
    PatientQueue() {
        front = rear = NULL;
    }

    void checkIn(string pname) {
        Node* temp = new Node();
        temp->name = pname;
        temp->next = NULL;

        if (front == NULL) {
            front = rear = temp;
        } else {
            rear->next = temp;
            rear = temp;
        }
        cout << pname << " checked in.\n";
    }

    void assignPatient() {
        if (front == NULL) {
            cout << "No patients waiting.\n";
            return;
        }

        Node* temp = front;
        cout << "Assigning patient: " << temp->name << endl;

        front = front->next;

        if (front == NULL)
            rear = NULL;

        delete temp;
    }

    bool isEmpty() {
        return front == NULL;
    }

    void displayQueue() {
        if (isEmpty()) {
            cout << "No patients waiting.\n";
            return;
        }

        Node* temp = front;
        cout << "Patients in queue: ";
        while (temp) {
            cout << temp->name << " -> ";
            temp = temp->next;
        }
        cout << "NULL\n";
    }
};

int main() {
    PatientQueue clinic;
    int choice;
    string name;

    do {
        cout << "\n1. Check-in Patient\n2. Assign Patient\n3. Display Queue\n4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter patient name: ";
                cin >> name;
                clinic.checkIn(name);
                break;

            case 2:
                clinic.assignPatient();
                break;

            case 3:
                clinic.displayQueue();
                break;

            case 4:
                cout << "Exiting...\n";
                break;
        }
    } while (choice != 4);

    return 0;
}
```
# OUTPUT

1. Check-in Patient
2. Assign Patient
3. Display Queue
4. Exit
Enter your choice: 1
Enter patient name: Rahul
Rahul checked in.

1. Check-in Patient
2. Assign Patient
3. Display Queue
4. Exit
Enter your choice: 1
Enter patient name: Priya
Priya checked in.

1. Check-in Patient
2. Assign Patient
3. Display Queue
4. Exit
Enter your choice: 1
Enter patient name: Amit
Amit checked in.

1. Check-in Patient
2. Assign Patient
3. Display Queue
4. Exit
Enter your choice: 3
Patients in queue: Rahul -> Priya -> Amit -> NULL

1. Check-in Patient
2. Assign Patient
3. Display Queue
4. Exit
Enter your choice: 2
Assigning patient: Rahul

1. Check-in Patient
2. Assign Patient
3. Display Queue
4. Exit
Enter your choice: 3
Patients in queue: Priya -> Amit -> NULL

1. Check-in Patient
2. Assign Patient
3. Display Queue
4. Exit
Enter your choice: 2
Assigning patient: Priya

1. Check-in Patient
2. Assign Patient
3. Display Queue
4. Exit
Enter your choice: 3
Patients in queue: Amit -> NULL

1. Check-in Patient
2. Assign Patient
3. Display Queue
4. Exit
Enter your choice: 4
Exiting...
