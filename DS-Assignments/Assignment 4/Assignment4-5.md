# Assignment NO 4.5

# Title : Given a list, split it into two sublists — one for the front half, and one for the back half. If the number of elements is odd, the extra element should go in the front list. So FrontBackSplit() on the list {2, 3, 5, 7, 11} should yield the two lists {2, 3, 5} and {7, 11}. Getting this right for all the cases is harder than it looks. You should check your solution against a few cases (length = 2, length = 3, length=4) to make sure that the list gets split correctly near the shortlist boundary conditions. If it works right for length=4, it probably works right for length=1000. You will probably need special case code to deal with the (length <2) cases.


# Algorithm 

We use the slow-fast pointer technique

# FrontBackSplit(head, &front, &back)

Step 1: If head == NULL:
            front = NULL
            back  = NULL
            return

Step 2: If head->next == NULL:
            front = head
            back  = NULL
            return

Step 3: Initialize two pointers:
            slow = head
            fast = head->next

Step 4: While fast != NULL:
            Move fast = fast->next
            If fast != NULL:
                Move slow = slow->next
                Move fast = fast->next

Step 5: Now:
            - slow is at the midpoint (end of front list)
            - slow->next begins the back list

Step 6: front = head
Step 7: back  = slow->next
Step 8: Set slow->next = NULL   (split the list)

# CODE

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* next;
};

Node* insertEnd(Node* head, int val) {
    Node* n = new Node{val, NULL};
    if (!head) return n;

    Node* temp = head;
    while (temp->next) temp = temp->next;
    temp->next = n;
    return head;
}

void display(Node* head) {
    while (head) {
        cout << head->data << " ";
        head = head->next;
    }
    cout << endl;
}

void FrontBackSplit(Node* head, Node*& front, Node*& back) {
    if (head == NULL) {      
        front = back = NULL;
        return;
    }

    if (head->next == NULL) { 
        front = head;
        back = NULL;
        return;
    }

    Node* slow = head;
    Node* fast = head->next;

    while (fast != NULL) {
        fast = fast->next;
        if (fast != NULL) {
            slow = slow->next;
            fast = fast->next;
        }
    }

    front = head;
    back = slow->next;
    slow->next = NULL;
}

int main() {
    Node* head = NULL;
    Node* front = NULL;
    Node* back = NULL;

    int n, x;
    cout << "Enter number of elements: ";
    cin >> n;

    cout << "Enter elements: ";
    for (int i = 0; i < n; i++) {
        cin >> x;
        head = insertEnd(head, x);
    }

    cout << "\nOriginal list: ";
    display(head);

    FrontBackSplit(head, front, back);

    cout << "Front list: ";
    display(front);

    cout << "Back list: ";
    display(back);

    return 0;
}
```
# OUTPUT

# Input :

5
2 3 5 7 11

# Output :

Original list: 2 3 5 7 11
Front list: 2 3 5
Back list: 7 11
