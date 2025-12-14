# Assignment NO 6.4

# Title : Write a program to implement multiple queues i.e. two queues using array and perform following operations on it. A. Add Queue, B. Delete from Queue, C. Display Queue

# Algorithm 

A queue is a linear data structure that follows the First-Come, First-Served (FCFS) principle, where insertion is done at the rear and deletion is done from the front. In this program, a single array is logically divided to represent two independent queues, allowing better utilization of available memory.

The Add Queue (enqueue) operation inserts an element into the selected queue after checking for overflow conditions. The Delete from Queue (dequeue) operation removes an element from the front of the chosen queue, handling underflow conditions when the queue is empty.

The Display Queue operation shows the elements present in each queue in proper order. Separate front and rear indices are maintained for both queues to ensure correct and independent operations.

Thus, implementing two queues using an array helps in understanding memory sharing, queue operations, and efficient data management using linear data structures.

# COD
```cpp
#include <iostream>
using namespace std;

class TwoQueues {
private:
    int arr[100], n, mid;
    int front1, rear1;
    int front2, rear2;

public:
    TwoQueues(int size) {
        n = size;
        mid = (n / 2) - 1;

        front1 = rear1 = -1;
        front2 = rear2 = -1;
    }

    void enqueue(int q, int value) {

        if (q == 1) {   
            if (rear1 == mid) {
                cout << "Queue 1 Overflow!\n";
                return;
            }
            if (front1 == -1) {
                front1 = rear1 = 0;
            } else {
                rear1++;
            }
            arr[rear1] = value;
            cout << "Inserted " << value << " into Queue 1\n";
        }

        else if (q == 2) {   
            if (rear2 == n - 1) {
                cout << "Queue 2 Overflow!\n";
                return;
            }
            if (front2 == -1) {
                front2 = rear2 = mid + 1;
            } else {
                rear2++;
            }
            arr[rear2] = value;
            cout << "Inserted " << value << " into Queue 2\n";
        }
    }


    void dequeue(int q) {

        if (q == 1) {
            if (front1 == -1) {
                cout << "Queue 1 Underflow!\n";
                return;
            }
            cout << "Deleted " << arr[front1] << " from Queue 1\n";
            if (front1 == rear1)
                front1 = rear1 = -1;
            else
                front1++;
        }

        else if (q == 2) {
            if (front2 == -1) {
                cout << "Queue 2 Underflow!\n";
                return;
            }
            cout << "Deleted " << arr[front2] << " from Queue 2\n";
            if (front2 == rear2)
                front2 = rear2 = -1;
            else
                front2++;
        }
    }


    void display(int q) {

        if (q == 1) {
            if (front1 == -1) {
                cout << "Queue 1 is empty\n";
                return;
            }
            cout << "Queue 1: ";
            for (int i = front1; i <= rear1; i++)
                cout << arr[i] << " ";
            cout << endl;
        }

        else if (q == 2) {
            if (front2 == -1) {
                cout << "Queue 2 is empty\n";
                return;
            }
            cout << "Queue 2: ";
            for (int i = front2; i <= rear2; i++)
                cout << arr[i] << " ";
            cout << endl;
        }
    }
};

int main() {
    int size;
    cout << "Enter total array size: ";
    cin >> size;

    TwoQueues t(size);

    int choice, q, val;

    do {
        cout << "\n1. Add to Queue\n2. Delete from Queue\n3. Display Queue\n4. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter queue number (1 or 2): ";
                cin >> q;
                cout << "Enter value: ";
                cin >> val;
                t.enqueue(q, val);
                break;

            case 2:
                cout << "Enter queue number (1 or 2): ";
                cin >> q;
                t.dequeue(q);
                break;

            case 3:
                cout << "Enter queue number (1 or 2): ";
                cin >> q;
                t.display(q);
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
Enter total array size: 10

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Exit
Enter choice: 1
Enter queue number (1 or 2): 1
Enter value: 10
Inserted 10 into Queue 1

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Exit
Enter choice: 1
Enter queue number (1 or 2): 1
Enter value: 20
Inserted 20 into Queue 1

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Exit
Enter choice: 1
Enter queue number (1 or 2): 2
Enter value: 50
Inserted 50 into Queue 2

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Exit
Enter choice: 1
Enter queue number (1 or 2): 2
Enter value: 60
Inserted 60 into Queue 2

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Exit
Enter choice: 3
Enter queue number (1 or 2): 1
Queue 1: 10 20

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Exit
Enter choice: 3
Enter queue number (1 or 2): 2
Queue 2: 50 60

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Exit
Enter choice: 2
Enter queue number (1 or 2): 1
Deleted 10 from Queue 1

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Exit
Enter choice: 3
Enter queue number (1 or 2): 1
Queue 1: 20

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Exit
Enter choice: 2
Enter queue number (1 or 2): 2
Deleted 50 from Queue 2

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Exit
Enter choice: 3
Enter queue number (1 or 2): 2
Queue 2: 60

1. Add to Queue
2. Delete from Queue
3. Display Queue
4. Exit
Enter choice: 4
Exiting...
