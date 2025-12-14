
# Assignment 12.2

# Title : WAP to simulate a faculty database as a hash table using MOD hash function with linear probing and chaining with replacement method for collision handling.

# Thoery :

A hash table is an efficient data structure used to store and retrieve records using a unique key. In this assignment, a faculty database is implemented using hashing, where each faculty record is identified by a unique Faculty ID.

The MOD (modulo) hash function is used to compute the bucket index as:  
index = faculty_id % table_size

To handle collisions, separate chaining with replacement** technique is used. In this method, each bucket maintains a linked listof records that map to the same hash index. When a collision occurs, the new record is inserted into the linked list of that bucket.

Each faculty record contains:
- Faculty ID  
- Name  
- Department  
- Years of experience  

This approach helps in understanding hashing techniques, collision resolution using linked lists, and efficient record management.

# CODE
```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

struct Lecturer
{
    int id;
    string name;
    string department;
    int experience;
    Lecturer(int i = -1, string n = "", string d = "", int e = 0) : id(i), name(n), department(d), experience(e) {}
};

struct ListNode
{
    Lecturer data;
    ListNode *next;
    ListNode(const Lecturer &L) : data(L), next(nullptr) {}
};

class LecturerHashTable
{
private:
    vector<ListNode *> buckets;
    int bucketCount;

    int computeBucket(int id)
    {
        return id % bucketCount;
    }

public:
    LecturerHashTable(int count) : bucketCount(count)
    {
        buckets.resize(bucketCount, nullptr);
    }

    ~LecturerHashTable()
    {
        for (int i = 0; i < bucketCount; ++i)
        {
            ListNode *cur = buckets[i];
            while (cur)
            {
                ListNode *tmp = cur;
                cur = cur->next;
                delete tmp;
            }
        }
    }

    void addLecturer(int id, const string &name, const string &dept, int exp)
    {
        int idx = computeBucket(id);
        Lecturer L(id, name, dept, exp);
        ListNode *node = new ListNode(L);
        if (!buckets[idx])
        {
            buckets[idx] = node;
            return;
        }
        ListNode *cur = buckets[idx];
        while (cur)
        {
            if (cur->data.id == id)
            {
                cur->data.name = name;
                cur->data.department = dept;
                cur->data.experience = exp;
                delete node;
                return;
            }
            if (!cur->next)
                break;
            cur = cur->next;
        }
        cur->next = node;
    }

    Lecturer *findLecturer(int id)
    {
        int idx = computeBucket(id);
        ListNode *cur = buckets[idx];
        while (cur)
        {
            if (cur->data.id == id)
                return &cur->data;
            cur = cur->next;
        }
        return nullptr;
    }

    void showTable()
    {
        for (int i = 0; i < bucketCount; ++i)
        {
            cout << "Chain " << i << ": ";
            ListNode *cur = buckets[i];
            if (!cur)
            {
                cout << "Empty";
            }
            while (cur)
            {
                cout << "(ID:" << cur->data.id << ", Name:" << cur->data.name
                     << ", Dept:" << cur->data.department << ", Exp:" << cur->data.experience << " yrs)";
                if (cur->next)
                    cout << " -> ";
                cur = cur->next;
            }
            cout << endl;
        }
    }
};

int main()
{
    cout << "Specify number of chains for the faculty registry: ";
    int chains;
    cin >> chains;
    LecturerHashTable registry(chains);

    bool running = true;
    while (running)
    {
        cout << "\nMenu:\n";
        cout << "1. Insert faculty record\n";
        cout << "2. Search faculty by ID\n";
        cout << "3. Display registry\n";
        cout << "4. Quit\n";
        cout << "Choose option: ";
        int opt;
        cin >> opt;
        switch (opt)
        {
        case 1:
        {
            cout << "Provide Faculty ID: ";
            int id;
            cin >> id;
            cin.ignore();
            cout << "Provide Full Name: ";
            string name;
            getline(cin, name);
            cout << "Provide Department Name: ";
            string dept;
            getline(cin, dept);
            cout << "Provide Years of Experience: ";
            int yrs;
            cin >> yrs;
            registry.addLecturer(id, name, dept, yrs);
            cout << "Record stored successfully." << endl;
            break;
        }
        case 2:
        {
            cout << "Enter Faculty ID to lookup: ";
            int id;
            cin >> id;
            Lecturer *L = registry.findLecturer(id);
            if (L)
                cout << "Located -> ID " << L->id << ", " << L->name << ", " << L->department << ", " << L->experience << " yrs" << endl;
            else
                cout << "No record found for ID " << id << "." << endl;
            break;
        }
        case 3:
            registry.showTable();
            break;
        case 4:
            running = false;
            cout << "Terminating registry application." << endl;
            break;
        default:
            cout << "Invalid selection, try again." << endl;
            break;
        }
    }

    return 0;
}
```

# Output 

Specify number of chains for the faculty registry: 5  

Menu:  
1. Insert faculty record  
2. Search faculty by ID  
3. Display registry  
4. Quit  
Choose option: 1  
Provide Faculty ID: 101  
Provide Full Name: Farooq  
Provide Department Name: Computer  
Provide Years of Experience: 6  
Record stored successfully.  

Menu:  
1. Insert faculty record  
2. Search faculty by ID  
3. Display registry  
4. Quit  
Choose option: 1  
Provide Faculty ID: 106  
Provide Full Name: Fija  
Provide Department Name: IT  
Provide Years of Experience: 8  
Record stored successfully.  

Menu:  
1. Insert faculty record  
2. Search faculty by ID  
3. Display registry  
4. Quit  
Choose option: 1  
Provide Faculty ID: 109  
Provide Full Name: Rohan  
Provide Department Name: Electronics  
Provide Years of Experience: 5  
Record stored successfully.  

Menu:  
1. Insert faculty record  
2. Search faculty by ID  
3. Display registry  
4. Quit  
Choose option: 3  
Chain 0: Empty  
Chain 1: (ID:101, Name:Farooq, Dept:Computer, Exp:6 yrs) -> (ID:106, Name:Fija, Dept:IT, Exp:8 yrs) -> (ID:109, Name:Rohan, Dept:Electronics, Exp:5 yrs)  
Chain 2: Empty  
Chain 3: Empty  
Chain 4: Empty  

Menu:  
1. Insert faculty record  
2. Search faculty by ID  
3. Display registry  
4. Quit  
Choose option: 2  
Enter Faculty ID to lookup: 101  
Located -> ID 101, Farooq, Computer, 6 yrs  

Menu:  
1. Insert faculty record  
2. Search faculty by ID  
3. Display registry  
4. Quit  
Choose option: 4  
Terminating registry application.