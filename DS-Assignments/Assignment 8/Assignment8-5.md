# Assignment NO 8.5

Write a program to implement deletion operations in the product inventory system using a search tree.
 Each product must store the following information:
●	Unique Product Code
●	Product Name
●	Price
●	Quantity in Stock
●	Date Received
●	Expiration Date
Implement the following operations:
1.	Delete a product using its unique product code.

2.	Delete all expired products based on the current date.

# Algorithm 

This problem focuses on managing deletion operations in a product inventory system using a search tree, typically a Binary Search Tree (BST).

In this system, each product is stored as a node in the tree containing details such as unique product code, product name, price, quantity, date received, and expiration date. The search tree structure allows efficient searching and deletion of product records.

The first operation deletes a specific product using its unique product code. The tree is traversed to locate the node with the matching code, and deletion is performed based on standard BST deletion cases: deleting a leaf node, a node with one child, or a node with two children while maintaining the tree structure.

The second operation deletes all expired products by comparing each product’s expiration date with the current date. A traversal of the tree is performed, and nodes representing expired products are removed safely without breaking the search tree property.

Using a search tree ensures that deletion operations are efficient and scalable, even when the inventory grows large. This approach helps maintain an up-to-date inventory by removing outdated or unwanted products automatically and accurately.

# CODE

```cpp
#include <iostream>
#include <string>
using namespace std;

struct Node {
    string code;
    string name;
    float price;
    int quantity;
    string dateReceived;
    string expDate;
    Node* left;
    Node* right;
};

Node* createNode(string c, string n, float p, int q, string dr, string ed) {
    Node* node = new Node();
    node->code = c;
    node->name = n;
    node->price = p;
    node->quantity = q;
    node->dateReceived = dr;
    node->expDate = ed;
    node->left = node->right = NULL;
    return node;
}

Node* insertProduct(Node* root, string c, string n, float p, int q, string dr, string ed) {
    if (root == NULL)
        return createNode(c, n, p, q, dr, ed);

    if (c < root->code)
        root->left = insertProduct(root->left, c, n, p, q, dr, ed);
    else if (c > root->code)
        root->right = insertProduct(root->right, c, n, p, q, dr, ed);

    return root;
}

Node* minValueNode(Node* root) {
    while (root->left != NULL)
        root = root->left;
    return root;
}

Node* deleteProduct(Node* root, string c) {
    if (root == NULL)
        return NULL;

    if (c < root->code)
        root->left = deleteProduct(root->left, c);

    else if (c > root->code)
        root->right = deleteProduct(root->right, c);

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
        root->code = temp->code;
        root->name = temp->name;
        root->price = temp->price;
        root->quantity = temp->quantity;
        root->dateReceived = temp->dateReceived;
        root->expDate = temp->expDate;

        root->right = deleteProduct(root->right, temp->code);
    }

    return root;
}

bool isExpired(string expDate, string currDate) {
    return expDate < currDate; 
}

Node* deleteExpired(Node* root, string currDate) {
    if (root == NULL) return NULL;

    root->left = deleteExpired(root->left, currDate);
    root->right = deleteExpired(root->right, currDate);

    if (isExpired(root->expDate, currDate))
        return deleteProduct(root, root->code);

    return root;
}

void inorder(Node* root) {
    if (root != NULL) {
        inorder(root->left);
        cout << "\nCode: " << root->code
             << " | Name: " << root->name
             << " | Price: " << root->price
             << " | Qty: " << root->quantity
             << " | Received: " << root->dateReceived
             << " | Expires: " << root->expDate << endl;
        inorder(root->right);
    }
}

int main() {
    Node* root = NULL;
    int choice;
    string code, name, dr, ed, currDate;
    float price;
    int qty;

    do {
        cout << "\n1. Insert Product"
             << "\n2. Delete Product by Code"
             << "\n3. Delete All Expired Products"
             << "\n4. Display Inventory (Inorder)"
             << "\n5. Exit"
             << "\nEnter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Code: "; cin >> code;
                cout << "Name: "; cin >> name;
                cout << "Price: "; cin >> price;
                cout << "Quantity: "; cin >> qty;
                cout << "Date Received (YYYY-MM-DD): "; cin >> dr;
                cout << "Expiration Date (YYYY-MM-DD): "; cin >> ed;

                root = insertProduct(root, code, name, price, qty, dr, ed);
                break;

            case 2:
                cout << "Enter product code to delete: ";
                cin >> code;
                root = deleteProduct(root, code);
                break;

            case 3:
                cout << "Enter today's date (YYYY-MM-DD): ";
                cin >> currDate;
                root = deleteExpired(root, currDate);
                break;

            case 4:
                cout << "\n--- Inventory (Sorted by Code) ---\n";
                inorder(root);
                break;
        }

    } while (choice != 5);

    return 0;
}
```
# OUTPUT
1. Insert Product
2. Delete Product by Code
3. Delete All Expired Products
4. Display Inventory (Inorder)
5. Exit
Enter choice: 1
Code: P101
Name: Biscuit
Price: 20
Quantity: 50
Date Received (YYYY-MM-DD): 2024-12-10
Expiration Date (YYYY-MM-DD): 2024-12-25

1. Insert Product
2. Delete Product by Code
3. Delete All Expired Products
4. Display Inventory (Inorder)
5. Exit
Enter choice: 1
Code: P102
Name: Milk
Price: 30
Quantity: 20
Date Received (YYYY-MM-DD): 2024-12-30
Expiration Date (YYYY-MM-DD): 2025-01-05

1. Insert Product
2. Delete Product by Code
3. Delete All Expired Products
4. Display Inventory (Inorder)
5. Exit
Enter choice: 1
Code: P103
Name: Chips
Price: 25
Quantity: 40
Date Received (YYYY-MM-DD): 2024-11-15
Expiration Date (YYYY-MM-DD): 2024-12-01

1. Insert Product
2. Delete Product by Code
3. Delete All Expired Products
4. Display Inventory (Inorder)
5. Exit
Enter choice: 4

--- Inventory (Sorted by Code) ---

Code: P101 | Name: Biscuit | Price: 20 | Qty: 50 | Received: 2024-12-10 | Expires: 2024-12-25
Code: P102 | Name: Milk    | Price: 30 | Qty: 20 | Received: 2024-12-30 | Expires: 2025-01-05
Code: P103 | Name: Chips   | Price: 25 | Qty: 40 | Received: 2024-11-15 | Expires: 2024-12-01

1. Insert Product
2. Delete Product by Code
3. Delete All Expired Products
4. Display Inventory (Inorder)
5. Exit
Enter choice: 2
Enter product code to delete: P102

1. Insert Product
2. Delete Product by Code
3. Delete All Expired Products
4. Display Inventory (Inorder)
5. Exit
Enter choice: 4

--- Inventory (Sorted by Code) ---

Code: P101 | Name: Biscuit | Price: 20 | Qty:
