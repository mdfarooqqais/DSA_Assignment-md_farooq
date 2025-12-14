# Assignment NO 7.3

# Title : Write a Program to create a Binary Tree Search and Find Minimum/Maximum in BST 

# THEORY
This problem is based on creating a Binary Search Tree (BST) and finding the minimum and maximum values stored in it.

A Binary Search Tree is a hierarchical data structure where each node contains a key and two child pointers. The BST property ensures that all values in the left subtree are smaller than the root node, while all values in the right subtree are greater than the root node. This property helps in efficient searching and retrieval of data.

To find the minimum value in a BST, traversal is performed repeatedly towards the leftmost node, since the smallest element always lies on the extreme left. Similarly, the maximum value is found by traversing towards the rightmost node, as the largest element lies on the extreme right of the tree.

Creating a BST involves inserting elements one by one while maintaining the BST property. Once the tree is constructed, finding the minimum and maximum values becomes efficient and requires less time compared to linear data structures.

Thus, using a BST provides an organized way to store data and allows quick access to the smallest and largest elements in the dataset.
# CODE

```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
};

Node* createNode(int val) {
    Node* n = new Node();
    n->data = val;
    n->left = n->right = NULL;
    return n;
}

Node* insertNode(Node* root, int val) {
    if (root == NULL)
        return createNode(val);

    if (val < root->data)
        root->left = insertNode(root->left, val);
    else if (val > root->data)
        root->right = insertNode(root->right, val);

    return root;
}

bool searchBST(Node* root, int key) {
    if (root == NULL) return false;

    if (key == root->data) return true;

    if (key < root->data)
        return searchBST(root->left, key);
    else
        return searchBST(root->right, key);
}

int findMin(Node* root) {
    if (root == NULL) {
        cout << "Tree is empty!\n";
        return -1;
    }
    Node* temp = root;
    while (temp->left != NULL)
        temp = temp->left;
    return temp->data;
}

int findMax(Node* root) {
    if (root == NULL) {
        cout << "Tree is empty!\n";
        return -1;
    }
    Node* temp = root;
    while (temp->right != NULL)
        temp = temp->right;
    return temp->data;
}

int main() {
    Node* root = NULL;
    int choice, val;

    do {
        cout << "\n1. Insert Node\n2. Search\n3. Find Minimum\n4. Find Maximum\n5. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter value to insert: ";
                cin >> val;
                root = insertNode(root, val);
                break;

            case 2:
                cout << "Enter value to search: ";
                cin >> val;
                if (searchBST(root, val))
                    cout << "Value found!\n";
                else
                    cout << "Value NOT found!\n";
                break;

            case 3:
                cout << "Minimum Value = " << findMin(root) << endl;
                break;

            case 4:
                cout << "Maximum Value = " << findMax(root) << endl;
                break;

            case 5:
                cout << "Exiting...\n";
                break;
        }

    } while (choice != 5);

    return 0;
}
```
# OUTPUT

1. Insert Node
2. Search
3. Find Minimum
4. Find Maximum
5. Exit
Enter choice: 1
Enter value to insert: 50

1. Insert Node
2. Search
3. Find Minimum
4. Find Maximum
5. Exit
Enter choice: 1
Enter value to insert: 30

1. Insert Node
2. Search
3. Find Minimum
4. Find Maximum
5. Exit
Enter choice: 1
Enter value to insert: 70

1. Insert Node
2. Search
3. Find Minimum
4. Find Maximum
5. Exit
Enter choice: 1
Enter value to insert: 20

1. Insert Node
2. Search
3. Find Minimum
4. Find Maximum
5. Exit
Enter choice: 1
Enter value to insert: 40

1. Insert Node
2. Search
3. Find Minimum
4. Find Maximum
5. Exit
Enter choice: 1
Enter value to insert: 60

1. Insert Node
2. Search
3. Find Minimum
4. Find Maximum
5. Exit
Enter choice: 1
Enter value to insert: 80

1. Insert Node
2. Search
3. Find Minimum
4. Find Maximum
5. Exit
Enter choice: 2
Enter value to search: 40
Value found!

1. Insert Node
2. Search
3. Find Minimum
4. Find Maximum
5. Exit
Enter choice: 2
Enter value to search: 90
Value NOT found!

1. Insert Node
2. Search
3. Find Minimum
4. Find Maximum
5. Exit
Enter choice: 3
Minimum Value = 20

1. Insert Node
2. Search
3. Find Minimum
4. Find Maximum
5. Exit
Enter choice: 4
Maximum Value = 80

1. Insert Node
2. Search
3. Find Minimum
4. Find Maximum
5. Exit
Enter choice: 5
Exiting...


