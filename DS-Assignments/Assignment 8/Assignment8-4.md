# Assignment NO 8.4

Write a program to implement a product inventory management system for a shop using a search tree data structure. Each product must store the following information:
●	Unique Product Code
●	Product Name
●	Price
●	Quantity in Stock
●	Date Received
●	Expiration Date
Implement the following operations:
1.	Insert a product into the tree ( organized by product name).
2.	Display all items in the inventory using inorder traversal.
3.	List expired items in prefix (preorder) order of their names.

# Algorithm 

This problem focuses on designing a product inventory management system using a search tree data structure, specifically a Binary Search Tree (BST).

In this system, each product is stored as a node in the tree containing details such as unique product code, product name, price, quantity, date received, and expiration date. The tree is organized using the product name as the key, which allows products to be stored in alphabetical order automatically.

The insert operation places a new product into the BST at the correct position based on its product name, maintaining the search tree property. The inorder traversal is used to display all inventory items in sorted order of product names, making it easy to view the complete inventory systematically.

To identify expired products, a preorder traversal (prefix order) is performed. During traversal, each product’s expiration date is checked against the current date, and expired items are listed accordingly. Preorder traversal ensures that parent nodes are processed before their children.

Using a search tree provides efficient insertion, organized storage, and quick traversal-based operations, making it suitable for managing dynamic product inventories in shops.

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

    if (n < root->name)
        root->left = insertProduct(root->left, c, n, p, q, dr, ed);
    else if (n > root->name)
        root->right = insertProduct(root->right, c, n, p, q, dr, ed);

    return root;
}

void inorder(Node* root) {
    if (root != NULL) {
        inorder(root->left);
        cout << "\nProduct Name: " << root->name
             << "\nCode: " << root->code
             << "\nPrice: " << root->price
             << "\nQuantity: " << root->quantity
             << "\nReceived: " << root->dateReceived
             << "\nExpires: " << root->expDate << "\n";
        inorder(root->right);
    }
}

bool isExpired(string exp) {
    string today = "2025-01-01";  
    return exp < today;
}

void listExpired(Node* root) {
    if (root == NULL)
        return;

    if (isExpired(root->expDate)) {
        cout << "\nEXPIRED PRODUCT → " << root->name
             << " | Code: " << root->code
             << " | Expired on: " << root->expDate << "\n";
    }

    listExpired(root->left);
    listExpired(root->right);
}

int main() {
    Node* root = NULL;
    int choice;
    string code, name, dr, ed;
    float price;
    int qty;

    do {
        cout << "\n1. Insert Product"
             << "\n2. Display All Products (Inorder)"
             << "\n3. List Expired Items (Preorder)"
             << "\n4. Exit"
             << "\nEnter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter Product Code: ";
                cin >> code;

                cout << "Enter Product Name: ";
                cin >> name;

                cout << "Enter Price: ";
                cin >> price;

                cout << "Enter Quantity: ";
                cin >> qty;

                cout << "Enter Date Received (YYYY-MM-DD): ";
                cin >> dr;

                cout << "Enter Expiration Date (YYYY-MM-DD): ";
                cin >> ed;

                root = insertProduct(root, code, name, price, qty, dr, ed);
                break;

            case 2:
                cout << "\n--- Inventory (Sorted by Product Name) ---\n";
                inorder(root);
                break;

            case 3:
                cout << "\n--- Expired Products ---\n";
                listExpired(root);
                break;
        }
    } while (choice != 4);

    return 0;
}
```
# CODE

1. Insert Product
2. Display All Products (Inorder)
3. List Expired Items (Preorder)
4. Exit
Enter choice: 1
Enter Product Code: P101
Enter Product Name: Biscuit
Enter Price: 20
Enter Quantity: 50
Enter Date Received (YYYY-MM-DD): 2024-12-10
Enter Expiration Date (YYYY-MM-DD): 2024-12-25

1. Insert Product
2. Display All Products (Inorder)
3. List Expired Items (Preorder)
4. Exit
Enter choice: 1
Enter Product Code: P102
Enter Product Name: Milk
Enter Price: 30
Enter Quantity: 20
Enter Date Received (YYYY-MM-DD): 2024-12-30
Enter Expiration Date (YYYY-MM-DD): 2025-01-05

1. Insert Product
2. Display All Products (Inorder)
3. List Expired Items (Preorder)
4. Exit
Enter choice: 1
Enter Product Code: P103
Enter Product Name: Chips
Enter Price: 25
Enter Quantity: 40
Enter Date Received (YYYY-MM-DD): 2024-11-15
Enter Expiration Date (YYYY-MM-DD): 2024-12-01

1. Insert Product
2. Display All Products (Inorder)
3. List Expired Items (Preorder)
4. Exit
Enter choice: 2

--- Inventory (Sorted by Product Name) ---

Product Name: Biscuit
Code: P101
Price: 20
Quantity: 50
Received: 2024-12-10
Expires: 2024-12-25

Product Name: Chips
Code: P103
Price: 25
Quantity: 40
Received: 2024-11-15
Expires: 2024-12-01

Product Name: Milk
Code: P102
Price: 30
Quantity: 20
Received: 2024-12-30
Expires: 2025-01-05

1. Insert Product
2. Display All Products (Inorder)
3. List Expired Items (Preorder)
4. Exit
Enter choice: 3

--- Expired Products ---

EXPIRED PRODUCT → Biscuit | Code: P101 | Expired on: 2024-12-25
EXPIRED PRODUCT → Chips | Code: P103 | Expired on: 2024-12-01

1. Insert Product
2. Display All Products (Inorder)
3. List Expired Items (Preorder)
4. Exit
Enter choice: 4
