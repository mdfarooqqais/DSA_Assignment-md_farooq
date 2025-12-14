# Assignment 11.3

# Title : Implement collision resolution using linked lists.

# Thoery :

A hash table is a data structure that stores key–value pairs and provides efficient operations such as insertion, searching, and deletion. A hash function is used to convert a key into an index of the hash table.

When two or more keys generate the same hash index, a collision occurs. One common and effective method to handle collisions is Separate Chaining using Linked Lists.

In this method, each index (bucket) of the hash table maintains a linked list. All elements that hash to the same index are stored in the corresponding linked list. This allows multiple elements to coexist at the same hash index without overwriting data.

In this program, string keys are used. The hash function computes the sum of ASCII values of characters in the string and applies the modulo operation with the number of buckets to determine the bucket index.

This technique improves flexibility, handles collisions efficiently, and helps in understanding dynamic memory allocation, pointers, and linked list traversal.

# CODE 
```cpp
#include <iostream>
#include <vector>
#include <string>

using namespace std;

struct BucketNode {
    string key;
    int value;
    BucketNode* next;
    BucketNode(const string& k, int v) : key(k), value(v), next(nullptr) {}
};

class ChainedHash {
    vector<BucketNode*> buckets;
    int bucketCount;

    int hashIndex(const string& key) {
        int sum = 0;
        for (char c : key) sum += static_cast<int>(c);
        return sum % bucketCount;
    }

public:
    ChainedHash(int size) : buckets(size, nullptr), bucketCount(size) {}

    ~ChainedHash() {
        for (int i = 0; i < bucketCount; ++i) {
            BucketNode* cur = buckets[i];
            while (cur) {
                BucketNode* nxt = cur->next;
                delete cur;
                cur = nxt;
            }
        }
    }

    void insertOrUpdate(const string& key, int value) {
        int idx = hashIndex(key);
        BucketNode* cur = buckets[idx];
        while (cur) {
            if (cur->key == key) {
                cur->value = value;
                cout << "Updated (" << key << ", " << value << ") at bucket " << idx << '\n';
                return;
            }
            cur = cur->next;
        }
        BucketNode* node = new BucketNode(key, value);
        node->next = buckets[idx];
        buckets[idx] = node;
        cout << "Inserted (" << key << ", " << value << ") at bucket " << idx << '\n';
    }

    bool search(const string& key, int &outValue) {
        int idx = hashIndex(key);
        BucketNode* cur = buckets[idx];
        while (cur) {
            if (cur->key == key) {
                outValue = cur->value;
                return true;
            }
            cur = cur->next;
        }
        return false;
    }

    void removeKey(const string& key) {
        int idx = hashIndex(key);
        BucketNode* cur = buckets[idx];
        BucketNode* prev = nullptr;
        while (cur) {
            if (cur->key == key) {
                if (prev) prev->next = cur->next;
                else buckets[idx] = cur->next;
                delete cur;
                cout << "Deleted key \"" << key << "\" from bucket " << idx << '\n';
                return;
            }
            prev = cur;
            cur = cur->next;
        }
        cout << "Key \"" << key << "\" not found\n";
    }

    void display() {
        cout << "Hash Table Contents:\n";
        for (int i = 0; i < bucketCount; ++i) {
            cout << "Bucket " << i << ": ";
            BucketNode* cur = buckets[i];
            if (!cur) {
                cout << "Empty\n";
                continue;
            }
            while (cur) {
                cout << "(" << cur->key << ", " << cur->value << ")";
                if (cur->next) cout << " -> ";
                cur = cur->next;
            }
            cout << '\n';
        }
    }
};

int main() {
    int size;
    cout << "Enter number of buckets for hash table: ";
    cin >> size;
    ChainedHash ht(size);

    int choice;
    do {
        cout << "\n--- Hash Table Menu ---\n";
        cout << "1. Insert/Update\n";
        cout << "2. Search\n";
        cout << "3. Delete\n";
        cout << "4. Display\n";
        cout << "5. Exit\n";
        cout << "Enter choice: ";
        cin >> choice;

        if (choice == 1) {
            string k; int v;
            cout << "Enter key and value: ";
            cin >> k >> v;
            ht.insertOrUpdate(k, v);
        } else if (choice == 2) {
            string k; int out;
            cout << "Enter key to search: ";
            cin >> k;
            if (ht.search(k, out))
                cout << "Found: (" << k << ", " << out << ")\n";
            else
                cout << "Key not found\n";
        } else if (choice == 3) {
            string k;
            cout << "Enter key to delete: ";
            cin >> k;
            ht.removeKey(k);
        } else if (choice == 4) {
            ht.display();
        } else if (choice == 5) {
            cout << "Exiting...\n";
        } else {
            cout << "Invalid choice\n";
        }
    } while (choice != 5);

    return 0;
}
```

# Output 

Enter number of buckets for hash table: 5  

--- Hash Table Menu ---  
1. Insert/Update  
2. Search  
3. Delete  
4. Display  
5. Exit  
Enter choice: 1  
Enter key and value: apple 100  
Inserted (apple, 100) at bucket 0  

--- Hash Table Menu ---  
1. Insert/Update  
2. Search  
3. Delete  
4. Display  
5. Exit  
Enter choice: 1  
Enter key and value: mango 200  
Inserted (mango, 200) at bucket 2  

--- Hash Table Menu ---  
1. Insert/Update  
2. Search  
3. Delete  
4. Display  
5. Exit  
Enter choice: 1  
Enter key and value: grape 150  
Inserted (grape, 150) at bucket 4  

--- Hash Table Menu ---  
1. Insert/Update  
2. Search  
3. Delete  
4. Display  
5. Exit  
Enter choice: 4  
Hash Table Contents:  
Bucket 0: (apple, 100)  
Bucket 1: Empty  
Bucket 2: (mango, 200)  
Bucket 3: Empty  
Bucket 4: (grape, 150)  

--- Hash Table Menu ---  
1. Insert/Update  
2. Search  
3. Delete  
4. Display  
5. Exit  
Enter choice: 2  
Enter key to search: mango  
Found: (mango, 200)  

--- Hash Table Menu ---  
1. Insert/Update  
2. Search  
3. Delete  
4. Display  
5. Exit  
Enter choice: 3  
Enter key to delete: apple  
Deleted key "apple" from bucket 0  

--- Hash Table Menu ---  
1. Insert/Update  
2. Search  
3. Delete  
4. Display  
5. Exit  
Enter choice: 4  
Hash Table Contents:  
Bucket 0: Empty  
Bucket 1: Empty  
Bucket 2: (mango, 200)  
Bucket 3: Empty  
Bucket 4: (grape, 150)  

--- Hash Table Menu ---  
1. Insert/Update  
2. Search  
3. Delete  
4. Display  
5. Exit  
Enter choice: 5  
Exiting...  
