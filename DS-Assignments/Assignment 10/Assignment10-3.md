# Assignment 10.3

# Title : Write a program to implement Prim’s algorithm to find the Minimum Spanning Tree (MST) of a user defined graph using Adjacency List representation.

# Thoery :

A Minimum Spanning Tree (MST) of a connected, weighted, and undirected graph is a subset of edges that connects all the vertices with the minimum possible total edge weight and without forming any cycles.

Prim’s Algorithm is a greedy algorithm used to find the Minimum Spanning Tree of a graph. It starts from a selected source vertex and gradually expands the MST by adding the smallest weight edge that connects a vertex inside the MST to a vertex outside the MST.

An Adjacency List representation is used to store the graph efficiently, where each vertex maintains a list of its adjacent vertices along with the corresponding edge weights. This representation is especially suitable for sparse graphs.

A priority queue (min-heap) is used in Prim’s algorithm to efficiently select the edge with the minimum weight at each step. Arrays are maintained to track the minimum edge weight (key), parent of each vertex in the MST, and whether a vertex is already included in the MST.

This implementation helps in understanding greedy algorithms, priority queues, and adjacency list–based graph representations.


# CODE 
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <climits>

using namespace std;

using Edge = pair<int,int>; 
using PQItem = pair<int,int>; 

class Graph {
    int V;
    vector<vector<Edge>> adj;
public:
    Graph(int vertices) : V(vertices), adj(vertices) {}

    void addEdge(int u, int v, int w) {
        adj[u].push_back({v,w});
        adj[v].push_back({u,w}); 
    }

    void primMST(int src, vector<int>& parent, vector<int>& key) {
        parent.assign(V, -1);
        key.assign(V, INT_MAX);
        vector<bool> inMST(V,false);

        priority_queue<PQItem, vector<PQItem>, greater<PQItem>> pq;
        key[src] = 0;
        pq.push({0, src});

        while (!pq.empty()) {
            int u = pq.top().second;
            pq.pop();
            if (inMST[u]) continue;
            inMST[u] = true;

            for (auto &e : adj[u]) {
                int v = e.first;
                int w = e.second;
                if (!inMST[v] && w < key[v]) {
                    key[v] = w;
                    parent[v] = u;
                    pq.push({key[v], v});
                }
            }
        }
    }

    void printMST(const vector<int>& parent, const vector<int>& key) {
        cout << "Minimum Spanning Tree Edges:\n";
        int total = 0;
        for (int v = 0; v < V; ++v) {
            if (parent[v] != -1) {
                cout << parent[v] << " - " << v << " (weight: " << key[v] << ")\n";
                total += key[v];
            }
        }
        cout << "Total Weight of MST: " << total << "\n";
    }
};

int main() {
    int n, e;
    cout << "Enter number of vertices: ";
    cin >> n;
    cout << "Enter number of edges: ";
    cin >> e;

    Graph g(n);
    cout << "Enter edges as: u v weight  (vertices numbered 0 to " << n-1 << ")\n";
    for (int i = 0; i < e; ++i) {
        int u,v,w;
        cin >> u >> v >> w;
        g.addEdge(u,v,w);
    }

    int start;
    cout << "Enter starting vertex for Prim's algorithm: ";
    cin >> start;

    vector<int> parent, key;
    g.primMST(start, parent, key);
    g.printMST(parent, key);

    return 0;
}
```
# Output 

Enter number of vertices: 5  
Enter number of edges: 6  
Enter edges as: u v weight  (vertices numbered 0 to 4)  
0 1 2  
0 3 6  
1 2 3  
1 3 8  
1 4 5  
2 4 7  
Enter starting vertex for Prim's algorithm: 0  

Minimum Spanning Tree Edges:  
0 - 1 (weight: 2)  
1 - 2 (weight: 3)  
1 - 4 (weight: 5)  
0 - 3 (weight: 6)  
Total Weight of MST: 16 