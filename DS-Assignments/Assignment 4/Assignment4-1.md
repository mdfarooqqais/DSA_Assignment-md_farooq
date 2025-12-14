
# Assignment NO 4.1

# Title : Write a C++ program to implement a Set using a Generalized Linked List (GLL). For example:
Let S = { p, q, {r, s, t, {}, {u, v}, w, x, {y, z}, a1, b1} }
Store this structure using a Generalized Linked List and display the elements in correct set notation format.


# Theory: Generalized Linked List (GLL)

A Generalized Linked List (GLL) is a data structure used to represent collections in which elements can be either atomic values or sub-lists. Unlike a simple linked list that stores linear data, a GLL supports nested and hierarchical structures, making it suitable for representing complex sets.

Each node in a generalized linked list contains a tag field to indicate whether the node stores a data element or a link to another list. If the node represents a sub-list, it points to another generalized linked list, enabling recursive data representation.

GLLs are widely used to implement sets with nested elements, where a set may contain individual elements as well as other sets. The structure preserves the logical grouping and hierarchy of elements.

Traversal of a generalized linked list is performed recursively to access all elements and sub-elements in proper order. This allows correct display of the set in standard mathematical notation.

Thus, generalized linked lists provide a flexible and efficient way to store and manipulate complex set structures that cannot be handled using arrays or simple linked lists.

# CODE

```cpp
#include <iostream>
#include <string>
using namespace std;

struct Node {
    int tag;
    string data;     
    Node* down;      
    Node* next;      

    Node(int t = 0, const string &d = "") : tag(t), data(d), down(NULL), next(NULL) {}
};

Node* createAtom(const string &val) {
    return new Node(0, val);
}

Node* createSublist(Node* downHead) {
    Node* n = new Node(1);
    n->down = downHead;
    return n;
}

Node* appendSibling(Node* head, Node* nd) {
    if (!head) return nd;
    Node* p = head;
    while (p->next) p = p->next;
    p->next = nd;
    return head;
}

void displayList(Node* head) {
    cout << "{";
    Node* p = head;
    while (p) {
        if (p->tag == 0) {
            cout << p->data;
        } else {
            if (p->down == NULL) {
                cout << "{}";
            } else {
                displayList(p->down);
            }
        }
        if (p->next) cout << ", ";
        p = p->next;
    }
    cout << "}";
}
void freeList(Node* head) {
    Node* p = head;
    while (p) {
        Node* nxt = p->next;
        if (p->tag == 1 && p->down) {
            freeList(p->down);
        }
        delete p;
        p = nxt;
    }
}

Node* buildSampleSet() {
    
    Node* uv = createAtom("u");
    uv = appendSibling(uv, createAtom("v"));
    Node* sub_uv = createSublist(uv);

    Node* yz = createAtom("y");
    yz = appendSibling(yz, createAtom("z"));
    Node* sub_yz = createSublist(yz);

    Node* emptySet = NULL;
    Node* sub_empty = createSublist(emptySet); 

    Node* inner = createAtom("r");
    inner = appendSibling(inner, createAtom("s"));
    inner = appendSibling(inner, createAtom("t"));
    inner = appendSibling(inner, sub_empty);
    inner = appendSibling(inner, sub_uv);
    inner = appendSibling(inner, createAtom("w"));
    inner = appendSibling(inner, createAtom("x"));
    inner = appendSibling(inner, sub_yz);
    inner = appendSibling(inner, createAtom("a1"));
    inner = appendSibling(inner, createAtom("b1"));

    Node* sub_inner = createSublist(inner);

    Node* S = createAtom("p");
    S = appendSibling(S, createAtom("q"));
    S = appendSibling(S, sub_inner);

    return S;
}

int main() {

    Node* S = buildSampleSet();

    cout << "S = ";
    displayList(S);
    cout << endl;

    freeList(S);

    return 0;
}
```

# Output:

S = {p, q, {r, s, t, {}, {u, v}, w, x, {y, z}, a1, b1}}
