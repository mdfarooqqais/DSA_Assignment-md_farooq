
# Assignment 12.4

# Title :
WAP to simulate student databases as a hash table. A student database management system using hashing techniques to allow efficient insertion, search, and deletion of student records.

# Thoery :

A hash table is an efficient data structure that stores data using a key–value mapping, enabling fast insertion, searching, and deletion operations. In this assignment, a student database is implemented using hashing techniques.

Each student record is uniquely identified using a roll number, which acts as the hash key. The hash function computes the index using the modulo method. To handle collisions, separate chaining is used, where multiple records mapping to the same index are stored in a linked list.

Each student record contains:
- Roll Number  
- Name  
- Age  
- Grade  

This approach improves performance compared to linear searching and demonstrates the practical use of hashing with linked lists for database management.

# CODE
```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

struct Pupil
{
    int roll;
    string name;
    int age;
    float score;
    Pupil(int r = -1, string n = "", int a = 0, float s = 0.0f)
        : roll(r), name(n), age(a), score(s) {}
};

struct BucketNode
{
    Pupil info;
    BucketNode *next;
    BucketNode(Pupil p) : info(p), next(nullptr) {}
};

class PupilRegistry
{
private:
    vector<BucketNode *> chains;
    int chainsCount;

    int computeIndex(int rollNumber)
    {
        return rollNumber % chainsCount;
    }

public:
    PupilRegistry(int c) : chainsCount(c)
    {
        chains.resize(chainsCount, nullptr);
    }

    ~PupilRegistry()
    {
        for (int i = 0; i < chainsCount; ++i)
        {
            BucketNode *cur = chains[i];
            while (cur)
            {
                BucketNode *tmp = cur;
                cur = cur->next;
                delete tmp;
            }
        }
    }

    void addPupil(int rollNumber, const string &name, int age, float score)
    {
        int idx = computeIndex(rollNumber);
        Pupil p(rollNumber, name, age, score);
        BucketNode *node = new BucketNode(p);

        if (!chains[idx])
        {
            chains[idx] = node;
            return;
        }

        BucketNode *cur = chains[idx];
        while (cur->next)
        {
            if (cur->info.roll == rollNumber)
            {
                cur->info = p;
                delete node;
                return;
            }
            cur = cur->next;
        }

        if (cur->info.roll == rollNumber)
        {
            cur->info = p;
            delete node;
            return;
        }

        cur->next = node;
    }

    Pupil *findPupil(int rollNumber)
    {
        int idx = computeIndex(rollNumber);
        BucketNode *cur = chains[idx];
        while (cur)
        {
            if (cur->info.roll == rollNumber)
                return &cur->info;
            cur = cur->next;
        }
        return nullptr;
    }

    void deletePupil(int rollNumber)
    {
        int idx = computeIndex(rollNumber);
        BucketNode *cur = chains[idx];
        BucketNode *prev = nullptr;
        while (cur)
        {
            if (cur->info.roll == rollNumber)
            {
                if (prev)
                    prev->next = cur->next;
                else
                    chains[idx] = cur->next;
                delete cur;
                return;
            }
            prev = cur;
            cur = cur->next;
        }
    }

    void displayRegistry()
    {
        for (int i = 0; i < chainsCount; ++i)
        {
            cout << "Chain " << i << ": ";
            BucketNode *cur = chains[i];
            if (!cur)
                cout << "Empty";
            while (cur)
            {
                cout << "(Roll:" << cur->info.roll << ", Name:" << cur->info.name
                     << ", Age:" << cur->info.age << ", Grade:" << cur->info.score << ")";
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
    cout << "Enter number of buckets for the student registry: ";
    int buckets;
    cin >> buckets;
    PupilRegistry registry(buckets);

    bool active = true;
    while (active)
    {
        cout << "\nSelect an action:\n";
        cout << "1 - Insert/Update student\n";
        cout << "2 - Search student by roll\n";
        cout << "3 - Delete student by roll\n";
        cout << "4 - Show registry\n";
        cout << "5 - Exit\n";
        cout << "Your choice: ";
        int choice;
        cin >> choice;

        switch (choice)
        {
        case 1:
        {
            int r, a;
            float g;
            string nm;
            cout << "Provide Roll Number: ";
            cin >> r;
            cin.ignore();
            cout << "Provide Student Name: ";
            getline(cin, nm);
            cout << "Provide Age: ";
            cin >> a;
            cout << "Provide Grade (numeric): ";
            cin >> g;
            registry.addPupil(r, nm, a, g);
            cout << "Student record inserted/updated." << endl;
            break;
        }
        case 2:
        {
            int r;
            cout << "Enter Roll Number to search: ";
            cin >> r;
            Pupil *p = registry.findPupil(r);
            if (p)
                cout << "Found -> Roll " << p->roll << ", " << p->name
                     << ", Age " << p->age << ", Grade " << p->score << endl;
            else
                cout << "No student with roll " << r << " found." << endl;
            break;
        }
        case 3:
        {
            int r;
            cout << "Enter Roll Number to delete: ";
            cin >> r;
            registry.deletePupil(r);
            cout << "If present, student with roll " << r << " deleted." << endl;
            break;
        }
        case 4:
            registry.displayRegistry();
            break;
        case 5:
            active = false;
            cout << "Registry closed." << endl;
            break;
        default:
            cout << "Invalid option, please choose again." << endl;
        }
    }
    return 0;
}
```

# Output

Enter number of buckets for the student registry: 5  

Select an action:  
1 - Insert/Update student  
2 - Search student by roll  
3 - Delete student by roll  
4 - Show registry  
5 - Exit  
Your choice: 1  
Provide Roll Number: 101  
Provide Student Name: Farooq  
Provide Age: 20  
Provide Grade (numeric): 8.6  
Student record inserted/updated.  

Select an action:  
1 - Insert/Update student  
2 - Search student by roll  
3 - Delete student by roll  
4 - Show registry  
5 - Exit  
Your choice: 1  
Provide Roll Number: 106  
Provide Student Name: Fija  
Provide Age: 21  
Provide Grade (numeric): 9.1  
Student record inserted/updated.  

Select an action:  
1 - Insert/Update student  
2 - Search student by roll  
3 - Delete student by roll  
4 - Show registry  
5 - Exit  
Your choice: 1  
Provide Roll Number: 109  
Provide Student Name: Roha  
Provide Age: 19  
Provide Grade (numeric): 8.4  
Student record inserted/updated.  

Select an action:  
1 - Insert/Update student  
2 - Search student by roll  
3 - Delete student by roll  
4 - Show registry  
5 - Exit  
Your choice: 4  
Chain 0: Empty  
Chain 1: (Roll:101, Name:Farooq, Age:20, Grade:8.6) -> (Roll:106, Name:Fija, Age:21, Grade:9.1)  
Chain 2: (Roll:109, Name:Roha, Age:19, Grade:8.4)  
Chain 3: Empty  
Chain 4: Empty  

Select an action:  
1 - Insert/Update student  
2 - Search student by roll  
3 - Delete student by roll  
4 - Show registry  
5 - Exit  
Your choice: 2  
Enter Roll Number to search: 101  
Found -> Roll 101, Farooq, Age 20, Grade 8.6  

Select an action:  
1 - Insert/Update student  
2 - Search student by roll  
3 - Delete student by roll  
4 - Show registry  
5 - Exit  
Your choice: 5  
Registry closed.  