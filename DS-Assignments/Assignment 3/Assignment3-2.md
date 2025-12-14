Assigment No 3.2
# Title : The ticket reservation system for Galaxy Multiplex is to be implemented using a C++ program. The multiplex has 8 rows, with 8 seats in each row. A doubly circular linked list will be used to track the availability of seats in each row.Initially, assume that some seats are randomly booked. An array will store head pointers for each row’s linked list.The system should support the following operations: a) Display the current list of available seats. b) Book one or more seats as per customer request. c) Cancel an existing booking when requested.


# Theory
In this problem, a ticket reservation system for a multiplex is designed using a doubly circular linked list, which is an efficient data structure for managing seat availability. Each seat is represented as a node containing seat information, along with pointers to both the previous and next seats. The circular nature ensures that traversal can continue seamlessly from the last seat back to the first seat.

The multiplex consists of 8 rows with 8 seats in each row, and an array of head pointers is used where each array index represents one row. Each head pointer points to the doubly circular linked list of seats for that particular row. This makes row-wise seat management simple and organized.

The system supports essential operations such as displaying available seats, booking one or more seats, and canceling bookings. Booking a seat involves updating the seat status in the linked list, while cancellation restores the seat’s availability. The doubly circular linked list allows easy forward and backward traversal, making booking and cancellation operations efficient.

Overall, this approach provides a dynamic and scalable solution for seat reservation, avoids unnecessary memory shifting, and ensures smooth management of seat availability in a real-time multiplex environment.

# CODE 

```cpp
#include <iostream>
using namespace std;

struct Node {
    int seat_no;
    int status;     
    Node* next;
    Node* prev;
};

Node* head[8];  


void createMultiplex() {
    for (int r = 0; r < 8; r++) {
        head[r] = NULL;

        for (int s = 1; s <= 8; s++) {
            Node* temp = new Node;
            temp->seat_no = s;
            temp->status = 0;
            temp->next = temp->prev = NULL;

            if (head[r] == NULL) {
                head[r] = temp;
                temp->next = temp;
                temp->prev = temp;
            } else {
                Node* last = head[r]->prev;
                last->next = temp;
                temp->prev = last;
                temp->next = head[r];
                head[r]->prev = temp;
            }
        }
    }
}


void displayAvailable() {
    cout << "\n--- AVAILABLE SEATS ---\n";
    for (int r = 0; r < 8; r++) {
        cout << "Row " << r+1 << ": ";
        Node* ptr = head[r];

        do {
            if (ptr->status == 0)
                cout << ptr->seat_no << " ";
            ptr = ptr->next;
        } while (ptr != head[r]);

        cout << endl;
    }
}


void bookSeat(int row, int seat) {
    Node* ptr = head[row-1];

    do {
        if (ptr->seat_no == seat) {
            if (ptr->status == 1) {
                cout << "Seat already booked.\n";
                return;
            }
            ptr->status = 1;
            cout << "Seat booked successfully!\n";
            return;
        }
        ptr = ptr->next;
    } while (ptr != head[row-1]);
}


void cancelSeat(int row, int seat) {
    Node* ptr = head[row-1];

    do {
        if (ptr->seat_no == seat) {
            if (ptr->status == 0) {
                cout << "Seat is not booked.\n";
                return;
            }
            ptr->status = 0;
            cout << "Booking cancelled.\n";
            return;
        }
        ptr = ptr->next;
    } while (ptr != head[row-1]);
}

int main() {
    createMultiplex();

    int choice, row, seat;

    while (1) {
        cout << "\n1. Display Available Seats";
        cout << "\n2. Book Seat";
        cout << "\n3. Cancel Seat";
        cout << "\n4. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                displayAvailable();
                break;

            case 2:
                cout << "Enter row (1-8): ";
                cin >> row;
                cout << "Enter seat (1-8): ";
                cin >> seat;
                bookSeat(row, seat);
                break;

            case 3:
                cout << "Enter row (1-8): ";
                cin >> row;
                cout << "Enter seat (1-8): ";
                cin >> seat;
                cancelSeat(row, seat);
                break;

            case 4:
                return 0;

            default:
                cout << "Invalid choice!\n";
        }
    }
}
```

# OUTPUT
1. Display Available Seats
2. Book Seat
3. Cancel Seat
4. Exit
Enter choice: 1

--- AVAILABLE SEATS ---
Row 1: 1 2 3 4 5 6 7 8
Row 2: 1 2 3 4 5 6 7 8
Row 3: 1 2 3 4 5 6 7 8
Row 4: 1 2 3 4 5 6 7 8
Row 5: 1 2 3 4 5 6 7 8
Row 6: 1 2 3 4 5 6 7 8
Row 7: 1 2 3 4 5 6 7 8
Row 8: 1 2 3 4 5 6 7 8


1. Display Available Seats
2. Book Seat
3. Cancel Seat
4. Exit
Enter choice: 2
Enter row (1-8): 3
Enter seat (1-8): 5
Seat booked successfully!


1. Display Available Seats
2. Book Seat
3. Cancel Seat
4. Exit
Enter choice: 2
Enter row (1-8): 3
Enter seat (1-8): 5
Seat already booked.


1. Display Available Seats
2. Book Seat
3. Cancel Seat
4. Exit
Enter choice: 1

--- AVAILABLE SEATS ---
Row 1: 1 2 3 4 5 6 7 8
Row 2: 1 2 3 4 5 6 7 8
Row 3: 1 2 3 4 6 7 8
Row 4: 1 2 3 4 5 6 7 8
Row 5: 1 2 3 4 5 6 7 8
Row 6: 1 2 3 4 5 6 7 8
Row 7: 1 2 3 4 5 6 7 8
Row 8: 1 2 3 4 5 6 7 8


1. Display Available Seats
2. Book Seat
3. Cancel Seat
4. Exit
Enter choice: 3
Enter row (1-8): 3
Enter seat (1-8): 5
Booking cancelled.


1. Display Available Seats
2. Book Seat
3. Cancel Seat
4. Exit
Enter choice: 1

--- AVAILABLE SEATS ---
Row 1: 1 2 3 4 5 6 7 8
Row 2: 1 2 3 4 5 6 7 8
Row 3: 1 2 3 4 5 6 7 8
R

