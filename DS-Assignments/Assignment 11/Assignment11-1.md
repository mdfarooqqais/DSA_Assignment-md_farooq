# Assignment 11.1

# Title : Implement a hash table with collision resolution using linear probing.

# Thoery :

A hash table is a data structure that stores key–value pairs and provides fast insertion, deletion, and searching operations. It works by applying a hash function to a key to compute an index in an array where the value is stored.

When two different keys produce the same hash index, a collision occurs. One common technique to handle collisions is Linear Probing. In linear probing, if the computed index is already occupied, the algorithm searches sequentially for the next available slot in the table.

In linear probing, three states are usually maintained for each slot:
- EMPTY – slot has never been used
- OCCUPIED – slot currently stores a key–value pair
- DELETED – slot previously contained data but is now removed

Linear probing is simple to implement and efficient when the load factor of the hash table is low. It helps in understanding open addressing techniques and collision resolution strategies in hashing.

# CODE 
```cpp
#include <iostream>
#include <vector>
#include <utility>
using namespace std;

class HashMapLinearProbe {
    int capacity;
    vector<pair<int,int>> table;
    vector<int> slotState; // 0 = EMPTY, 1 = OCCUPIED, 2 = DELETED

    int hashIndex(int key) {
        return key % capacity;
    }

public:
    HashMapLinearProbe(int cap) : capacity(cap), table(cap), slotState(cap, 0) {}

    void put(int key, int value) {
        int idx = hashIndex(key);
        int start = idx;
        while (slotState[idx] == 1 && table[idx].first != key) {
            idx = (idx + 1) % capacity;
            if (idx == start) {
                cout << "Hash table full — cannot insert.\n";
                return;
            }
        }
        if (slotState[idx] == 1 && table[idx].first == key) {
            table[idx].second = value;
            cout << "Updated key " << key << " with value " << value << " at index " << idx << ".\n";
        } else {
            table[idx] = {key, value};
            slotState[idx] = 1;
            cout << "Inserted (" << key << ", " << value << ") at index " << idx << ".\n";
        }
    }

    bool find(int key, int &outValue) {
        int idx = hashIndex(key);
        int start = idx;
        while (slotState[idx] != 0) {
            if (slotState[idx] == 1 && table[idx].first == key) {
                outValue = table[idx].second;
                return true;
            }
            idx = (idx + 1) % capacity;
            if (idx == start) break;
        }
        return false;
    }

    void removeKey(int key) {
        int idx = hashIndex(key);
        int start = idx;
        while (slotState[idx] != 0) {
            if (slotState[idx] == 1 && table[idx].first == key) {
                slotState[idx] = 2;
                cout << "Removed key " << key << " from index " << idx << ".\n";
                return;
            }
            idx = (idx + 1) % capacity;
            if (idx == start) break;
        }
        cout << "Key " << key << " not found.\n";
    }

    void showTable() {
        cout << "Hash Table Status:\n";
        for (int i = 0; i < capacity; ++i) {
            cout << "Index " << i << ": ";
            if (slotState[i] == 1) {
                cout << "(" << table[i].first << ", " << table[i].second << ")";
            } else if (slotState[i] == 2) {
                cout << "<deleted>";
            } else {
                cout << "<empty>";
            }
            cout << "\n";
        }
    }
};

int main() {
    int size;
    cout << "Specify hash table capacity: ";
    cin >> size;
    HashMapLinearProbe map(size);

    int choice;
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
            map.showTable();
            break;
        case 2: {
            int k, v;
            cout << "Enter key and value to insert: ";
            cin >> k >> v;
            map.put(k, v);
            break;
        }
        case 3: {
            int k;
            cout << "Enter key to search: ";
            cin >> k;
            int val;
            if (map.find(k, val)) {
                cout << "Found key " << k << " with value " << val << ".\n";
            } else {
                cout << "Key " << k << " not found.\n";
            }
            break;
        }
        case 4: {
            int k;
            cout << "Enter key to delete: ";
            cin >> k;
            map.removeKey(k);
            break;
        }
        case 5:
            cout << "Exiting program.\n";
            break;
        default:
            cout << "Invalid option — try again.\n";
        }
    } while (choice != 5);

    return 0;
}
```
# Output 

Specify hash table capacity: 5  

--- Hash Map Menu ---  
1) Display table  
2) Insert / Update key  
3) Search key  
4) Delete key  
5) Exit  
Choose an option: 2  
Enter key and value to insert: 10 100  
Inserted (10, 100) at index 0.  

--- Hash Map Menu ---  
1) Display table  
2) Insert / Update key  
3) Search key  
4) Delete key  
5) Exit  
Choose an option: 2  
Enter key and value to insert: 15 150  
Inserted (15, 150) at index 1.  

--- Hash Map Menu ---  
1) Display table  
2) Insert / Update key  
3) Search key  
4) Delete key  
5) Exit  
Choose an option: 3  
Enter key to search: 10  
Found key 10 with value 100.  

--- Hash Map Menu ---  
1) Display table  
2) Insert / Update key  
3) Search key  
4) Delete key  
5) Exit  
Choose an option: 4  
Enter key to delete: 15  
Removed key 15 from index 1.  

--- Hash Map Menu ---  
1) Display table  
2) Insert / Update key  
3) Search key  
4) Delete key  
5) Exit  
Choose an option: 1  
Hash Table Status:  
Index 0: (10, 100)  
Index 1: <deleted>  
Index 2: <empty>  
Index 3: <empty>  
Index 4: <empty>  

--- Hash Map Menu ---  
1) Display table  
2) Insert / Update key  
3) Search key  
4) Delete key  
5) Exit  
Choose an option: 5  
Exiting program.  