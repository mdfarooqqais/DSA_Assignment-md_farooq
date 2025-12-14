# Assignment 9.2

# Title : Write a program to implement Prim’s algorithm to find the Minimum Spanning Tree (MST) of a user defined graph using Adjacency List representation.

# Thoery :

A Minimum Spanning Tree (MST)** of a weighted, connected, and undirected graph is a subset of edges that connects all the vertices together without any cycles and with the minimum possible total edge weight. MSTs are widely used in network design, routing, and optimization problems.

Prim’s Algorithm is a greedy algorithm used to find the Minimum Spanning Tree of a graph. It starts from a chosen vertex and grows the MST by repeatedly selecting the smallest weight edge that connects a vertex inside the MST to a vertex outside the MST.

An Adjacency List is an efficient way to represent a graph, especially when the graph is sparse. In this representation, each vertex maintains a list of its neighboring vertices along with the corresponding edge weights.

In Prim’s algorithm, a priority queue (min-heap) is used to efficiently select the next edge with the minimum weight. Arrays are maintained to store the minimum edge weight for each vertex, the parent of each vertex in the MST, and whether a vertex is already included in the MST.

This approach helps in understanding greedy algorithms, priority queues, and graph traversal techniques using adjacency lists.

# Algorithm :

# CODE 
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <climits>
using namespace std;

typedef pair<int,int> EdgeWeight; // (vertex, weight)

void computePrimMST(const vector<vector<EdgeWeight>>& graph, int vertexCount, int startVertex) {
    vector<int> minEdge(vertexCount, INT_MAX);
    vector<int> parent(vertexCount, -1);
    vector<bool> inMST(vertexCount, false);
    priority_queue<EdgeWeight, vector<EdgeWeight>, greater<EdgeWeight>> heap;

    minEdge[startVertex] = 0;
    heap.push({0, startVertex});
    int total = 0;

    while (!heap.empty()) {
        int u = heap.top().second;
        heap.pop();

        if (inMST[u]) continue;
        inMST[u] = true;
        total += minEdge[u];

        for (auto &nbr : graph[u]) {
            int v = nbr.first;
            int w = nbr.second;
            if (!inMST[v] && w < minEdge[v]) {
                minEdge[v] = w;
                parent[v] = u;
                heap.push({minEdge[v], v});
            }
        }
    }

    cout << "MST Edges (parent - vertex : weight)\n";
    for (int v = 0; v < vertexCount; ++v) {
        if (parent[v] != -1) {
            cout << parent[v] << " - " << v << " : " << minEdge[v] << '\n';
        }
    }
    cout << "Total MST weight: " << total << '\n';
}

int main() {
    int vertices, edges;
    cout << "Number of vertices: ";
    cin >> vertices;
    cout << "Number of edges: ";
    cin >> edges;

    vector<vector<EdgeWeight>> graph(vertices);

    cout << "Enter edges as: u v weight (vertices numbered 0 to " << vertices-1 << ")\n";
    for (int i = 0; i < edges; ++i) {
        int u, v, w;
        cin >> u >> v >> w;
        graph[u].push_back({v, w});
        graph[v].push_back({u, w});
    }

    int start;
    cout << "Choose start vertex for Prim's algorithm: ";
    cin >> start;

    computePrimMST(graph, vertices, start);

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
Choose start vertex for Prim's algorithm: 0  

MST Edges (parent - vertex : weight)  
0 - 1 : 2  
1 - 2 : 3  
1 - 4 : 5  
0 - 3 : 6  
Total MST weight: 16  
