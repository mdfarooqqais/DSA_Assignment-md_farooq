# Assignment NO 6.2

# Title : Pizza parlour accepting maximum n orders. Orders are served on an FCFS basis. Order once placed can’t be cancelled. Write C++ program to simulate the system using circular QUEUE.

# Theory 

In a pizza parlour, orders are accepted up to a fixed maximum limit and are served in the same order in which they are placed. A circular queue is suitable here because it efficiently utilizes memory by reusing empty spaces created after serving orders, unlike a simple linear queue.

When a customer places an order, it is enqueued at the rear of the circular queue. Orders are dequeued from the front when they are prepared and served. Since an order cannot be cancelled once placed, only enqueue and dequeue operations are required.

The circular nature of the queue ensures that when the rear reaches the end of the queue, it can wrap around to the beginning if space is available. This avoids unnecessary wastage of memory.

Thus, implementing the pizza parlour system using a circular queue provides an efficient, organized, and realistic simulation of order handling in real-life food service systems.

# CODE 
```cpp
#include <iostream>
using namespace std;

class CircularQueue {
private:
    int *queue;
    int front, rear, size;

public:
    CircularQueue(int n) {
        size = n;
        queue = new int[n];
        front = rear = -1;
    }

    
    void addOrder(int orderNo) {
        if ((front == 0 && rear == size - 1) || (rear + 1) % size == front) {
            cout << "Order Rejected! Queue is FULL.\n";
            return;
        }

        if (front == -1) { 
            front = rear = 0;
        } else {
            rear = (rear + 1) % size;
        }

        queue[rear] = orderNo;
        cout << "Order " << orderNo << " added successfully.\n";
    }

    
    void serveOrder() {
        if (front == -1) {
            cout << "No orders to serve! Queue is EMPTY.\n";
            return;
        }

        cout << "Order " << queue[front] << " served.\n";

        if (front == rear) { 
            front = rear = -1; 
        } else {
            front = (front + 1) % size;
        }
    }

    void displayOrders() {
        if (front == -1) {
            cout << "No pending orders.\n";
            return;
        }

        cout << "Pending orders: ";
        int i = front;
        while (true) {
            cout << queue[i] << " ";
            if (i == rear) break;
            i = (i + 1) % size;
        }
        cout << endl;
    }
};

int main() {
    int n, choice, orderNo;

    cout << "Enter maximum number of orders: ";
    cin >> n;

    CircularQueue cq(n);

    do {
        cout << "\n1. Add Order\n2. Serve Order\n3. Display Orders\n4. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter order number: ";
                cin >> orderNo;
                cq.addOrder(orderNo);
                break;

            case 2:
                cq.serveOrder();
                break;

            case 3:
                cq.displayOrders();
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

Enter maximum number of orders: 5

1. Add Order
2. Serve Order
3. Display Orders
4. Exit
Enter choice: 1
Enter order number: 101
Order 101 added successfully.

1. Add Order
2. Serve Order
3. Display Orders
4. Exit
Enter choice: 1
Enter order number: 102
Order 102 added successfully.

1. Add Order
2. Serve Order
3. Display Orders
4. Exit
Enter choice: 1
Enter order number: 103
Order 103 added successfully.

1. Add Order
2. Serve Order
3. Display Orders
4. Exit
Enter choice: 3
Pending orders: 101 102 103

1. Add Order
2. Serve Order
3. Display Orders
4. Exit
Enter choice: 2
Order 101 served.

1. Add Order
2. Serve Order
3. Display Orders
4. Exit
Enter choice: 3
Pending orders: 102 103

1. Add Order
2. Serve Order
3. Display Orders
4. Exit
Enter choice: 1
Enter order number: 104
Order 104 added successfully.

1. Add Order
2. Serve Order
3. Display Orders
4. Exit
Enter choice: 1
Enter order number: 105
Order 105 added successfully.

1. Add Order
2. Serve Order
3. Display Orders
4. Exit
Enter choice: 1
Enter order number: 106
Order Rejected! Queue is FULL.

1. Add Order
2. Serve Order
3. Display Orders
4. Exit
Enter choice: 3
Pending orders: 102 103 104 105

1. Add Order
2. Serve Order
3. Display Orders
4. Exit
Enter choice: 4
Exiting...

