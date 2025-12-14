# Assignment NO 7.2

# Title : Write a program to perform Binary Search Tree (BST) operations (Count the total number of nodes, Compute the height of the BST, Mirror Image ).

# Theory
This problem focuses on performing advanced operations on a Binary Search Tree (BST) such as counting nodes, computing height, and creating a mirror image of the tree.

A Binary Search Tree is a hierarchical data structure in which each node contains a data value and two child pointers. The BST property ensures that all values in the left subtree are smaller than the root, and all values in the right subtree are larger than the root.

The count operation calculates the total number of nodes present in the BST by recursively visiting each node. The height of the BST is computed as the maximum number of levels from the root to the deepest leaf node, which reflects the depth of the tree.

The mirror image operation transforms the BST by swapping the left and right child pointers of every node. This operation is performed recursively and results in a tree that is the mirror reflection of the original BST.

These operations help in analyzing the structure of a BST and are useful in applications such as tree balancing, structural analysis, and visualization of hierarchical data.

# CODE
```cpp
#include <iostream>
#include <algorithm>
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

int countNodes(Node* root) {
    if (root == NULL)
        return 0;
    return countNodes(root->left) + countNodes(root->right) + 1;
}

int height(Node* root) {
    if (root == NULL)
        return 0;
    return max(height(root->left), height(root->right)) + 1;
}

void mirrorImage(Node* root) {
    if (root == NULL)
        return;

    Node* temp = root->left;
    root->left = root->right;
    root->right = temp;

    mirrorImage(root->left);
    mirrorImage(root->right);
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
    int choice, val;

    do {
        cout << "\n1. Insert Node\n2. Count Nodes\n3. Compute Height\n4. Mirror Image\n5. Display (Inorder)\n6. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter value: ";
                cin >> val;
                root = insertNode(root, val);
                break;

            case 2:
                cout << "Total Nodes = " << countNodes(root) << endl;
                break;

            case 3:
                cout << "Height of BST = " << height(root) << endl;
                break;

            case 4:
                mirrorImage(root);
                cout << "Mirror image created.\n";
                break;

            case 5:
                cout << "Inorder Traversal: ";
                inorder(root);
                cout << endl;
                break;

            case 6:
                cout << "Exiting...\n";
        }
    } while (choice != 6);

    return 0;
}
```

# OUTPUT

1. Insert Node
2. Count Nodes
3. Compute Height
4. Mirror Image
5. Display (Inorder)
6. Exit
Enter choice: 1
Enter value: 50

1. Insert Node
2. Count Nodes
3. Compute Height
4. Mirror Image
5. Display (Inorder)
6. Exit
Enter choice: 1
Enter value: 30

1. Insert Node
2. Count Nodes
3. Compute Height
4. Mirror Image
5. Display (Inorder)
6. Exit
Enter choice: 1
Enter value: 70

1. Insert Node
2. Count Nodes
3. Compute Height
4. Mirror Image
5. Display (Inorder)
6. Exit
Enter choice: 1
Enter value: 20

1. Insert Node
2. Count Nodes
3. Compute Height
4. Mirror Image
5. Display (Inorder)
6. Exit
Enter choice: 1
Enter value: 40

1. Insert Node
2. Count Nodes
3. Compute Height
4. Mirror Image
5. Display (Inorder)
6. Exit
Enter choice: 1
Enter value: 60

1. Insert Node
2. Count Nodes
3. Compute Height
4. Mirror Image
5. Display (Inorder)
6. Exit
Enter choice: 1
Enter value: 80

1. Insert Node
2. Count Nodes
3. Compute Height
4. Mirror Image
5. Display (Inorder)
6. Exit
Enter choice: 5
Inorder Traversal: 20 30 40 50 60 70 80

1. Insert Node
2. Count Nodes
3. Compute Height
4. Mirror Image
5. Display (Inorder)
6. Exit
Enter choice: 2
Total Nodes = 7

1. Insert Node
2. Count Nodes
3. Compute Height
4. Mirror Image
5. Display (Inorder)
6. Exit
Enter choice: 3
Height of BST = 3

1. Insert Node
2. Count Nodes
3. Compute Height
4. Mirror Image
5. Display (Inorder)
6. Exit
Enter choice: 4
Mirror image created.

1. Insert Node
2. Count Nodes
3. Compute Height
4. Mirror Image
5. Display (Inorder)
6. Exit
Enter choice: 5
Inorder Traversal: 80 70 60 50 40 30 20

1. Insert Node
2. Count Nodes
3. Compute Height
4. Mirror Image
5. Display (Inorder)
6. Exit
Enter choice: 6
Exiting...
