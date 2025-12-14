# Assignment NO 7.4

# Title : Write a Program to create a Binary Tree and perform following Nonrecursive operations on it. a. Inorder Traversal b. Preorder Traversal c. Display Number of Leaf Nodes d. Mirror Image


# Algorithm 
This problem focuses on creating a Binary Tree and performing important non-recursive operations on it using iterative techniques.

A binary tree is a hierarchical data structure in which each node has at most two children, called the left and right child. Instead of recursion, non-recursive traversal uses an explicit stack to control the order of node processing, which helps avoid function call overhead and stack overflow issues.

Inorder traversal visits the left subtree, then the root, and finally the right subtree, while preorder traversal visits the root first, followed by the left and right subtrees. Both traversals are implemented using stacks to simulate recursive behavior.

The number of leaf nodes is determined by counting nodes that have no left or right child. The mirror image operation is performed by swapping the left and right child pointers of every node in the tree, producing a reversed structure.

Thus, non-recursive tree operations improve efficiency and provide better control over memory usage while performing traversal, analysis, and transformation of a binary tree.

# CODE
```cpp
#include <iostream>
#include <stack>
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

Node* insertNode() {
    int val;
    cout << "Enter node value: ";
    cin >> val;

    Node* root = createNode(val);

    queue<Node*> q;
    q.push(root);

    char ch;
    while (!q.empty()) {
        Node* temp = q.front();
        q.pop();

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

void inorderNR(Node* root) {
    stack<Node*> st;
    Node* curr = root;

    while (curr != NULL || !st.empty()) {
        while (curr != NULL) {
            st.push(curr);
            curr = curr->left;
        }

        curr = st.top();
        st.pop();
        cout << curr->data << " ";
        curr = curr->right;
    }
}

void preorderNR(Node* root) {
    if (root == NULL) return;

    stack<Node*> st;
    st.push(root);

    while (!st.empty()) {
        Node* temp = st.top();
        st.pop();

        cout << temp->data << " ";

        if (temp->right) st.push(temp->right);
        if (temp->left) st.push(temp->left);
    }
}

int countLeaf(Node* root) {
    if (root == NULL) return 0;

    queue<Node*> q;
    q.push(root);

    int count = 0;

    while (!q.empty()) {
        Node* temp = q.front();
        q.pop();

        if (temp->left == NULL && temp->right == NULL)
            count++;

        if (temp->left) q.push(temp->left);
        if (temp->right) q.push(temp->right);
    }
    return count;
}

void mirrorNR(Node* root) {
    if (root == NULL) return;

    queue<Node*> q;
    q.push(root);

    while (!q.empty()) {
        Node* temp = q.front();
        q.pop();

        Node* t = temp->left;
        temp->left = temp->right;
        temp->right = t;

        if (temp->left) q.push(temp->left);
        if (temp->right) q.push(temp->right);
    }
}

int main() {
    Node* root = NULL;
    int choice;

    do {
        cout << "\n1. Create Tree"
             << "\n2. Inorder (Nonrecursive)"
             << "\n3. Preorder (Nonrecursive)"
             << "\n4. Count Leaf Nodes"
             << "\n5. Mirror Image"
             << "\n6. Exit"
             << "\nEnter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                root = insertNode();
                break;
            case 2:
                cout << "Inorder: ";
                inorderNR(root);
                cout << endl;
                break;
            case 3:
                cout << "Preorder: ";
                preorderNR(root);
                cout << endl;
                break;
            case 4:
                cout << "Leaf Nodes = " << countLeaf(root) << endl;
                break;
            case 5:
                mirrorNR(root);
                cout << "Mirror image created.\n";
                break;
        }
    } while (choice != 6);

    return 0;
}
```

# OUTPUT

1. Create Tree
2. Inorder (Nonrecursive)
3. Preorder (Nonrecursive)
4. Count Leaf Nodes
5. Mirror Image
6. Exit
Enter choice: 1
Enter node value: 10
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
2. Inorder (Nonrecursive)
3. Preorder (Nonrecursive)
4. Count Leaf Nodes
5. Mirror Image
6. Exit
Enter choice: 2
Inorder: 40 20 10 30

1. Create Tree
2. Inorder (Nonrecursive)
3. Preorder (Nonrecursive)
4. Count Leaf Nodes
5. Mirror Image
6. Exit
Enter choice: 3
Preorder: 10 20 40 30

1. Create Tree
2. Inorder (Nonrecursive)
3. Preorder (Nonrecursive)
4. Count Leaf Nodes
5. Mirror Image
6. Exit
Enter choice: 4
Leaf Nodes = 2

1. Create Tree
2. Inorder (Nonrecursive)
3. Preorder (Nonrecursive)
4. Count Leaf Nodes
5. Mirror Image
6. Exit
Enter choice: 5
Mirror image created.

1. Create Tree
2. Inorder (Nonrecursive)
3. Preorder (Nonrecursive)
4. Count Leaf Nodes
5. Mirror Image
6. Exit
Enter choice: 2
Inorder: 30 10 20 40

1. Create Tree
2. Inorder (Nonrecursive)
3. Preorder (Nonrecursive)
4. Count Leaf Nodes
5. Mirror Image
6. Exit
Enter choice: 6
