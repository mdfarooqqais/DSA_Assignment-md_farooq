# Assignment 10.4

# Title : Write a program to implement Kruskal’s algorithm to find the Minimum Spanning Tree (MST) of a user defined graph using Adjacency List representation.

# Thoery :

A Minimum Spanning Tree (MST) of a connected, weighted, and undirected graph is a set of edges that connects all vertices with the minimum possible total edge weight and without forming any cycles. Minimum spanning trees are widely used in network design, cable routing, and optimization problems.

Kruskal’s Algorithm is a greedy algorithm used to find the Minimum Spanning Tree. It works by sorting all the edges of the graph in ascending order of their weights and then selecting edges one by one. An edge is included in the MST only if it does not form a cycle with the already selected edges.

An Adjacency List representation stores for each vertex a list of its neighboring vertices along with the corresponding edge weights. This representation is memory efficient and suitable for sparse graphs.

To efficiently detect cycles during edge selection, the Disjoint Set (Union-Find) data structure is used. It keeps track of connected components and ensures that no cycles are formed while constructing the MST.

This implementation helps in understanding greedy algorithms, adjacency list representation, and disjoint set operations.

# CODE 
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct EdgeRecord {
    int u, v;
    int weight;
    bool operator<(const EdgeRecord &other) const {
        return weight < other.weight;
    }
};

class DisjointSet {
    vector<int> parent;
    vector<int> rankv;
public:
    DisjointSet(int n) : parent(n), rankv(n, 0) {
        for (int i = 0; i < n; ++i) parent[i] = i;
    }
    int findSet(int x) {
        if (parent[x] != x) parent[x] = findSet(parent[x]);
        return parent[x];
    }
    void unionSet(int x, int y) {
        int rx = findSet(x), ry = findSet(y);
        if (rx == ry) return;
        if (rankv[rx] < rankv[ry]) parent[rx] = ry;
        else if (rankv[rx] > rankv[ry]) parent[ry] = rx;
        else { parent[ry] = rx; rankv[rx]++; }
    }
};

int main() {
    int vertices, edges;
    cout << "Enter number of vertices: ";
    cin >> vertices;
    cout << "Enter number of edges: ";
    cin >> edges;

    vector<vector<pair<int,int>>> adj(vertices);
    cout << "Enter edges in format: u v weight (0-based vertex indices)\n";
    for (int i = 0; i < edges; ++i) {
        int u, v, w;
        cin >> u >> v >> w;
        if (u < 0 || u >= vertices || v < 0 || v >= vertices) {
            cout << "Invalid vertex index, skipping this edge.\n";
            --i;
            continue;
        }
        adj[u].push_back({v, w});
        adj[v].push_back({u, w});
    }

    vector<EdgeRecord> allEdges;
    for (int u = 0; u < vertices; ++u) {
        for (auto &p : adj[u]) {
            int v = p.first, w = p.second;
            if (u < v) allEdges.push_back({u, v, w});
        }
    }

    sort(allEdges.begin(), allEdges.end());

    DisjointSet ds(vertices);
    vector<EdgeRecord> mst;
    int totalWeight = 0;

    for (auto &e : allEdges) {
        if (ds.findSet(e.u) != ds.findSet(e.v)) {
            ds.unionSet(e.u, e.v);
            mst.push_back(e);
            totalWeight += e.weight;
        }
    }

    cout << "\nEdges in Minimum Spanning Tree:\n";
    for (auto &e : mst) {
        cout << e.u << " - " << e.v << "  weight: " << e.weight << "\n";
    }
    cout << "Total weight of MST: " << totalWeight << "\n";

    return 0;
}
```

# Output 

Enter number of vertices: 6  
Enter number of edges: 9  
Enter edges in format: u v weight (0-based vertex indices)  
0 1 2  
0 3 6  
1 2 3  
1 3 8  
1 4 5  
2 4 7  
2 5 4  
3 5 9  
4 5 1  

Edges in Minimum Spanning Tree:  
4 - 5  weight: 1  
0 - 1  weight: 2  
1 - 2  weight: 3  
2 - 5  weight: 4  
0 - 3  weight: 6  

Total weight of MST: 16  

