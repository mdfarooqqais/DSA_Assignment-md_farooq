# Assignment 9.3

# Title : Write a program to implement Kruskal’s algorithm to find the Minimum Spanning Tree (MST) of a user defined graph using Adjacency List representation.

# Thoery :
A Minimum Spanning Tree (MST) of a weighted, connected, and undirected graph is a subset of edges that connects all the vertices together without forming any cycles and with the minimum possible total edge weight. Minimum Spanning Trees are commonly used in network design, cable laying, and cost optimization problems.

Kruskal’s Algorithm is a greedy algorithm used to find the Minimum Spanning Tree of a graph. It works by sorting all the edges of the graph in increasing order of their weights and then adding edges one by one to the MST, ensuring that no cycles are formed.

An Adjacency List representation stores, for each vertex, a list of its adjacent vertices along with the corresponding edge weights. This representation is efficient for sparse graphs.

To detect and avoid cycles while selecting edges, Disjoint Set (Union-Find) data structure is used. It helps in keeping track of connected components and ensures that only safe edges are added to the MST.

This approach helps in understanding greedy algorithms, edge sorting, and the use of disjoint set data structures in graph algorithms.

# CODE 
```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct GraphEdge {
    int u;
    int v;
    int w;
    bool operator<(const GraphEdge &other) const { 
        return w < other.w; 
    }
};

class DisjointSet {
    vector<int> parent;
    vector<int> rankv;
public:
    DisjointSet(int n) {
        parent.resize(n);
        rankv.assign(n, 0);
        for (int i = 0; i < n; ++i)
            parent[i] = i;
    }

    int findSet(int x) {
        if (parent[x] != x)
            parent[x] = findSet(parent[x]);
        return parent[x];
    }

    void uniteSets(int a, int b) {
        int pa = findSet(a);
        int pb = findSet(b);
        if (pa == pb) return;

        if (rankv[pa] < rankv[pb])
            parent[pa] = pb;
        else if (rankv[pa] > rankv[pb])
            parent[pb] = pa;
        else {
            parent[pb] = pa;
            rankv[pa]++;
        }
    }
};

void computeKruskalMST(const vector<vector<pair<int,int>>> &adjList, int vertices) {
    vector<GraphEdge> edges;

    for (int u = 0; u < vertices; ++u) {
        for (auto &p : adjList[u]) {
            int v = p.first;
            int w = p.second;
            if (u < v)
                edges.push_back({u, v, w});
        }
    }

    sort(edges.begin(), edges.end());

    DisjointSet ds(vertices);
    int totalWeight = 0;

    cout << "MST Edges (u - v : weight):\n";
    for (auto &e : edges) {
        if (ds.findSet(e.u) != ds.findSet(e.v)) {
            ds.uniteSets(e.u, e.v);
            cout << e.u << " - " << e.v << " : " << e.w << '\n';
            totalWeight += e.w;
        }
    }

    cout << "Total MST Weight: " << totalWeight << '\n';
}

int main() {
    int vertexCount, edgeCount;
    cout << "Number of vertices: ";
    cin >> vertexCount;
    cout << "Number of edges: ";
    cin >> edgeCount;

    vector<vector<pair<int,int>>> adjList(vertexCount);

    cout << "Enter edges as: u v weight (vertices numbered 0 to " 
         << vertexCount - 1 << ")\n";
    for (int i = 0; i < edgeCount; ++i) {
        int u, v, w;
        cin >> u >> v >> w;
        adjList[u].push_back({v, w});
        adjList[v].push_back({u, w});
    }

    computeKruskalMST(adjList, vertexCount);
    return 0;
}
```
# Output 

Number of vertices: 5  
Number of edges: 6  
Enter edges as: u v weight (vertices numbered 0 to 4)  
0 1 2  
0 3 6  
1 2 3  
1 3 8  
1 4 5  
2 4 7  

MST Edges (u - v : weight):  
0 - 1 : 2  
1 - 2 : 3  
1 - 4 : 5  
0 - 3 : 6  
Total MST Weight: 16  
