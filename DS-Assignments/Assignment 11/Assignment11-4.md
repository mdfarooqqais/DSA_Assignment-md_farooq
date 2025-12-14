
# Assignment 11.4

# Title : Store and retrieve student records using roll numbers.

# Thoery :

A student record management system stores and retrieves student information efficiently using a unique identifier such as a roll number . To achieve fast access, a  hash table is used.

In this program, separate chaining using linked lists is employed to handle collisions. Each bucket of the hash table points to a linked list of student records that hash to the same bucket index.

The hash function uses the roll number modulo the number of buckets to determine the appropriate bucket. Each record contains the student’s roll number, name, and grade.

This approach allows:
- Efficient insertion and updating of records
- Fast searching by roll number
- Safe deletion of records
- Organized storage using buckets and linked lists

It helps in understanding hashing techniques, linked list operations, and dynamic memory management.


# CODE 
```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

struct RecordNode
{
    int roll;
    string name;
    float grade;
    RecordNode *next;
    RecordNode(int r, const string &n, float g) : roll(r), name(n), grade(g), next(nullptr) {}
};

class StudentDirectory
{
    vector<RecordNode*> buckets;
    int capacity;

    int bucketIndex(int roll)
    {
        return roll % capacity;
    }

public:
    StudentDirectory(int size) : capacity(size)
    {
        buckets.resize(capacity, nullptr);
    }

    ~StudentDirectory()
    {
        for (int i = 0; i < capacity; ++i)
        {
            RecordNode *cur = buckets[i];
            while (cur)
            {
                RecordNode *tmp = cur;
                cur = cur->next;
                delete tmp;
            }
        }
    }

    void addRecord(int roll, const string &name, float grade)
    {
        int idx = bucketIndex(roll);
        RecordNode *cur = buckets[idx];
        if (!cur)
        {
            buckets[idx] = new RecordNode(roll, name, grade);
            cout << "Record added in bucket " << idx << ".\n";
            return;
        }
        RecordNode *prev = nullptr;
        while (cur)
        {
            if (cur->roll == roll)
            {
                cur->name = name;
                cur->grade = grade;
                cout << "Existing record updated in bucket " << idx << ".\n";
                return;
            }
            prev = cur;
            cur = cur->next;
        }
        prev->next = new RecordNode(roll, name, grade);
        cout << "Record appended in bucket " << idx << ".\n";
    }

    RecordNode* findRecord(int roll)
    {
        int idx = bucketIndex(roll);
        RecordNode *cur = buckets[idx];
        while (cur)
        {
            if (cur->roll == roll)
                return cur;
            cur = cur->next;
        }
        return nullptr;
    }

    void deleteRecord(int roll)
    {
        int idx = bucketIndex(roll);
        RecordNode *cur = buckets[idx];
        RecordNode *prev = nullptr;
        while (cur)
        {
            if (cur->roll == roll)
            {
                if (prev)
                    prev->next = cur->next;
                else
                    buckets[idx] = cur->next;
                delete cur;
                cout << "Record with roll " << roll << " deleted from bucket " << idx << ".\n";
                return;
            }
            prev = cur;
            cur = cur->next;
        }
        cout << "Record with roll " << roll << " not found.\n";
    }

    void displayDirectory()
    {
        cout << "\n--- Student Directory ---\n";
        for (int i = 0; i < capacity; ++i)
        {
            cout << "Bucket " << i << ": ";
            RecordNode *cur = buckets[i];
            if (!cur)
            {
                cout << "empty\n";
                continue;
            }
            while (cur)
            {
                cout << "[Roll:" << cur->roll << ", Name:" << cur->name << ", Grade:" << cur->grade << "]";
                if (cur->next) cout << " -> ";
                cur = cur->next;
            }
            cout << "\n";
        }
        cout << "-------------------------\n";
    }
};

int main()
{
    int buckets;
    cout << "Enter number of buckets for directory: ";
    cin >> buckets;
    if (buckets <= 0) buckets = 10;
    StudentDirectory dir(buckets);

    int choice;
    do
    {
        cout << "\nMenu:\n";
        cout << "1) Add / Update Student Record\n";
        cout << "2) Search Student by Roll\n";
        cout << "3) Delete Student by Roll\n";
        cout << "4) Display All Records\n";
        cout << "5) Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        switch (choice)
        {
        case 1:
        {
            int roll;
            string name;
            float grade;
            cout << "Enter Roll Number: ";
            cin >> roll;
            cout << "Enter Name: ";
            cin >> ws;
            getline(cin, name);
            cout << "Enter Grade: ";
            cin >> grade;
            dir.addRecord(roll, name, grade);
            break;
        }
        case 2:
        {
            int roll;
            cout << "Enter Roll Number to search: ";
            cin >> roll;
            RecordNode *res = dir.findRecord(roll);
            if (res)
                cout << "Found -> Roll: " << res->roll << ", Name: " << res->name << ", Grade: " << res->grade << "\n";
            else
                cout << "Student with roll " << roll << " not found.\n";
            break;
        }
        case 3:
        {
            int roll;
            cout << "Enter Roll Number to delete: ";
            cin >> roll;
            dir.deleteRecord(roll);
            break;
        }
        case 4:
            dir.displayDirectory();
            break;
        case 5:
            cout << "Exiting program.\n";
            break;
        default:
            cout << "Invalid choice.\n";
        }
    } while (choice != 5);

    return 0;
}
```

# Output 

Enter number of buckets for directory: 5  

Menu:  
1) Add / Update Student Record  
2) Search Student by Roll  
3) Delete Student by Roll  
4) Display All Records  
5) Exit  
Enter choice: 1  
Enter Roll Number: 101  
Enter Name: Farooq  
Enter Grade: 8.7  
Record added in bucket 1.  

Menu:  
1) Add / Update Student Record  
2) Search Student by Roll  
3) Delete Student by Roll  
4) Display All Records  
5) Exit  
Enter choice: 1  
Enter Roll Number: 106  
Enter Name: Fija  
Enter Grade: 9.1  
Record appended in bucket 1.  

Menu:  
1) Add / Update Student Record  
2) Search Student by Roll  
3) Delete Student by Roll  
4) Display All Records  
5) Exit  
Enter choice: 1  
Enter Roll Number: 109  
Enter Name: Rohan  
Enter Grade: 8.9  
Record appended in bucket 1.  

Menu:  
1) Add / Update Student Record  
2) Search Student by Roll  
3) Delete Student by Roll  
4) Display All Records  
5) Exit  
Enter choice: 4  

--- Student Directory ---  
Bucket 0: empty  
Bucket 1: [Roll:101, Name:Farooq, Grade:8.7] -> [Roll:106, Name:Fija, Grade:9.1] -> [Roll:109, Name:Rohan, Grade:8.9]  
Bucket 2: empty  
Bucket 3: empty  
Bucket 4: empty  
-------------------------  

Menu:  
1) Add / Update Student Record  
2) Search Student by Roll  
3) Delete Student by Roll  
4) Display All Records  
5) Exit  
Enter choice: 2  
Enter Roll Number to search: 101  
Found -> Roll: 101, Name: Farooq, Grade: 8.7  

Menu:  
1) Add / Update Student Record  
2) Search Student by Roll  
3) Delete Student by Roll  
4) Display All Records  
5) Exit  
Enter choice: 3  
Enter Roll Number to delete: 106  
Record with roll 106 deleted from bucket 1.  

Menu:  
1) Add / Update Student Record  
2) Search Student by Roll  
3) Delete Student by Roll  
4) Display All Records  
5) Exit  
Enter choice: 4  

--- Student Directory ---  
Bucket 0: empty  
Bucket 1: [Roll:101, Name:Farooq, Grade:8.7] -> [Roll:109, Name:Rohan, Grade:8.9]  
Bucket 2: empty  
Bucket 3: empty  
Bucket 4: empty  
-------------------------  

Menu:  
1) Add / Update Student Record  
2) Search Student by Roll  
3) Delete Student by Roll  
4) Display All Records  
5) Exit  
Enter choice: 5  
Exiting program.   