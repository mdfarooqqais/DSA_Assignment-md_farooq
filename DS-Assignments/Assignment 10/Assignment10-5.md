 

# Assignment 10.5

# Title : Write a program to implement Dijkstra’s algorithm to find the shortest distance between two nodes of a user defined graph using Adjacency List representation.

# Thoery :

In graph theory, the shortest path problem is used to find the minimum distance between two vertices in a weighted graph. This problem is widely used in navigation systems, network routing, and communication systems.

Dijkstra’s Algorithm is a greedy algorithm that finds the shortest path from a single source vertex to all other vertices in a graph with non-negative edge weights. The algorithm works by repeatedly selecting the vertex with the minimum known distance and relaxing all its adjacent edges.

An Adjacency List representation stores, for each vertex, a list of connected vertices along with the weight of the connecting edge. This representation is efficient in terms of memory usage, especially for sparse graphs.

A priority queue (min-heap) is used to efficiently select the next vertex with the smallest distance. An array is maintained to store the shortest distance from the source to each vertex, and another array stores the parent of each vertex to reconstruct the shortest path.

This approach helps in understanding greedy algorithms, priority queues, and shortest path computation in graphs.

# CODE 
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <stack>
#include <climits>
using namespace std;

typedef pair<int,int> pii;

void computeShortestPaths(const vector<vector<pii>>& graph, int vertices, int source,
                          vector<int>& dist, vector<int>& parent) {
    dist.assign(vertices, INT_MAX);
    parent.assign(vertices, -1);
    priority_queue<pii, vector<pii>, greater<pii>> pq;
    dist[source] = 0;
    pq.push({0, source});

    while (!pq.empty()) {
        int u = pq.top().second;
        int d = pq.top().first;
        pq.pop();
        if (d > dist[u]) continue;
        for (auto &edge : graph[u]) {
            int v = edge.first;
            int w = edge.second;
            if (dist[u] != INT_MAX && dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                parent[v] = u;
                pq.push({dist[v], v});
            }
        }
    }
}

void printShortestPath(const vector<int>& parent, int dest) {
    if (parent[dest] == -1) {
        cout << "No path to destination." << endl;
        return;
    }
    stack<int> path;
    for (int v = dest; v != -1; v = parent[v]) path.push(v);
    cout << "Shortest Path: ";
    while (!path.empty()) {
        cout << path.top();
        path.pop();
        if (!path.empty()) cout << " -> ";
    }
    cout << endl;
}

int main() {
    int V, E;
    cout << "Enter number of vertices: ";
    cin >> V;
    cout << "Enter number of edges: ";
    cin >> E;

    vector<vector<pii>> adj(V);
    cout << "Enter edges (u v weight) :\n";
    for (int i = 0; i < E; ++i) {
        int u, v, w;
        cin >> u >> v >> w;
        adj[u].push_back({v, w});
    }

    int src, dst;
    cout << "Enter source vertex: ";
    cin >> src;
    cout << "Enter destination vertex: ";
    cin >> dst;

    vector<int> dist, parent;
    computeShortestPaths(adj, V, src, dist, parent);

    if (dist[dst] == INT_MAX) {
        cout << "No path exists from " << src << " to " << dst << "." << endl;
    } else {
        cout << "Shortest distance from " << src << " to " << dst << " = " << dist[dst] << endl;
        printShortestPath(parent, dst);
    }

    return 0;
}
```
# Output 

Enter number of vertices: 6  
Enter number of edges: 9  
Enter edges (u v weight) :  
0 1 2  
0 2 4  
1 2 1  
1 3 7  
2 4 3  
3 4 1  
2 5 4  
4 5 2  
3 5 6  
Enter source vertex: 0  
Enter destination vertex: 5  

Shortest distance from 0 to 5 = 9  
Shortest Path: 0 -> 1 -> 2 -> 4 -> 5  
