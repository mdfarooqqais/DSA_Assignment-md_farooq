# Assignment 9.1

# Title : Write a program to accept a graph from a user and represent it using an Adjacency Matrix and perform BFS and DFS traversals on it.

# Thoery :

A graph is a non-linear data structure that consists of a set of vertices (nodes) and edges (connections between nodes). Graphs are widely used to represent real-world problems such as computer networks, social networks, maps, and routing algorithms.

An Adjacency Matrix is a way of representing a graph using a two-dimensional array. If there is an edge between two vertices, the corresponding matrix cell is marked as 1; otherwise, it is marked as 0. This representation is simple and allows easy checking of whether an edge exists between two nodes.

Breadth First Search (BFS) is a graph traversal technique that starts from a selected node and visits all its neighboring nodes first before moving to the next level. BFS uses a queue to maintain the order of traversal and ensures that nodes are visited level by level.

Depth First Search (DFS) is another graph traversal technique that starts from a selected node and explores as deep as possible along each branch before backtracking. DFS is commonly implemented using recursion and a visited array to avoid revisiting nodes.

These traversal techniques help in understanding graph connectivity, traversal order, and the application of auxiliary data structures such as queues, recursion, and visited arrays.

# CODE 
```cpp
#include <iostream>
#include <vector>
#include <queue>
using namespace std;

void runBFS(const vector<vector<int>>& matrix, int beginNode, int totalNodes) {
    vector<bool> visited(totalNodes, false);
    queue<int> q;
    q.push(beginNode);
    visited[beginNode] = true;
    cout << "BFS Output: ";
    while (!q.empty()) {
        int current = q.front();
        q.pop();
        cout << current << " ";
        for (int next = 0; next < totalNodes; next++) {
            if (matrix[current][next] && !visited[next]) {
                visited[next] = true;
                q.push(next);
            }
        }
    }
    cout << endl;
}

void depthFirst(const vector<vector<int>>& matrix, int node, vector<bool>& visited, int totalNodes) {
    visited[node] = true;
    cout << node << " ";
    for (int next = 0; next < totalNodes; next++) {
        if (matrix[node][next] && !visited[next]) {
            depthFirst(matrix, next, visited, totalNodes);
        }
    }
}

void startDFS(const vector<vector<int>>& matrix, int beginNode, int totalNodes) {
    vector<bool> visited(totalNodes, false);
    cout << "DFS Output: ";
    depthFirst(matrix, beginNode, visited, totalNodes);
    cout << endl;
}

int main() {
    int vertexCount, edgeCount;
    cout << "How many nodes in the graph? ";
    cin >> vertexCount;

    cout << "How many connections between nodes? ";
    cin >> edgeCount;

    vector<vector<int>> graph(vertexCount, vector<int>(vertexCount, 0));

    cout << "Enter each connection (format: A B):" << endl;
    for (int i = 0; i < edgeCount; i++) {
        int a, b;
        cin >> a >> b;
        graph[a][b] = 1;
        graph[b][a] = 1;
    }

    int startNode;
    cout << "Choose a starting node: ";
    cin >> startNode;

    runBFS(graph, startNode, vertexCount);
    startDFS(graph, startNode, vertexCount);

    return 0;
}
```
# Output 

How many nodes in the graph? 5  
How many connections between nodes? 4  
Enter each connection (format: A B):  
0 1  
0 2  
1 3  
2 4  
Choose a starting node: 0  

BFS Output: 0 1 2 3 4  
DFS Output: 0 1 3 2 4  
