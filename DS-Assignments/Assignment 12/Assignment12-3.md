
# Assignment 12.3

# Title : 
WAP to simulate employee databases as a hash table. Search a particular faculty by using Mid Square method as a hash function for linear probing method of collision handling technique. Assume suitable data for faculty record.

# Thoery :

A hash table is an efficient data structure used to store and retrieve records using a unique key. In this assignment, an employee database is simulated using a hash table, where each employee record is identified by a unique employee ID.

The Mid Square Hashing Method is used to compute the hash index. In this method, the key is squared and a few middle digits of the resulting number are extracted to determine the hash index. This technique helps in distributing keys more uniformly across the hash table.

Linear probing is used as the collision handling technique. When a collision occurs (i.e., the calculated index is already occupied), the algorithm sequentially checks the next available slots until an empty slot is found.

Each employee record stores:
- Employee ID  
- Employee Name  
- Unit / Department  
- Years of Experience  

This program demonstrates hashing techniques, collision resolution using linear probing, and efficient searching in hash tables.

# CODE 
```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

struct Employee
{
    int empId;
    string fullName;
    string unit;
    int yearsExp;
    Employee(int id = -1, string name = "", string dept = "", int exp = 0) 
        : empId(id), fullName(name), unit(dept), yearsExp(exp) {}
};

class EmployeeHash
{
private:
    vector<Employee *> storage;
    int capacity;
    int occupied;

    int midSquareHash(int id)
    {
        long long sq = (long long)id * id;
        string s = to_string(sq);
        int len = s.length();
        int take = 3;
        if (len < take) return stoi(s) % capacity;
        int start = (len - take) / 2;
        string mid = s.substr(start, take);
        int midVal = stoi(mid);
        return midVal % capacity;
    }

public:
    EmployeeHash(int cap) : capacity(cap), occupied(0)
    {
        storage.resize(capacity, nullptr);
    }

    ~EmployeeHash()
    {
        for (int i = 0; i < capacity; ++i)
            if (storage[i])
                delete storage[i];
    }

    bool insertRecord(int id, const string &name, const string &dept, int exp)
    {
        if (occupied >= capacity)
        {
            cout << "Registry full!" << endl;
            return false;
        }
        int idx = midSquareHash(id);
        int start = idx;
        while (storage[idx] != nullptr && storage[idx]->empId != id)
        {
            idx = (idx + 1) % capacity;
            if (idx == start)
            {
                cout << "No slot available (wrapped)!" << endl;
                return false;
            }
        }
        if (storage[idx] != nullptr && storage[idx]->empId == id)
        {
            storage[idx]->fullName = name;
            storage[idx]->unit = dept;
            storage[idx]->yearsExp = exp;
        }
        else
        {
            storage[idx] = new Employee(id, name, dept, exp);
            occupied++;
        }
        return true;
    }

    Employee *searchById(int id)
    {
        int idx = midSquareHash(id);
        int start = idx;
        while (storage[idx] != nullptr)
        {
            if (storage[idx]->empId == id)
                return storage[idx];
            idx = (idx + 1) % capacity;
            if (idx == start) break;
        }
        return nullptr;
    }

    void displayAll()
    {
        for (int i = 0; i < capacity; ++i)
        {
            cout << "Slot " << i << ": ";
            if (storage[i])
                cout << "(ID:" << storage[i]->empId 
                     << ", Name:" << storage[i]->fullName
                     << ", Unit:" << storage[i]->unit 
                     << ", Exp:" << storage[i]->yearsExp << " yrs)";
            else
                cout << "Empty";
            cout << endl;
        }
    }
};

int main()
{
    cout << "Enter desired hash registry size: ";
    int size;
    cin >> size;
    EmployeeHash registry(size);

    bool run = true;
    while (run)
    {
        cout << "\nOptions:\n";
        cout << "1 - Add employee\n";
        cout << "2 - Lookup employee by ID\n";
        cout << "3 - Show registry\n";
        cout << "4 - Exit\n";
        cout << "Select choice: ";
        int choice;
        cin >> choice;
        switch (choice)
        {
        case 1:
        {
            cout << "Enter Employee ID: ";
            int id;
            cin >> id;
            cin.ignore();
            cout << "Enter Employee Name: ";
            string name;
            getline(cin, name);
            cout << "Enter Unit/Department: ";
            string unit;
            getline(cin, unit);
            cout << "Enter Years of Experience: ";
            int yrs;
            cin >> yrs;
            if (registry.insertRecord(id, name, unit, yrs))
                cout << "Employee added/updated successfully." << endl;
            else
                cout << "Failed to add employee." << endl;
            break;
        }
        case 2:
        {
            cout << "Enter ID to search: ";
            int id;
            cin >> id;
            Employee *e = registry.searchById(id);
            if (e)
                cout << "Found -> ID " << e->empId << ", " 
                     << e->fullName << ", " 
                     << e->unit << ", " 
                     << e->yearsExp << " yrs" << endl;
            else
                cout << "No record for ID " << id << "." << endl;
            break;
        }
        case 3:
            registry.displayAll();
            break;
        case 4:
            run = false;
            cout << "Closing registry." << endl;
            break;
        default:
            cout << "Invalid input. Try again." << endl;
            break;
        }
    }
    return 0;
}
```

# Output 

Enter desired hash registry size: 5  

Options:  
1 - Add employee  
2 - Lookup employee by ID  
3 - Show registry  
4 - Exit  
Select choice: 1  
Enter Employee ID: 101  
Enter Employee Name: Farooq  
Enter Unit/Department: Computer  
Enter Years of Experience: 6  
Employee added/updated successfully.  

Options:  
1 - Add employee  
2 - Lookup employee by ID  
3 - Show registry  
4 - Exit  
Select choice: 1  
Enter Employee ID: 106  
Enter Employee Name: Fija  
Enter Unit/Department: IT  
Enter Years of Experience: 5  
Employee added/updated successfully.  

Options:  
1 - Add employee  
2 - Lookup employee by ID  
3 - Show registry  
4 - Exit  
Select choice: 1  
Enter Employee ID: 109  
Enter Employee Name: Roha  
Enter Unit/Department: Electronics  
Enter Years of Experience: 4  
Employee added/updated successfully.  

Options:  
1 - Add employee  
2 - Lookup employee by ID  
3 - Show registry  
4 - Exit  
Select choice: 3  
Slot 0: Empty  
Slot 1: (ID:101, Name:Farooq, Unit:Computer, Exp:6 yrs)  
Slot 2: (ID:106, Name:Fija, Unit:IT, Exp:5 yrs)  
Slot 3: (ID:109, Name:Roha, Unit:Electronics, Exp:4 yrs)  
Slot 4: Empty  

Options:  
1 - Add employee  
2 - Lookup employee by ID  
3 - Show registry  
4 - Exit  
Select choice: 2  
Enter ID to search: 101  
Found -> ID 101, Farooq, Computer, 6 yrs  

Options:  
1 - Add employee  
2 - Lookup employee by ID  
3 - Show registry  
4 - Exit  
Select choice: 4  
Closing registry.  
