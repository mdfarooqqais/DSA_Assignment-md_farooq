# Assignment NO 8.2

# Title : Write a program to illustrate operations on a BST holding numeric keys. The menu must include: • Insert • Delete • Find • Show

# Theory 
This problem demonstrates the implementation of basic operations on a Binary Search Tree (BST) with numeric keys using a menu-driven approach.

A Binary Search Tree is a hierarchical data structure in which each node contains a numeric key and two child pointers. The BST property ensures that all keys in the left subtree are smaller than the node’s key, and all keys in the right subtree are larger, which makes operations efficient.

The Insert operation adds a new key into the BST while maintaining the BST property by placing it at the correct position. The Delete operation removes a specified key from the tree and restructures it based on whether the node has zero, one, or two children. The Find operation searches for a given numeric key by comparing values and traversing left or right accordingly.

The Show operation displays the contents of the BST, usually using an inorder traversal, which prints the keys in sorted order. This helps in verifying the structure of the tree and the correctness of operations.

Thus, a menu-driven BST program provides a clear understanding of how dynamic insertion, deletion, searching, and display of numeric data can be efficiently handled using tree data structures.

# CODE
```cpp
#include <iostream>
using namespace std;

struct Node {
    int data;
    Node* left;
    Node* right;
};

Node* createNode(int key) {
    Node* n = new Node();
    n->data = key;
    n->left = n->right = NULL;
    return n;
}

Node* insertNode(Node* root, int key) {
    if (root == NULL)
        return createNode(key);

    if (key < root->data)
        root->left = insertNode(root->left, key);
    else if (key > root->data)
        root->right = insertNode(root->right, key);

    return root;
}

bool searchNode(Node* root, int key) {
    if (root == NULL) return false;
    if (root->data == key) return true;

    if (key < root->data)
        return searchNode(root->left, key);
    else
        return searchNode(root->right, key);
}

Node* minValueNode(Node* root) {
    while (root->left != NULL)
        root = root->left;
    return root;
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

void inorder(Node* root) {
    if (root != NULL) {
        inorder(root->left);
        cout << root->data << " ";
        inorder(root->right);
    }
}

int main() {
    Node* root = NULL;
    int choice, key;

    do {
        cout << "\n1. Insert\n2. Delete\n3. Find\n4. Show\n5. Exit\nEnter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter value to insert: ";
                cin >> key;
                root = insertNode(root, key);
                break;

            case 2:
                cout << "Enter value to delete: ";
                cin >> key;
                root = deleteNode(root, key);
                break;

            case 3:
                cout << "Enter value to search: ";
                cin >> key;
                if (searchNode(root, key))
                    cout << "Key found!\n";
                else
                    cout << "Key NOT found!\n";
                break;

            case 4:
                cout << "BST (Inorder): ";
                inorder(root);
                cout << endl;
                break;
        }

    } while (choice != 5);

    return 0;
}
```

# OUTPUT 

1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter choice: 1
Enter value to insert: 50

1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter choice: 1
Enter value to insert: 30

1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter choice: 1
Enter value to insert: 70

1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter choice: 1
Enter value to insert: 20

1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter choice: 1
Enter value to insert: 40

1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter choice: 4
BST (Inorder): 20 30 40 50 70

1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter choice: 3
Enter value to search: 40
Key found!

1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter choice: 3
Enter value to search: 90
Key NOT found!

1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter choice: 2
Enter value to delete: 30

1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter choice: 4
BST (Inorder): 20 40 50 70

1. Insert
2. Delete
3. Find
4. Show
5. Exit
Enter choice: 5
