
# Assignment NO 8.1

# Title : Write a program, using trees, to assign the roll nos. to the students of your class as per their previous years result. i.e topper will be roll no. 1

# Theory
This problem uses a tree data structure to assign roll numbers to students based on their previous year results, where the topper gets roll number 1, the next highest scorer gets roll number 2, and so on.

A Binary Search Tree (BST) is suitable for this task because it stores data in a sorted order automatically. Each student’s marks are used as the key while inserting records into the tree. Higher marks indicate better performance, so the tree organizes students according to their results.

After constructing the tree, an inorder traversal is performed to obtain the students in sorted order of marks. By traversing the tree in descending order (right subtree first), students are ranked from highest to lowest marks. Roll numbers are then assigned sequentially starting from 1.

Using a tree ensures efficient insertion and automatic sorting without using extra sorting algorithms. This approach reflects real-life academic ranking systems where students are assigned roll numbers based on merit.

Thus, implementing this problem using trees provides an organized, efficient, and scalable way to assign roll numbers fairly based on students’ performance.

# CODE
```cpp
#include <iostream>
using namespace std;

struct Node {
    string name;
    int marks;
    int roll;
    Node* left;
    Node* right;
};

Node* createNode(string name, int marks) {
    Node* n = new Node();
    n->name = name;
    n->marks = marks;
    n->roll = 0;
    n->left = n->right = NULL;
    return n;
}

Node* insertNode(Node* root, string name, int marks) {
    if (root == NULL)
        return createNode(name, marks);

    if (marks < root->marks)
        root->left = insertNode(root->left, name, marks);
    else
        root->right = insertNode(root->right, name, marks);

    return root;
}

void assignRoll(Node* root, int &roll) {
    if (root == NULL) return;

    assignRoll(root->right, roll);  
    root->roll = roll++;
    assignRoll(root->left, roll);
}

void display(Node* root) {
    if (root == NULL) return;

    display(root->right);
    cout << "Roll No: " << root->roll
         << " | Name: " << root->name
         << " | Marks: " << root->marks << endl;
    display(root->left);
}

int main() {
    Node* root = NULL;
    int choice, marks;
    string name;

    do {
        cout << "\n1. Add Student"
             << "\n2. Assign Roll Numbers"
             << "\n3. Display Students"
             << "\n4. Exit"
             << "\nEnter choice: ";
        cin >> choice;

        switch (choice) {
            case 1:
                cout << "Enter student name: ";
                cin >> name;
                cout << "Enter marks: ";
                cin >> marks;
                root = insertNode(root, name, marks);
                break;

            case 2: {
                int roll = 1;
                assignRoll(root, roll);
                cout << "Roll numbers assigned successfully!\n";
                break;
            }

            case 3:
                cout << "\nStudents (Highest marks first):\n";
                display(root);
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
1. Add Student
2. Assign Roll Numbers
3. Display Students
4. Exit
Enter choice: 1
Enter student name: Rahul
Enter marks: 85

1. Add Student
2. Assign Roll Numbers
3. Display Students
4. Exit
Enter choice: 1
Enter student name: Priya
Enter marks: 92

1. Add Student
2. Assign Roll Numbers
3. Display Students
4. Exit
Enter choice: 1
Enter student name: Amit
Enter marks: 78

1. Add Student
2. Assign Roll Numbers
3. Display Students
4. Exit
Enter choice: 1
Enter student name: Neha
Enter marks: 88

1. Add Student
2. Assign Roll Numbers
3. Display Students
4. Exit
Enter choice: 2
Roll numbers assigned successfully!

1. Add Student
2. Assign Roll Numbers
3. Display Students
4. Exit
Enter choice: 3

Students (Highest marks first):
Roll No: 1 | Name: Priya | Marks: 92
Roll No: 2 | Name: Neha  | Marks: 88
Roll No: 3 | Name: Rahul | Marks: 85
Roll No: 4 | Name: Amit  | Marks: 78

1. Add Student
2. Assign Roll Numbers
3. Display Students
4. Exit
Enter choice: 4
Exiting...
