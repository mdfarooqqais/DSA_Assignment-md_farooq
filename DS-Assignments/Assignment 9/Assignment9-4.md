# Assignment 9.4

# Title : Write a program to implement Dijkstra’s algorithm to find the shortest distance between two nodes of a user defined graph using Adjacency List representation.

# Thoery :

In graph theory, finding the shortest path between two nodes is an important problem with applications in routing, navigation systems, and network analysis.

Dijkstra’s Algorithm is a greedy algorithm used to find the shortest path from a single source node to all other nodes in a weighted graph, provided that all edge weights are non-negative. The algorithm works by repeatedly selecting the node with the minimum tentative distance and relaxing its adjacent edges.

An Adjacency List representation is used to store the graph efficiently. Each node maintains a list of its neighboring nodes along with the weight of the edge connecting them.

A priority queue (min-heap) is used in Dijkstra’s algorithm to efficiently extract the node with the smallest distance value. Additionally, an array is maintained to store the minimum distance from the source to each node, and another array keeps track of the previous node to reconstruct the shortest path.

This algorithm helps in understanding greedy techniques, priority queues, and shortest path computation in graphs.

# Algorithm :

# CODE 
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <stack>
#include <climits>
using namespace std;

typedef pair<int,int> WeightNode;

void computeShortest(const vector<vector<WeightNode>> &graph, int totalNodes, int startNode,
                     vector<int> &minDistance, vector<int> &prevNode)
{
    minDistance.assign(totalNodes, INT_MAX);
    prevNode.assign(totalNodes, -1);

    priority_queue<WeightNode, vector<WeightNode>, greater<WeightNode>> pq;
    minDistance[startNode] = 0;
    pq.push({0, startNode});

    while(!pq.empty())
    {
        int currentNode = pq.top().second;
        int currentDist = pq.top().first;
        pq.pop();

        if(currentDist > minDistance[currentNode])
            continue;

        for(auto &nbr : graph[currentNode])
        {
            int nextNode = nbr.first;
            int edgeCost = nbr.second;

            if(minDistance[currentNode] + edgeCost < minDistance[nextNode])
            {
                minDistance[nextNode] = minDistance[currentNode] + edgeCost;
                prevNode[nextNode] = currentNode;
                pq.push({minDistance[nextNode], nextNode});
            }
        }
    }
}

void showPath(const vector<int> &prevNode, int endNode)
{
    if(prevNode[endNode] == -1)
    {
        cout << "Path not available" << endl;
        return;
    }
    stack<int> route;
    for(int v = endNode; v != -1; v = prevNode[v])
        route.push(v);

    cout << "Path Followed: ";
    while(!route.empty())
    {
        cout << route.top();
        route.pop();
        if(!route.empty()) cout << " -> ";
    }
    cout << endl;
}

int main()
{
    int nodeCount, edgeCount;
    cout << "Enter total nodes: ";
    cin >> nodeCount;

    cout << "Enter total edges: ";
    cin >> edgeCount;

    vector<vector<WeightNode>> graph(nodeCount);

    cout << "Provide edges (from to weight):" << endl;
    for(int i = 0; i < edgeCount; i++)
    {
        int a, b, w;
        cin >> a >> b >> w;
        graph[a].push_back({b, w});
    }

    int startNode, endNode;
    cout << "Enter start node: ";
    cin >> startNode;

    cout << "Enter destination node: ";
    cin >> endNode;

    vector<int> minDistance, prevNode;
    computeShortest(graph, nodeCount, startNode, minDistance, prevNode);

    if(minDistance[endNode] == INT_MAX)
    {
        cout << "No valid route found" << endl;
    }
    else
    {
        cout << "Minimum Distance: " << minDistance[endNode] << endl;
        showPath(prevNode, endNode);
    }

    return 0;
}
```
# Output 

Enter total nodes: 5  
Enter total edges: 6  
Provide edges (from to weight):  
0 1 2  
0 2 4  
1 2 1  
1 3 7  
2 4 3  
3 4 1  
Enter start node: 0  
Enter destination node: 4  

Minimum Distance: 6  
Path Followed: 0 -> 1 -> 2 -> 4  
