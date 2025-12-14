# Assignment NO 7.5

# Include : Write a Program to create a Binary Tree and perform the following Recursive operations on it. a. Inorder Traversal b. Preorder Traversal c. Display Number of Leaf Nodes d. Mirror Image

# Theory
This problem focuses on creating a Binary Tree and performing important recursive operations on it.

A binary tree is a hierarchical data structure in which each node contains data and pointers to its left and right child. Recursive techniques are naturally suited for tree operations because a tree itself is defined recursively as smaller subtrees.

Inorder traversal is performed recursively by visiting the left subtree, then the root node, and finally the right subtree. Preorder traversal visits the root node first, followed by recursive traversal of the left and right subtrees. These traversals help in understanding the structure and ordering of the tree.

The number of leaf nodes is found recursively by checking nodes that have no left or right children. Each leaf node contributes one to the total count.
The mirror image operation is also performed recursively by swapping the left and right child of every node, producing a mirror reflection of the original tree.

Thus, recursive binary tree operations provide a clear, simple, and efficient way to traverse, analyze, and transform tree structures.

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

Node* createTree() {
    int val;
    cout << "Enter root value: ";
    cin >> val;

    Node* root = createNode(val);
    queue<Node*> q;
    q.push(root);

    while (!q.empty()) {
        Node* temp = q.front();
        q.pop();

        char ch;
        cout << "Add left child of " << temp->data << "? (y/n): ";
        cin >> ch;
        if (ch == 'y') {
            cout << "Enter value: ";
            cin >> val;
            temp->left = createNode(val);
            q.push(temp->left);
        }

        cout << "Add right child of " << temp->data << "? (y/n): ";
        cin >> ch;
        if (ch == 'y') {
            cout << "Enter value: ";
            cin >> val;
            temp->right = createNode(val);
            q.push(temp->right);
        }
    }
    return root;
}

void inorder(Node* root) {
    if (root == NULL) return;
    inorder(root->left);
    cout << root->data << " ";
    inorder(root->right);
}

void preorder(Node* root) {
    if (root == NULL) return;
    cout << root->data << " ";
    preorder(root->left);
    preorder(root->right);
}

int countLeaf(Node* root) {
    if (root == NULL) return 0;
    if (root->left == NULL && root->right == NULL)
        return 1;
    return countLeaf(root->left) + countLeaf(root->right);
}

void mirror(Node* root) {
    if (root == NULL) return;

    Node* temp = root->left;
    root->left = root->right;
    root->right = temp;

    mirror(root->left);
    mirror(root->right);
}

int main() {
    Node* root = NULL;
    int choice;

    do {
        cout << "\n1. Create Tree"
             << "\n2. Inorder Traversal"
             << "\n3. Preorder Traversal"
             << "\n4. Count Leaf Nodes"
             << "\n5. Mirror Image"
             << "\n6. Exit"
             << "\nEnter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                root = createTree();
                break;

            case 2:
                cout << "Inorder: ";
                inorder(root);
                cout << endl;
                break;

            case 3:
                cout << "Preorder: ";
                preorder(root);
                cout << endl;
                break;

            case 4:
                cout << "Leaf Nodes = " << countLeaf(root) << endl;
                break;

            case 5:
                mirror(root);
                cout << "Mirror image created.\n";
                break;
        }
    } while (choice != 6);

    return 0;
}
```

# OUTPUT

1. Create Tree
2. Inorder Traversal
3. Preorder Traversal
4. Count Leaf Nodes
5. Mirror Image
6. Exit
Enter choice: 1
Enter root value: 10
Add left child of 10? (y/n): y
Enter value: 20
Add right child of 10? (y/n): y
Enter value: 30
Add left child of 20? (y/n): y
Enter value: 40
Add right child of 20? (y/n): n
Add left child of 30? (y/n): n
Add right child of 30? (y/n): n
Add left child of 40? (y/n): n
Add right child of 40? (y/n): n

1. Create Tree
2. Inorder Traversal
3. Preorder Traversal
4. Count Leaf Nodes
5. Mirror Image
6. Exit
Enter choice: 2
Inorder: 40 20 10 30

1. Create Tree
2. Inorder Traversal
3. Preorder Traversal
4. Count Leaf Nodes
5. Mirror Image
6. Exit
Enter choice: 3
Preorder: 10 20 40 30

1. Create Tree
2. Inorder Traversal
3. Preorder Traversal
4. Count Leaf Nodes
5. Mirror Image
6. Exit
Enter choice: 4
Leaf Nodes = 2

1. Create Tree
2. Inorder Traversal
3. Preorder Traversal
4. Count Leaf Nodes
5. Mirror Image
6. Exit
Enter choice: 5
Mirror image created.

1. Create Tree
2. Inorder Traversal
3. Preorder Traversal
4. Count Leaf Nodes
5. Mirror Image
6. Exit
Enter choice: 2
Inorder: 30 10 20 40

1. Create Tree
2. Inorder Traversal
3. Preorder Traversal
4. Count Leaf Nodes
5. Mirror Image
6. Exit
Enter choice: 6
