# Assignment 9.5

# Title : Write a program to accept a graph from a user and represent it using Adjacency List and perform BFS and DFS traversals on it.

# Thoery :

A graph is a non-linear data structure consisting of vertices (nodes) and edges (connections between nodes). Graphs are widely used to represent networks such as computer networks, social networks, transportation systems, and communication systems.

An Adjacency List is an efficient way to represent a graph, especially when the graph is sparse. In this representation, each vertex stores a list of its adjacent vertices. This reduces memory usage compared to adjacency matrix representation.

Breadth First Search (BFS) is a graph traversal technique that explores nodes level by level. Starting from a given node, it first visits all its adjacent nodes before moving to the next level. BFS uses a queue data structure to maintain the traversal order.

Depth First Search (DFS) is another traversal technique that explores as deep as possible along each branch before backtracking. DFS is commonly implemented using recursion or a stack.

BFS and DFS help in understanding graph connectivity, traversal strategies, and the use of auxiliary data structures like queues, stacks, and recursion.

# CODE 
```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <stack>
using namespace std;

void runBFS(const vector<vector<int>> &graph, int totalNodes, int beginNode)
{
    vector<bool> seen(totalNodes, false);
    queue<int> qref;
    qref.push(beginNode);
    seen[beginNode] = true;

    cout << "BFS Order: ";
    while (!qref.empty())
    {
        int curr = qref.front();
        qref.pop();
        cout << curr << " ";
        for (int nxt : graph[curr])
        {
            if (!seen[nxt])
            {
                seen[nxt] = true;
                qref.push(nxt);
            }
        }
    }
    cout << endl;
}

void exploreDFS(const vector<vector<int>> &graph, vector<bool> &visited, int currNode)
{
    visited[currNode] = true;
    cout << currNode << " ";
    for (int nxt : graph[currNode])
    {
        if (!visited[nxt])
        {
            exploreDFS(graph, visited, nxt);
        }
    }
}

void runDFS(const vector<vector<int>> &graph, int totalNodes, int beginNode)
{
    vector<bool> visited(totalNodes, false);
    cout << "DFS Order: ";
    exploreDFS(graph, visited, beginNode);
    cout << endl;
}

int main()
{
    int vertices, edges;

    cout << "How many nodes in the graph? ";
    cin >> vertices;

    cout << "How many connections? ";
    cin >> edges;

    vector<vector<int>> graph(vertices);

    cout << "Enter each connection (node1 node2):" << endl;
    for (int i = 0; i < edges; i++)
    {
        int a, b;
        cin >> a >> b;
        graph[a].push_back(b);
        graph[b].push_back(a);
    }

    int startNode;
    cout << "Enter starting node: ";
    cin >> startNode;

    runBFS(graph, vertices, startNode);
    runDFS(graph, vertices, startNode);

    return 0;
}
```
# Output 

How many nodes in the graph? 5  
How many connections? 4  
Enter each connection (node1 node2):  
0 1  
0 2  
1 3  
2 4  
Enter starting node: 0  

BFS Order: 0 1 2 3 4  
DFS Order: 0 1 3 2 4  
