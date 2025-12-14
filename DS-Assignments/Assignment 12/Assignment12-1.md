# Assignment 12.1

# Title : WAP to simulate a faculty database as a hash table using divide method as hash function with linear probing (without replacement) for collision handling.

# Thoery :

A hash table is an efficient data structure used to store and retrieve records using a unique key. In this assignment, a faculty database is simulated where each faculty (instructor) is identified uniquely by an employee ID.

The divide method of hashing is used as the hash function, where the hash index is calculated as:

index = employee_ID % table_size

When two records map to the same hash index, a collision occurs. To resolve collisions, linear probing without replacement is used. In this method, if the computed slot is occupied, the algorithm searches sequentially for the next free slot, but existing records are not displaced.

Each faculty record consists of:
- Employee ID  
- Full Name  
- Department  
- Years of Experience  

This approach provides fast access, efficient collision handling, and helps in understanding hashing techniques and open addressing.

# CODE 
```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

struct Instructor
{
    int empId;
    string fullName;
    string section;
    int years;
    Instructor(int id = -1, string name = "", string dept = "", int exp = 0)
        : empId(id), fullName(name), section(dept), years(exp) {}
};

class InstructorHash
{
private:
    vector<Instructor *> slots;
    int capacity;
    int total;

    int computeIndex(int id)
    {
        return id % capacity;
    }

public:
    InstructorHash(int cap) : capacity(cap), total(0)
    {
        slots.resize(capacity, nullptr);
    }

    ~InstructorHash()
    {
        for (int i = 0; i < capacity; ++i)
            if (slots[i])
                delete slots[i];
    }

    bool addInstructor(int id, const string &name, const string &dept, int exp)
    {
        if (total >= capacity)
        {
            cout << "Table reached maximum capacity!" << endl;
            return false;
        }

        int idx = computeIndex(id);
        int start = idx;

        while (slots[idx] != nullptr && slots[idx]->empId != id)
        {
            idx = (idx + 1) % capacity;
            if (idx == start)
            {
                cout << "No free slot found (wrapped)!" << endl;
                return false;
            }
        }

        if (slots[idx] != nullptr && slots[idx]->empId == id)
        {
            slots[idx]->fullName = name;
            slots[idx]->section = dept;
            slots[idx]->years = exp;
        }
        else
        {
            slots[idx] = new Instructor(id, name, dept, exp);
            total++;
        }
        return true;
    }

    Instructor *lookupInstructor(int id)
    {
        int idx = computeIndex(id);
        int start = idx;

        while (slots[idx] != nullptr)
        {
            if (slots[idx]->empId == id)
                return slots[idx];
            idx = (idx + 1) % capacity;
            if (idx == start)
                break;
        }
        return nullptr;
    }

    void displayTable()
    {
        for (int i = 0; i < capacity; ++i)
        {
            cout << "Bucket " << i << ": ";
            if (slots[i])
                cout << "(ID: " << slots[i]->empId << ", Name: " << slots[i]->fullName
                     << ", Dept: " << slots[i]->section << ", Exp: " << slots[i]->years << " yrs)";
            else
                cout << "Empty";
            cout << endl;
        }
    }
};

int main()
{
    cout << "Provide hash table size: ";
    int n;
    cin >> n;
    InstructorHash db(n);

    bool running = true;
    while (running)
    {
        cout << "\nSelect operation:\n";
        cout << "1 - Register new instructor\n";
        cout << "2 - Find instructor by ID\n";
        cout << "3 - Display instructor table\n";
        cout << "4 - Terminate program\n";
        cout << "Enter choice: ";
        int choice;
        cin >> choice;w
        switch (choice)
        {
        case 1:
        {
            cout << "Enter Instructor ID: ";
            int id;
            cin >> id;
            cin.ignore();
            cout << "Enter Instructor Name: ";
            string name;
            getline(cin, name);
            cout << "Enter Department: ";
            string dept;
            getline(cin, dept);
            cout << "Enter Years of Experience: ";
            int yrs;
            cin >> yrs;
            if (db.addInstructor(id, name, dept, yrs))
                cout << "Instructor recorded successfully." << endl;
            else
                cout << "Failed to record instructor." << endl;
            break;
        }
        case 2:
        {
            cout << "Enter ID to search: ";
            int id;
            cin >> id;
            Instructor *res = db.lookupInstructor(id);
            if (res)
                cout << "Found -> ID " << res->empId << ", " << res->fullName << ", "
                     << res->section << ", " << res->years << " yrs" << endl;
            else
                cout << "No instructor found with ID " << id << "." << endl;
            break;
        }
        case 3:
            db.displayTable();
            break;
        case 4:
            running = false;
            cout << "Exiting. Goodbye." << endl;
            break;
        default:
            cout << "Invalid option. Try again." << endl;
            break;
        }
    }

    return 0;
}
```

# Output 

Provide hash table size: 5  

Select operation:  
1 - Register new instructor  
2 - Find instructor by ID  
3 - Display instructor table  
4 - Terminate program  
Enter choice: 1  
Enter Instructor ID: 101  
Enter Instructor Name: Farooq  
Enter Department: Computer  
Enter Years of Experience: 6  
Instructor recorded successfully.  

Select operation:  
1 - Register new instructor  
2 - Find instructor by ID  
3 - Display instructor table  
4 - Terminate program  
Enter choice: 1  
Enter Instructor ID: 106  
Enter Instructor Name: Fija  
Enter Department: IT  
Enter Years of Experience: 8  
Instructor recorded successfully.  

Select operation:  
1 - Register new instructor  
2 - Find instructor by ID  
3 - Display instructor table  
4 - Terminate program  
Enter choice: 1  
Enter Instructor ID: 109  
Enter Instructor Name: Roshan  
Enter Department: Electronics  
Enter Years of Experience: 5  
Instructor recorded successfully.  

Select operation:  
1 - Register new instructor  
2 - Find instructor by ID  
3 - Display instructor table  
4 - Terminate program  
Enter choice: 3  
Bucket 0: Empty  
Bucket 1: (ID: 101, Name: Farooq, Dept: Computer, Exp: 6 yrs)  
Bucket 2: (ID: 106, Name: Fija, Dept: IT, Exp: 8 yrs)  
Bucket 3: (ID: 109, Name: Roshan, Dept: Electronics, Exp: 5 yrs)  
Bucket 4: Empty  

Select operation:  
1 - Register new instructor  
2 - Find instructor by ID  
3 - Display instructor table  
4 - Terminate program  
Enter choice: 2  
Enter ID to search: 101  
Found -> ID 101, Farooq, Computer, 6 yrs  

Select operation:  
1 - Register new instructor  
2 - Find instructor by ID  
3 - Display instructor table  
4 - Terminate program  
Enter choice: 4  
Exiting. Goodbye.  