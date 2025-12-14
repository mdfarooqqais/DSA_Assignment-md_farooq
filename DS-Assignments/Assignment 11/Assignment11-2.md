
# Assignment 11.2

# Title : Implement collision handling using separate chaining.

# Thoery :

A hash table is a data structure that stores key–value pairs and provides efficient insertion, deletion, and searching operations. A hash function is used to compute an index where the data is stored.

When multiple keys produce the same hash index, a collision occurs. One effective method to handle collisions is Separate Chaining. In this technique, each index of the hash table contains a linked list (or chain) of elements that hash to the same index.

In separate chaining, all keys with the same hash value are stored in the same bucket using a linked list. This approach avoids probing and allows multiple elements to exist at the same index. It is efficient when the hash table size is small and collisions are frequent.

This method helps in understanding linked lists, hashing techniques, and efficient collision handling strategies.

# CODE 
```cpp
#include <iostream>
#include <vector>
#include <list>
#include <utility>
using namespace std;

class ChainedHashMap {
    int capacity;
    vector<list<pair<int,int>>> buckets;

    int hashIndex(int key) const {
        return key % capacity;
    }

public:
    ChainedHashMap(int cap) : capacity(cap), buckets(cap) {}

    void insertOrUpdate(int key, int value) {
        int idx = hashIndex(key);
        for (auto &p : buckets[idx]) {
            if (p.first == key) {
                p.second = value;
                cout << "Updated (" << key << ", " << value << ") at bucket " << idx << ".\n";
                return;
            }
        }
        buckets[idx].push_back({key, value});
        cout << "Inserted (" << key << ", " << value << ") at bucket " << idx << ".\n";
    }

    bool search(int key, int &valueOut) const {
        int idx = hashIndex(key);
        for (auto &p : buckets[idx]) {
            if (p.first == key) {
                valueOut = p.second;
                return true;
            }
        }
        return false;
    }

    void removeKey(int key) {
        int idx = hashIndex(key);
        for (auto it = buckets[idx].begin(); it != buckets[idx].end(); ++it) {
            if (it->first == key) {
                buckets[idx].erase(it);
                cout << "Deleted key " << key << " from bucket " << idx << ".\n";
                return;
            }
        }
        cout << "Key " << key << " not found.\n";
    }

    void displayTable() const {
        cout << "Hash Table Contents (by bucket):\n";
        for (int i = 0; i < capacity; ++i) {
            cout << "Bucket " << i << ": ";
            if (buckets[i].empty()) {
                cout << "<empty>";
            } else {
                for (auto &p : buckets[i]) {
                    cout << "(" << p.first << ", " << p.second << ") ";
                }
            }
            cout << "\n";
        }
    }
};

int main() {
    int size;
    cout << "Enter hash table capacity: ";
    cin >> size;
    if (size <= 0) return 0;

    ChainedHashMap map(size);

    int choice = 0;
    do {
        cout << "\n--- Hash Map Menu ---\n";
        cout << "1) Display table\n";
        cout << "2) Insert / Update key\n";
        cout << "3) Search key\n";
        cout << "4) Delete key\n";
        cout << "5) Exit\n";
        cout << "Choose an option: ";
        cin >> choice;

        switch (choice) {
            case 1:
                map.displayTable();
                break;
            case 2: {
                int key, value;
                cout << "Enter key and value to insert: ";
                cin >> key >> value;
                map.insertOrUpdate(key, value);
                break;
            }
            case 3: {
                int key;
                cout << "Enter key to search: ";
                cin >> key;
                int val;
                if (map.search(key, val))
                    cout << "Found key " << key << " with value " << val << ".\n";
                else
                    cout << "Key " << key << " not found.\n";
                break;
            }
            case 4: {
                int key;
                cout << "Enter key to delete: ";
                cin >> key;
                map.removeKey(key);
                break;
            }
            case 5:
                cout << "Exiting program.\n";
                break;
            default:
                cout << "Invalid option. Try again.\n";
        }
    } while (choice != 5);

    return 0;
}
```

# Output 

Enter hash table capacity: 5  

--- Hash Map Menu ---  
1) Display table  
2) Insert / Update key  
3) Search key  
4) Delete key  
5) Exit  
Choose an option: 2  
Enter key and value to insert: 10 100  
Inserted (10, 100) at bucket 0.  

--- Hash Map Menu ---  
1) Display table  
2) Insert / Update key  
3) Search key  
4) Delete key  
5) Exit  
Choose an option: 2  
Enter key and value to insert: 15 150  
Inserted (15, 150) at bucket 0.  

--- Hash Map Menu ---  
1) Display table  
2) Insert / Update key  
3) Search key  
4) Delete key  
5) Exit  
Choose an option: 2  
Enter key and value to insert: 7 70  
Inserted (7, 70) at bucket 2.  

--- Hash Map Menu ---  
1) Display table  
2) Insert / Update key  
3) Search key  
4) Delete key  
5) Exit  
Choose an option: 1  
Hash Table Contents (by bucket):  
Bucket 0: (10, 100) (15, 150)  
Bucket 1: <empty>  
Bucket 2: (7, 70)  
Bucket 3: <empty>  
Bucket 4: <empty>  

--- Hash Map Menu ---  
1) Display table  
2) Insert / Update key  
3) Search key  
4) Delete key  
5) Exit  
Choose an option: 3  
Enter key to search: 15  
Found key 15 with value 150.  

--- Hash Map Menu ---  
1) Display table  
2) Insert / Update key  
3) Search key  
4) Delete key  
5) Exit  
Choose an option: 4  
Enter key to delete: 10  
Deleted key 10 from bucket 0.  

--- Hash Map Menu ---  
1) Display table  
2) Insert / Update key  
3) Search key  
4) Delete key  
5) Exit  
Choose an option: 1  
Hash Table Contents (by bucket):  
Bucket 0: (15, 150)  
Bucket 1: <empty>  
Bucket 2: (7, 70)  
Bucket 3: <empty>  
Bucket 4: <empty>  

--- Hash Map Menu ---  
1) Display table  
2) Insert / Update key  
3) Search key  
4) Delete key  
5) Exit  
Choose an option: 5  
Exiting program.  