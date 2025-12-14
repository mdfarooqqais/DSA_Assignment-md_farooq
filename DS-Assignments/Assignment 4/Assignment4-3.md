 # Assignment NO 4.3
 
 # Title : Implement Bubble sort using Doubly Linked List

# Theory 

Bubble Sort using a Doubly Linked List is a sorting technique where the elements of the list are arranged by repeatedly comparing adjacent nodes and swapping their data if they are in the wrong order.

A doubly linked list consists of nodes that contain data, a pointer to the previous node, and a pointer to the next node. This structure allows traversal in both forward and backward directions, which makes swapping and movement between known nodes easier compared to a singly linked list.

In Bubble Sort, the list is traversed multiple times. During each pass, the data of two adjacent nodes is compared. If the first node’s value is greater than the next node’s value, their data is swapped. This process continues until the largest element moves to the end of the list. The passes are repeated until no more swaps are required, indicating that the list is sorted.

Using Bubble Sort on a doubly linked list does not require shifting elements as in arrays; instead, values are swapped between nodes. Although Bubble Sort is simple to implement, it is not very efficient for large lists because it has a time complexity of O(n²).

Thus, Bubble Sort using a doubly linked list is mainly used for learning purposes and for sorting small datasets where simplicity is more important than performance.

# CODE

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
    Node* prev;
};

Node* insertEnd(Node* head, int val) {
    Node* newNode = new Node{val, NULL, NULL};

    if (!head)
        return newNode;

    Node* temp = head;
    while (temp->next)
        temp = temp->next;

    temp->next = newNode;
    newNode->prev = temp;

    return head;
}

void display(Node* head) {
    Node* temp = head;
    while (temp) {
        cout << temp->data << " ";
        temp = temp->next;
    }
    cout << endl;
}

void bubbleSort(Node* head) {
    if (!head) return;

    bool swapped;
    Node* ptr;

    do {
        swapped = false;
        ptr = head;

        while (ptr->next != NULL) {
            if (ptr->data > ptr->next->data) {
                swap(ptr->data, ptr->next->data);
                swapped = true;
            }
            ptr = ptr->next;
        }

    } while (swapped);
}

int main() {
    Node* head = NULL;
    int n, x;

    cout << "Enter number of elements: ";
    cin >> n;

    cout << "Enter elements:\n";
    for (int i = 0; i < n; i++) {
        cin >> x;
        head = insertEnd(head, x);
    }

    cout << "\nBefore Sorting: ";
    display(head);

    bubbleSort(head);

    cout << "After Bubble Sort: ";
    display(head);

    return 0;
}
```

# Dry Run 

# Input 

45 20 35 10

# OUTPUT

10 20 35 45