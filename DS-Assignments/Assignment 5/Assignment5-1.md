# Assignment NO 5.1

# Title : WAP to build a simple stock price tracker that keeps a history of daily stock prices entered by the user. To allow users to go back and view or remove the most recent price, implement a stack using a linked list to store these integer prices.
# Implement the following operations: 1.	record(price) – Add a new stock price (an integer) to the stack. 2.	remove() – Remove and return the most recent price (top of the stack). 3.	latest() – Return the most recent stock price without removing it. 4. isEmpty() – Check if there are no prices recorded.

# Theory 
This problem deals with splitting a linked list into two separate sublists based on the number of elements present in the list.

The idea is to divide the original list into a front list and a back list. If the total number of nodes is even, both sublists will contain an equal number of elements. If the number of nodes is odd, the extra element is placed in the front list, ensuring the front list is always equal or larger in size than the back list.

To achieve this correctly, special attention is required for boundary cases such as lists with fewer than two nodes. For very small lists, direct handling is necessary to avoid errors like null pointer access. The logic should also work consistently for larger lists once it handles small cases properly.

A common approach is to first determine the length of the list or use a two-pointer technique (slow and fast pointers) to find the midpoint. Once the midpoint is identified, the list is broken into two parts by adjusting the links.

This operation is important in many linked list algorithms, such as merge sort, and ensures efficient and accurate list manipulation even for large datasets

# CODE

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
};

class StockStack {
private:
    Node* top;

public:
    StockStack() {
        top = NULL;
    }

    void record(int price) {
        Node* temp = new Node();
        temp->data = price;
        temp->next = top;
        top = temp;
        cout << "Recorded price: " << price << endl;
    }

    int removePrice() {
        if (top == NULL) {
            cout << "No prices to remove.\n";
            return -1;
        }

        Node* temp = top;
        int value = temp->data;
        top = top->next;
        delete temp;

        return value;
    }
    int latest() {
        if (top == NULL) {
            cout << "No prices recorded.\n";
            return -1;
        }
        return top->data;
    }

    bool isEmpty() {
        return (top == NULL);
    }

    void display() {
        Node* temp = top;
        cout << "Prices (latest first): ";
        while (temp) {
            cout << temp->data << " ";
            temp = temp->next;
        }
        cout << endl;
    }
};


int main() {
    StockStack s;
    int choice, price;

    do {
        cout << "\n1. Record Price\n2. Remove Latest Price\n3. Latest Price\n4. Check Empty\n5. Display Stack\n6. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            cout << "Enter price: ";
            cin >> price;
            s.record(price);
            break;

        case 2:
            price = s.removePrice();
            if (price != -1)
                cout << "Removed price: " << price << endl;
            break;

        case 3:
            price = s.latest();
            if (price != -1)
                cout << "Latest price: " << price << endl;
            break;

        case 4:
            if (s.isEmpty())
                cout << "Stack is empty.\n";
            else
                cout << "Stack has prices.\n";
            break;

        case 5:
            s.display();
            break;
        }
    } while (choice != 6);

    return 0;
}
```
# Output 

1. Record Price
2. Remove Latest Price
3. Latest Price
4. Check Empty
5. Display Stack
6. Exit
Enter choice: 1
Enter price: 450
Recorded price: 450

1. Record Price
2. Remove Latest Price
3. Latest Price
4. Check Empty
5. Display Stack
6. Exit
Enter choice: 1
Enter price: 470
Recorded price: 470

1. Record Price
2. Remove Latest Price
3. Latest Price
4. Check Empty
5. Display Stack
6. Exit
Enter choice: 1
Enter price: 460
Recorded price: 460

1. Record Price
2. Remove Latest Price
3. Latest Price
4. Check Empty
5. Display Stack
6. Exit
Enter choice: 5
Prices (latest first): 460 470 450

1. Record Price
2. Remove Latest Price
3. Latest Price
4. Check Empty
5. Display Stack
6. Exit
Enter choice: 3
Latest price: 460

1. Record Price
2. Remove Latest Price
3. Latest Price
4. Check Empty
5. Display Stack
6. Exit
Enter choice: 2
Removed price: 460

1. Record Price
2. Remove Latest Price
3. Latest Price
4. Check Empty
5. Display Stack
6. Exit
Enter choice: 5
Prices (latest first): 470 450

1. Record Price
2. Remove Latest Price
3. Latest Price
4. Check Empty
5. Display Stack
6. Exit
Enter choice: 4
Stack has prices.

1. Record Price
2. Remove Latest Price
3. Latest Price
4. Check Empty
5. Display Stack
6. Exit
Enter choice: 2
Removed price: 470

1. Record Price
2. Remove Latest Price
3. Latest Price
4. Check Empty
5. Display Stack
6. Exit
Enter choice: 2
Removed price: 450

1. Record Price
2. Remove Latest Price
3. Latest Price
4. Check Empty
5. Display Stack
6. Exit
Enter choice: 4
Stack is empty.

1. Record Price
2. Remove Latest Price
3. Latest Price
4. Check Empty
5. Display Stack
6. Exit
Enter choice: 6
