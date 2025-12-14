
# Assignment 10.2

# Title : Write a program to implement Dijkstra’s algorithm to find the shortest distance between two nodes of a user defined graph using Adjacency Matrix representation.

# Thoery :

In graph theory, finding the shortest path between two nodes is an important problem with applications in navigation systems, network routing, and traffic management.

Dijkstra’s Algorithm is a greedy algorithm used to find the shortest distance from a single source vertex to all other vertices in a weighted graph, provided that all edge weights are non-negative. The algorithm repeatedly selects the vertex with the minimum tentative distance and relaxes the edges connected to it.

An Adjacency Matrix representation stores the graph in a two-dimensional array where each cell represents the weight of the edge between two vertices. If no edge exists, a very large value (infinity) is stored.

A priority queue (min-heap) is used to efficiently select the next vertex with the smallest distance. Arrays are used to maintain the minimum distance from the source and to store parent information for reconstructing the shortest path.

This implementation helps in understanding shortest path algorithms, greedy strategies, and matrix-based graph representations.

# CODE 
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <stack>
#include <climits>

using namespace std;

typedef pair<int, int> WeightNode;

void computeShortestPath(const vector<vector<int>> &matrix, int vertexCount, int sourceVertex,
                         vector<int> &minDistance, vector<int> &parentNode)
{
    minDistance.assign(vertexCount, INT_MAX);
    parentNode.assign(vertexCount, -1);
    minDistance[sourceVertex] = 0;

    priority_queue<WeightNode, vector<WeightNode>, greater<WeightNode>> minHeap;
    minHeap.push({0, sourceVertex});

    while (!minHeap.empty())
    {
        int current = minHeap.top().second;
        int dist = minHeap.top().first;
        minHeap.pop();

        if (dist > minDistance[current])
            continue;

        for (int next = 0; next < vertexCount; next++)
        {
            if (matrix[current][next] != INT_MAX &&
                minDistance[current] + matrix[current][next] < minDistance[next])
            {
                minDistance[next] = minDistance[current] + matrix[current][next];
                parentNode[next] = current;
                minHeap.push({minDistance[next], next});
            }
        }
    }
}

void displayRoute(const vector<int> &parentNode, int destination)
{
    if (parentNode[destination] == -1)
    {
        cout << "No valid route found.\n";
        return;
    }

    stack<int> route;
    for (int v = destination; v != -1; v = parentNode[v])
        route.push(v);

    cout << "Route: ";
    while (!route.empty())
    {
        cout << route.top();
        route.pop();
        if (!route.empty())
            cout << " -> ";
    }
    cout << endl;
}

int main()
{
    int vertexCount, edgeCount;
    cout << "Enter total vertices: ";
    cin >> vertexCount;

    cout << "Enter total edges: ";
    cin >> edgeCount;

    vector<vector<int>> matrix(vertexCount, vector<int>(vertexCount, INT_MAX));

    cout << "Enter edges (from to weight):\n";
    for (int i = 0; i < edgeCount; i++)
    {
        int from, to, wt;
        cin >> from >> to >> wt;
        matrix[from][to] = wt;
    }

    int source, destination;
    cout << "Enter source vertex: ";
    cin >> source;

    cout << "Enter destination vertex: ";
    cin >> destination;

    vector<int> minDistance, parentNode;

    computeShortestPath(matrix, vertexCount, source, minDistance, parentNode);

    if (minDistance[destination] == INT_MAX)
    {
        cout << "No route exists between " << source << " and " << destination << ".\n";
    }
    else
    {
        cout << "Minimum Distance = " << minDistance[destination] << endl;
        displayRoute(parentNode, destination);
    }

    return 0;
}
```
# Output 

Enter total vertices: 5  
Enter total edges: 6  
Enter edges (from to weight):  
0 1 2  
0 2 4  
1 2 1  
1 3 7  
2 4 3  
3 4 1  
Enter source vertex: 0  
Enter destination vertex: 4  

Minimum Distance = 6  
Route: 0 -> 1 -> 2 -> 4  