# Assignment 10.1

# Title : Write a program to implement Kruskal’s algorithm to find the Minimum Spanning Tree (MST) of a user defined graph using Adjacency Matrix representation.

# Thoery :

A Minimum Spanning Tree (MST) of a connected, weighted, and undirected graph is a subset of edges that connects all the vertices together without forming any cycles and with the minimum possible total edge weight. MSTs are widely used in network design, circuit design, and optimization problems.

Kruskal’s Algorithm is a greedy algorithm used to find the Minimum Spanning Tree of a graph. It works by sorting all the edges in ascending order of their weights and then selecting edges one by one such that no cycles are formed.

An Adjacency Matrix is a two-dimensional array representation of a graph where each cell represents the weight of the edge between two vertices. A value of zero indicates that no edge exists between the vertices.

To efficiently detect cycles while selecting edges, the Disjoint Set (Union-Find) data structure is used. It keeps track of connected components and ensures that adding an edge does not create a cycle.

This implementation helps in understanding greedy algorithms, matrix-based graph representation, and union-find operations.

# CODE 
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct EdgeRecord {
    int u;
    int v;
    int w;
};

bool compareByWeight(const EdgeRecord &a, const EdgeRecord &b) {
    return a.w < b.w;
}

int findParent(vector<int> &parent, int x) {
    if (parent[x] != x) parent[x] = findParent(parent, parent[x]);
    return parent[x];
}

void unionSets(vector<int> &parent, vector<int> &rankk, int x, int y) {
    int rx = findParent(parent, x);
    int ry = findParent(parent, y);
    if (rx == ry) return;
    if (rankk[rx] < rankk[ry]) parent[rx] = ry;
    else if (rankk[rx] > rankk[ry]) parent[ry] = rx;
    else {
        parent[ry] = rx;
        rankk[rx]++;
    }
}

int main() {
    int vertices, edgesCount;
    cout << "Number of vertices: ";
    cin >> vertices;
    cout << "Number of edges: ";
    cin >> edgesCount;

    vector<vector<int>> adjMatrix(vertices, vector<int>(vertices, 0));
    cout << "Enter each edge as: u v weight (vertices indexed 0 to " << vertices - 1 << ")\n";
    for (int i = 0; i < edgesCount; ++i) {
        int u, v, wt;
        cin >> u >> v >> wt;
        if (u >= 0 && u < vertices && v >= 0 && v < vertices) {
            adjMatrix[u][v] = wt;
            adjMatrix[v][u] = wt;
        }
    }

    vector<EdgeRecord> edgeList;
    for (int i = 0; i < vertices; ++i) {
        for (int j = i + 1; j < vertices; ++j) {
            if (adjMatrix[i][j] != 0)
                edgeList.push_back({i, j, adjMatrix[i][j]});
        }
    }

    sort(edgeList.begin(), edgeList.end(), compareByWeight);

    vector<int> parent(vertices), rankk(vertices, 0);
    for (int i = 0; i < vertices; ++i) parent[i] = i;

    vector<EdgeRecord> mst;
    int totalWeight = 0;
    for (const auto &e : edgeList) {
        int pu = findParent(parent, e.u);
        int pv = findParent(parent, e.v);
        if (pu != pv) {
            unionSets(parent, rankk, pu, pv);
            mst.push_back(e);
            totalWeight += e.w;
        }
    }

    cout << "Edges in Minimum Spanning Tree:\n";
    for (const auto &e : mst) {
        cout << e.u << " - " << e.v << " : " << e.w << "\n";
    }
    cout << "Total weight of MST: " << totalWeight << "\n";

    return 0;
}
```
# Output 

Number of vertices: 5  
Number of edges: 6  
Enter each edge as: u v weight (vertices indexed 0 to 4)  
0 1 2  
0 3 6  
1 2 3  
1 3 8  
1 4 5  
2 4 7  

Edges in Minimum Spanning Tree:  
0 - 1 : 2  
1 - 2 : 3  
1 - 4 : 5  
0 - 3 : 6  
Total weight of MST: 16  