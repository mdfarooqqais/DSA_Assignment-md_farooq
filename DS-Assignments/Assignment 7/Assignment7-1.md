# Assignment NO 7.1

# Title : Write a program to perform Binary Search Tree (BST) operations (Create, Insert, Delete,Levelwise display )

# Theory
This problem focuses on implementing and performing basic operations on a Binary Search Tree (BST).

A Binary Search Tree is a hierarchical data structure in which each node contains a data value and two child pointers, left and right. In a BST, the left subtree contains values smaller than the root, and the right subtree contains values greater than the root, which makes searching efficient.

The create operation initializes an empty tree or creates the root node. The insert operation adds new nodes to the tree while maintaining the BST property by placing elements in their correct positions. The delete operation removes a specified node from the tree and adjusts the structure based on whether the node has no child, one child, or two children.

The level-wise display (level order traversal) prints the nodes of the tree level by level, starting from the root, using a queue. This helps in understanding the structure of the tree clearly.

Thus, BST operations provide an efficient way to store, organize, and manage data dynamically with faster searching, insertion, and deletion compared to linear data structures.

# CODE

```cpp
#include <iostream>
#include <queue>
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

Node* minValueNode(Node* node) {
    Node* current = node;
    while (current && current->left != NULL)
        current = current->left;
    return current;
}

Node* deleteNode(Node* root, int key) {

    if (root == NULL) return root;

    if (key < root->data)
        root->left = deleteNode(root->left, key);

    else if (key > root->data)
        root->right = deleteNode(root->right, key);

    else {
        if (root->left == NULL && root->right == NULL) {
            delete root;
            return NULL;
        }

        else if (root->left == NULL) {
            Node* temp = root->right;
            delete root;
            return temp;
        }
        else if (root->right == NULL) {
            Node* temp = root->left;
            delete root;
            return temp;
        }

        Node* temp = minValueNode(root->right); 
        root->data = temp->data; 
        root->right = deleteNode(root->right, temp->data);
    }

    return root;
}

void levelOrder(Node* root) {
    if (root == NULL) {
        cout << "Tree is empty.\n";
        return;
    }

    queue<Node*> q;
    q.push(root);

    cout << "Level Order Traversal: ";

    while (!q.empty()) {
        Node* temp = q.front();
        q.pop();

        cout << temp->data << " ";

        if (temp->left)
            q.push(temp->left);
        if (temp->right)
            q.push(temp->right);
    }
    cout << endl;
}

int main() {
    Node* root = NULL;
    int choice, val;

    do {
        cout << "\n1. Insert\n2. Delete\n3. Levelwise Display\n4. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter value to insert: ";
                cin >> val;
                root = insertNode(root, val);
                break;

            case 2:
                cout << "Enter value to delete: ";
                cin >> val;
                root = deleteNode(root, val);
                break;

            case 3:
                levelOrder(root);
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

1. Insert
2. Delete
3. Levelwise Display
4. Exit
Enter choice: 1
Enter value to insert: 50

1. Insert
2. Delete
3. Levelwise Display
4. Exit
Enter choice: 1
Enter value to insert: 30

1. Insert
2. Delete
3. Levelwise Display
4. Exit
Enter choice: 1
Enter value to insert: 70

1. Insert
2. Delete
3. Levelwise Display
4. Exit
Enter choice: 1
Enter value to insert: 20

1. Insert
2. Delete
3. Levelwise Display
4. Exit
Enter choice: 1
Enter value to insert: 40

1. Insert
2. Delete
3. Levelwise Display
4. Exit
Enter choice: 1
Enter value to insert: 60

1. Insert
2. Delete
3. Levelwise Display
4. Exit
Enter choice: 1
Enter value to insert: 80

1. Insert
2. Delete
3. Levelwise Display
4. Exit
Enter choice: 3
Level Order Traversal: 50 30 70 20 40 60 80

1. Insert
2. Delete
3. Levelwise Display
4. Exit
Enter choice: 2
Enter value to delete: 30

1. Insert
2. Delete
3. Levelwise Display
4. Exit
Enter choice: 3
Level Order Traversal: 50 40 70 20 60 80

1. Insert
2. Delete
3. Levelwise Display
4. Exit
Enter choice: 4
Exiting...
