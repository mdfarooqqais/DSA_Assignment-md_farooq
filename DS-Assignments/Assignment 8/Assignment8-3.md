# Assignment NO 8.3

# Title : Write a program to efficiently search a particular employee record by using Tree data structure. Also sort the data on emp­id in ascending order.

# Theory 
This problem focuses on efficiently storing, searching, and sorting employee records using a tree data structure, specifically a Binary Search Tree (BST).

In this approach, each employee record is stored as a node in the tree, with employee ID (emp-id) used as the key. The BST property ensures that all employee IDs in the left subtree are smaller than the root node’s ID, while those in the right subtree are larger. This structure allows fast searching of a particular employee record by comparing the emp-id and traversing left or right accordingly.

Searching an employee record becomes efficient because the BST reduces unnecessary comparisons compared to linear searching. The time complexity depends on the height of the tree, making it faster than array-based search for large datasets.

To sort the employee records in ascending order of emp-id, an inorder traversal of the BST is performed. Inorder traversal naturally displays the nodes in sorted order due to the BST property.

Thus, using a tree data structure provides an organized, efficient, and scalable solution for managing employee records, supporting both quick search operations and automatic sorting without additional algorithms.

# CODE
```cpp
#include <iostream>
using namespace std;

struct Node {
    int empid;
    string name;
    float salary;
    Node* left;
    Node* right;
};

Node* createNode(int id, string name, float salary) {
    Node* n = new Node();
    n->empid = id;
    n->name = name;
    n->salary = salary;
    n->left = n->right = NULL;
    return n;
}

Node* insertNode(Node* root, int id, string name, float salary) {
    if (root == NULL)
        return createNode(id, name, salary);

    if (id < root->empid)
        root->left = insertNode(root->left, id, name, salary);
    else if (id > root->empid)
        root->right = insertNode(root->right, id, name, salary);

    return root;
}

Node* searchNode(Node* root, int id) {
    if (root == NULL)
        return NULL;

    if (id == root->empid)
        return root;

    if (id < root->empid)
        return searchNode(root->left, id);
    else
        return searchNode(root->right, id);
}

void inorder(Node* root) {
    if (root != NULL) {
        inorder(root->left);
        cout << "EmpID: " << root->empid
             << " | Name: " << root->name
             << " | Salary: " << root->salary << endl;
        inorder(root->right);
    }
}

int main() {
    Node* root = NULL;
    int choice, id;
    string name;
    float salary;

    do {
        cout << "\n1. Insert Employee"
             << "\n2. Search Employee"
             << "\n3. Display Sorted Employees"
             << "\n4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter Employee ID: ";
                cin >> id;
                cout << "Enter Name: ";
                cin >> name;
                cout << "Enter Salary: ";
                cin >> salary;
                root = insertNode(root, id, name, salary);
                break;

            case 2:
                cout << "Enter Employee ID to search: ";
                cin >> id;
                {
                    Node* result = searchNode(root, id);
                    if (result != NULL)
                        cout << "Found → EmpID: " << result->empid
                             << " | Name: " << result->name
                             << " | Salary: " << result->salary << endl;
                    else
                        cout << "Employee NOT found.\n";
                }
                break;

            case 3:
                cout << "\nEmployee Records in Ascending Order (empid):\n";
                inorder(root);
                break;
        }

    } while (choice != 4);

    return 0;
}
```
# OUTPUT 
1. Insert Employee
2. Search Employee
3. Display Sorted Employees
4. Exit
Enter your choice: 1
Enter Employee ID: 103
Enter Name: Rahul
Enter Salary: 45000

1. Insert Employee
2. Search Employee
3. Display Sorted Employees
4. Exit
Enter your choice: 1
Enter Employee ID: 101
Enter Name: Priya
Enter Salary: 60000

1. Insert Employee
2. Search Employee
3. Display Sorted Employees
4. Exit
Enter your choice: 1
Enter Employee ID: 108
Enter Name: Amit
Enter Salary: 48000

1. Insert Employee
2. Search Employee
3. Display Sorted Employees
4. Exit
Enter your choice: 1
Enter Employee ID: 105
Enter Name: Neha
Enter Salary: 52000

1. Insert Employee
2. Search Employee
3. Display Sorted Employees
4. Exit
Enter your choice: 3

Employee Records in Ascending Order (empid):
EmpID: 101 | Name: Priya | Salary: 60000
EmpID: 103 | Name: Rahul | Salary: 45000
EmpID: 105 | Name: Neha  | Salary: 52000
EmpID: 108 | Name: Amit  | Salary: 48000

1. Insert Employee
2. Search Employee
3. Display Sorted Employees
4. Exit
Enter your choice: 2
Enter Employee ID to search: 105
Found → EmpID: 105 | Name: Neha | Salary: 52000

1. Insert Employee
2. Search Employee
3. Display Sorted Employees
4. Exit
Enter your choice: 2
Enter Employee ID to search: 120
Employee NOT found.

1. Insert Employee
2. Search Employee
3. Display Sorted Employees
4. Exit
Enter your choice: 4
