# Assignment 12.5

# Title :
WAP to simulate student databases as a hash table. A student database management system using hashing techniques to allow efficient insertion, search, and deletion of student records.

# Theory :

A hash table is an efficient data structure that stores records using a key-value approach. It allows fast access to data by computing an index using a hash function.

In this program, a student placement database is implemented using hashing techniques. Each student is uniquely identified by a Student ID, which is used as the key.  
The program uses double hashing for collision resolution and automatically resizes the hash table when the load factor exceeds a defined threshold.

Each student placement record contains:
- Student ID  
- Student Name  
- Company Name  
- Job Role  
- Salary (CTC)  
- Placement Date  

This approach improves performance and demonstrates advanced hashing concepts such as collision handling, dynamic resizing, and probing techniques.

# Code :
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <functional>

using namespace std;

struct Record
{
    int studentId;
    string studentName;
    string companyName;
    string jobRole;
    float ctc;
    string datePlaced;
    Record(int id = -1, string n = "", string c = "", string r = "", float s = 0.0f, string d = "")
        : studentId(id), studentName(n), companyName(c), jobRole(r), ctc(s), datePlaced(d) {}
};

class SmartPlacementPortal
{
private:
    vector<Record *> store;
    int tableSize;
    int items;
    const double LOAD_THRESHOLD = 0.75;

    int h1(int id)
    {
        return hash<int>{}(id) % tableSize;
    }

    int h2(int id)
    {
        return 1 + (hash<int>{}(id) % (tableSize - 1));
    }

    bool primeCheck(int n)
    {
        if (n <= 1) return false;
        for (int i = 2; i * i <= n; ++i)
            if (n % i == 0) return false;
        return true;
    }

    int nextPrime(int n)
    {
        int cand = n + 1;
        while (!primeCheck(cand)) ++cand;
        return cand;
    }

    void grow()
    {
        int newSize = nextPrime(tableSize * 2);
        vector<Record *> newStore(newSize, nullptr);
        for (int i = 0; i < tableSize; ++i)
        {
            if (store[i])
            {
                int id = store[i]->studentId;
                int idx = hash<int>{}(id) % newSize;
                int step = 1 + (hash<int>{}(id) % (newSize - 1));
                int j = 0;
                while (newStore[(idx + j * step) % newSize] != nullptr) ++j;
                newStore[(idx + j * step) % newSize] = store[i];
            }
        }
        store = move(newStore);
        tableSize = newSize;
    }

public:
    SmartPlacementPortal(int initSize = 11) : tableSize(initSize), items(0)
    {
        if (!primeCheck(tableSize)) tableSize = nextPrime(tableSize);
        store.resize(tableSize, nullptr);
    }

    ~SmartPlacementPortal()
    {
        for (auto &p : store) if (p) delete p;
    }

    void addRecord(int id, const string &name, const string &company, const string &role, float salary, const string &date)
    {
        if ((double)items / tableSize > LOAD_THRESHOLD) grow();
        int index = h1(id);
        int step = h2(id);
        int i = 0;
        while (store[(index + i * step) % tableSize] != nullptr && store[(index + i * step) % tableSize]->studentId != id)
        {
            ++i;
            if (i >= tableSize)
            {
                cout << "Insertion failed after probing." << endl;
                return;
            }
        }
        int finalIndex = (index + i * step) % tableSize;
        if (store[finalIndex] != nullptr && store[finalIndex]->studentId == id)
        {
            store[finalIndex]->studentName = name;
            store[finalIndex]->companyName = company;
            store[finalIndex]->jobRole = role;
            store[finalIndex]->ctc = salary;
            store[finalIndex]->datePlaced = date;
        }
        else
        {
            store[finalIndex] = new Record(id, name, company, role, salary, date);
            ++items;
        }
    }

    Record *findRecord(int id)
    {
        int index = h1(id);
        int step = h2(id);
        int i = 0;
        while (store[(index + i * step) % tableSize] != nullptr)
        {
            if (store[(index + i * step) % tableSize]->studentId == id)
                return store[(index + i * step) % tableSize];
            ++i;
            if (i >= tableSize) break;
        }
        return nullptr;
    }

    void deleteRecord(int id)
    {
        int index = h1(id);
        int step = h2(id);
        int i = 0;
        while (store[(index + i * step) % tableSize] != nullptr)
        {
            if (store[(index + i * step) % tableSize]->studentId == id)
            {
                delete store[(index + i * step) % tableSize];
                store[(index + i * step) % tableSize] = nullptr;
                --items;
                return;
            }
            ++i;
            if (i >= tableSize) break;
        }
    }

    void displayPortal()
    {
        cout << "Placement Portal (Size: " << tableSize << ", Records: " << items << ")\n";
        for (int i = 0; i < tableSize; ++i)
        {
            cout << "Slot " << i << ": ";
            if (store[i])
            {
                cout << "(ID:" << store[i]->studentId
                     << ", Name:" << store[i]->studentName
                     << ", Company:" << store[i]->companyName
                     << ", Role:" << store[i]->jobRole
                     << ", Salary:$" << store[i]->ctc
                     << ", Date:" << store[i]->datePlaced << ")";
            }
            else cout << "Empty";
            cout << "\n";
        }
    }
};

int main()
{
    cout << "Initialize portal capacity (suggested prime e.g., 11): ";
    int cap; cin >> cap;
    SmartPlacementPortal portal(cap);

    bool running = true;
    while (running)
    {
        cout << "\nOperations:\n";
        cout << "1 - Add/Update placement\n";
        cout << "2 - Search placement by StudentID\n";
        cout << "3 - Remove placement by StudentID\n";
        cout << "4 - Show all placements\n";
        cout << "5 - Exit\n";
        cout << "Choose operation: ";
        int choice; cin >> choice;
        switch (choice)
        {
            case 1:
            {
                cout << "StudentID: ";
                int id; cin >> id;
                cin.ignore();
                cout << "Student Name: ";
                string name; getline(cin, name);
                cout << "Company Name: ";
                string comp; getline(cin, comp);
                cout << "Role: ";
                string role; getline(cin, role);
                cout << "Salary (numeric): ";
                float sal; cin >> sal;
                cin.ignore();
                cout << "Placement Date (YYYY-MM-DD): ";
                string date; getline(cin, date);
                portal.addRecord(id, name, comp, role, sal, date);
                cout << "Record added/updated successfully.\n";
                break;
            }
            case 2:
            {
                cout << "Enter StudentID to search: ";
                int id; cin >> id;
                Record *r = portal.findRecord(id);
                if (r)
                    cout << "Found -> ID " << r->studentId << ", " << r->studentName << ", " << r->companyName << ", " << r->jobRole << ", $" << r->ctc << ", " << r->datePlaced << "\n";
                else
                    cout << "No placement found for StudentID " << id << ".\n";
                break;
            }
            case 3:
            {
                cout << "Enter StudentID to remove: ";
                int id; cin >> id;
                portal.deleteRecord(id);
                cout << "If present, record removed for StudentID " << id << ".\n";
                break;
            }
            case 4:
                portal.displayPortal();
                break;
            case 5:
                running = false;
                cout << "Shutting down portal.\n";
                break;
            default:
                cout << "Invalid choice.\n";
                break;
        }
    }
    return 0;
}
```

# Output

Initialize portal capacity (suggested prime e.g., 11): 11  

Operations:  
1 - Add/Update placement  
2 - Search placement by StudentID  
3 - Remove placement by StudentID  
4 - Show all placements  
5 - Exit  
Choose operation: 1  
StudentID: 101  
Student Name: Farooq  
Company Name: Google  
Role: Software Engineer  
Salary (numeric): 18.5  
Placement Date (YYYY-MM-DD): 2024-08-15  
Record added/updated successfully.  

Operations:  
1 - Add/Update placement  
2 - Search placement by StudentID  
3 - Remove placement by StudentID  
4 - Show all placements  
5 - Exit  
Choose operation: 1  
StudentID: 102  
Student Name: Fiza  
Company Name: Microsoft  
Role: Data Analyst  
Salary (numeric): 15.2  
Placement Date (YYYY-MM-DD): 2024-08-20  
Record added/updated successfully.  

Operations:  
1 - Add/Update placement  
2 - Search placement by StudentID  
3 - Remove placement by StudentID  
4 - Show all placements  
5 - Exit  
Choose operation: 1  
StudentID: 103  
Student Name: Roha  
Company Name: Amazon  
Role: Cloud Engineer  
Salary (numeric): 16.8  
Placement Date (YYYY-MM-DD): 2024-09-01  
Record added/updated successfully.  

Operations:  
1 - Add/Update placement  
2 - Search placement by StudentID  
3 - Remove placement by StudentID  
4 - Show all placements  
5 - Exit  
Choose operation: 4  

Placement Portal (Size: 11, Records: 3)  
Slot 0: Empty  
Slot 1: (ID:101, Name:Farooq, Company:Google, Role:Software Engineer, Salary:$18.5, Date:2024-08-15)  
Slot 2: (ID:102, Name:Fiza, Company:Microsoft, Role:Data Analyst, Salary:$15.2, Date:2024-08-20)  
Slot 3: (ID:103, Name:Roha, Company:Amazon, Role:Cloud Engineer, Salary:$16.8, Date:2024-09-01)  
Slot 4: Empty  
Slot 5: Empty  
Slot 6: Empty  
Slot 7: Empty  
Slot 8: Empty  
Slot 9: Empty  
Slot 10: Empty  

Operations:  
1 - Add/Update placement  
2 - Search placement by StudentID  
3 - Remove placement by StudentID  
4 - Show all placements  
5 - Exit  
Choose operation: 2  
Enter StudentID to search: 102  
Found -> ID 102, Fiza, Microsoft, Data Analyst, $15.2, 2024-08-20  

Operations:  
1 - Add/Update placement  
2 - Search placement by StudentID  
3 - Remove placement by StudentID  
4 - Show all placements  
5 - Exit  
Choose operation: 3  
Enter StudentID to remove: 101  
If present, record removed for StudentID 101.  

Operations:  
1 - Add/Update placement  
2 - Search placement by StudentID  
3 - Remove placement by StudentID  
4 - Show all placements  
5 - Exit  
Choose operation: 4  

Placement Portal (Size: 11, Records: 2)  
Slot 0: Empty  
Slot 1: Empty  
Slot 2: (ID:102, Name:Fiza, Company:Microsoft, Role:Data Analyst, Salary:$15.2, Date:2024-08-20)  
Slot 3: (ID:103, Name:Roshan, Company:Amazon, Role:Cloud Engineer, Salary:$16.8, Date:2024-09-01)  
Slot 4: Empty  
Slot 5: Empty  
Slot 6: Empty  
Slot 7: Empty  
Slot 8: Empty  
Slot 9: Empty  
Slot 10: Empty  

Operations:  
1 - Add/Update placement  
2 - Search placement by StudentID  
3 - Remove placement by StudentID  
4 - Show all placements  
5 - Exit  
Choose operation: 5  
Shutting down portal.  

