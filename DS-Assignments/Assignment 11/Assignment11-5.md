# Assignment 11.5

# Title : Store and retrieve student (faculty) records using roll numbers (IDs) using hashing with linear probing.

# Thoery :

A hash table is a data structure that allows fast insertion, searching, and retrieval of records using a unique key. In this assignment, faculty records are stored using a unique faculty ID** as the key.

The hash table uses open addressing with linear probing to resolve collisions. When two IDs map to the same index, linear probing searches for the next available slot sequentially until an empty slot is found.

Each faculty record stores:
- Faculty ID  
- Name  
- Department  
- Years of experience  

The hash function calculates the index using modulo operation:  
`index = id % table_size`

This method is efficient for small to medium-sized datasets and helps in understanding hashing, collision resolution, and record management.

# CODE 
```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

struct FacultyRecord {
    int id;
    string name;
    string department;
    int experience;
    FacultyRecord(int i = -1, string n = "", string d = "", int e = 0)
        : id(i), name(n), department(d), experience(e) {}
};

class FacultyHash {
    vector<FacultyRecord*> table;
    int capacity;
    int count;
    int hashFunc(int key) { return key % capacity; }

public:
    FacultyHash(int cap) : capacity(cap), count(0) { table.resize(capacity, nullptr); }
    ~FacultyHash() {
        for (auto p : table) if (p) delete p;
    }

    bool insertRecord(int id, const string &name, const string &dept, int exp) {
        if (count >= capacity) return false;
        int idx = hashFunc(id);
        int start = idx;
        while (table[idx] != nullptr && table[idx]->id != id) {
            idx = (idx + 1) % capacity;
            if (idx == start) return false;
        }
        if (table[idx] != nullptr && table[idx]->id == id) {
            table[idx]->name = name;
            table[idx]->department = dept;
            table[idx]->experience = exp;
        } else {
            table[idx] = new FacultyRecord(id, name, dept, exp);
            count++;
        }
        return true;
    }

    FacultyRecord* searchRecord(int id) {
        int idx = hashFunc(id);
        int start = idx;
        while (table[idx] != nullptr) {
            if (table[idx]->id == id) return table[idx];
            idx = (idx + 1) % capacity;
            if (idx == start) break;
        }
        return nullptr;
    }

    void displayTable() {
        cout << "\nFaculty Hash Table:\n";
        for (int i = 0; i < capacity; ++i) {
            cout << "Slot " << i << ": ";
            if (table[i] == nullptr) cout << "Empty\n";
            else cout << "(ID: " << table[i]->id << ", Name: " << table[i]->name
                      << ", Dept: " << table[i]->department
                      << ", Exp: " << table[i]->experience << ")\n";
        }
    }
};

int main() {
    int cap;
    cout << "Enter hash table capacity: ";
    cin >> cap;
    FacultyHash ht(cap);

    int choice = 0;
    while (true) {
        cout << "\n--- Faculty Hash Menu ---\n";
        cout << "1. Insert / Update Faculty\n";
        cout << "2. Search Faculty by ID\n";
        cout << "3. Display Table\n";
        cout << "4. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        if (choice == 1) {
            int id, exp; string name, dept;
            cout << "Enter ID: ";
            cin >> id;
            cin.ignore();
            cout << "Enter Name: ";
            getline(cin, name);
            cout << "Enter Department: ";
            getline(cin, dept);
            cout << "Enter Years of Experience: ";
            cin >> exp;
            bool ok = ht.insertRecord(id, name, dept, exp);
            if (ok) cout << "Inserted/Updated faculty with ID " << id << ".\n";
            else cout << "Insertion failed: table full.\n";
        }
        else if (choice == 2) {
            int id; cout << "Enter ID to search: "; cin >> id;
            FacultyRecord* rec = ht.searchRecord(id);
            if (rec) {
                cout << "Found: ID: " << rec->id << ", Name: " << rec->name
                     << ", Dept: " << rec->department
                     << ", Exp: " << rec->experience << '\n';
            } else cout << "Faculty with ID " << id << " not found.\n";
        }
        else if (choice == 3) {
            ht.displayTable();
        }
        else if (choice == 4) {
            cout << "Exiting.\n";
            break;
        }
        else {
            cout << "Invalid choice.\n";
        }
    }
    return 0;
}
```

# Output 

Enter hash table capacity: 5  

--- Faculty Hash Menu ---  
1. Insert / Update Faculty  
2. Search Faculty by ID  
3. Display Table  
4. Exit  
Enter choice: 1  
Enter ID: 101  
Enter Name: Farooq  
Enter Department: Computer  
Enter Years of Experience: 5  
Inserted/Updated faculty with ID 101.  

--- Faculty Hash Menu ---  
1. Insert / Update Faculty  
2. Search Faculty by ID  
3. Display Table  
4. Exit  
Enter choice: 1  
Enter ID: 106  
Enter Name: Fija  
Enter Department: IT  
Enter Years of Experience: 7  
Inserted/Updated faculty with ID 106.  

--- Faculty Hash Menu ---  
1. Insert / Update Faculty  
2. Search Faculty by ID  
3. Display Table  
4. Exit  
Enter choice: 1  
Enter ID: 109  
Enter Name: Rohan  
Enter Department: Electronics  
Enter Years of Experience: 4  
Inserted/Updated faculty with ID 109.  

--- Faculty Hash Menu ---  
1. Insert / Update Faculty  
2. Search Faculty by ID  
3. Display Table  
4. Exit  
Enter choice: 3  

Faculty Hash Table:  
Slot 0: Empty  
Slot 1: (ID: 101, Name: Farooq, Dept: Computer, Exp: 5)  
Slot 2: (ID: 106, Name: Fija, Dept: IT, Exp: 7)  
Slot 3: (ID: 109, Name: Rohan, Dept: Electronics, Exp: 4)  
Slot 4: Empty  

--- Faculty Hash Menu ---  
1. Insert / Update Faculty  
2. Search Faculty by ID  
3. Display Table  
4. Exit  
Enter choice: 2  
Enter ID to search: 101  
Found: ID: 101, Name: Farooq, Dept: Computer, Exp: 5  

--- Faculty Hash Menu ---  
1. Insert / Update Faculty  
2. Search Faculty by ID  
3. Display Table  
4. Exit  
Enter choice: 4  
Exiting.  
