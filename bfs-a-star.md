### Best First Search

**Best First Search** is a search algorithm used for exploring paths or solutions in a graph or tree by selecting the most promising nodes based on a heuristic function. This approach is used to find the most optimal path or solution efficiently.

#### Overview

- **Best First Search** uses a priority queue to explore the most promising nodes first based on their heuristic values. It is often used in AI and pathfinding algorithms.

#### C++ Code for Best First Search

Here's a basic implementation using a priority queue in C++:

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>

using namespace std;

struct Node {
    int id;
    int heuristic;

    bool operator>(const Node& other) const {
        return heuristic > other.heuristic;
    }
};

// Function to perform Best First Search
void bestFirstSearch(int start, int goal, const vector<vector<int>>& graph, const vector<int>& heuristics) {
    priority_queue<Node, vector<Node>, greater<Node>> pq;
    vector<bool> visited(graph.size(), false);

    pq.push({start, heuristics[start]});

    while (!pq.empty()) {
        Node current = pq.top();
        pq.pop();

        if (current.id == goal) {
            cout << "Goal node " << goal << " found." << endl;
            return;
        }

        if (visited[current.id]) continue;

        visited[current.id] = true;
        cout << "Exploring node " << current.id << endl;

        for (int neighbor : graph[current.id]) {
            if (!visited[neighbor]) {
                pq.push({neighbor, heuristics[neighbor]});
            }
        }
    }

    cout << "Goal node not found." << endl;
}

int main() {
    int n = 6; // Number of nodes
    vector<vector<int>> graph(n);

    // Example graph
    graph[0] = {1, 2};
    graph[1] = {3, 4};
    graph[2] = {4, 5};
    graph[3] = {};
    graph[4] = {5};
    graph[5] = {};

    vector<int> heuristics = {6, 4, 3, 2, 1, 0}; // Example heuristics

    int start = 0;
    int goal = 5;

    bestFirstSearch(start, goal, graph, heuristics);

    return 0;
}
```

#### Time and Space Complexity



| Operation             | Time Complexity | Space Complexity |
|-----------------------|-----------------|------------------|
| Best First Search     | O(b^d)          | O(b^d)           |

Where:
- **b**: Branching factor (average number of children per node).
- **d**: Depth of the goal node.



**Explanation:**
- **Time Complexity**: O(b^d) due to the exploration of potentially all nodes in the search space.
- **Space Complexity**: O(b^d) for storing nodes in the priority queue and visited list.

### A* with Priority Queue

**A* (A-star) Algorithm** is an extension of Best First Search that uses both the cost to reach the node and the estimated cost to the goal. It combines the advantages of Uniform Cost Search and Best First Search, making it more efficient in finding the optimal path.

#### Overview

- **A* Algorithm** uses a priority queue to explore nodes based on the cost function \( f(n) = g(n) + h(n) \), where \( g(n) \) is the cost from the start node to node \( n \), and \( h(n) \) is the heuristic estimate of the cost from node \( n \) to the goal.

#### C++ Code for A* Algorithm

Here's a basic implementation of the A* algorithm:

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>
#include <cmath>

using namespace std;

struct Node {
    int id;
    int g; // Cost from start node
    int h; // Heuristic cost to goal

    bool operator>(const Node& other) const {
        return (g + h) > (other.g + other.h);
    }
};

// Function to perform A* search
void aStarSearch(int start, int goal, const vector<vector<int>>& graph, const vector<int>& heuristics, const vector<vector<int>>& costs) {
    priority_queue<Node, vector<Node>, greater<Node>> pq;
    vector<int> gCosts(graph.size(), INT_MAX);
    vector<bool> visited(graph.size(), false);

    pq.push({start, 0, heuristics[start]});
    gCosts[start] = 0;

    while (!pq.empty()) {
        Node current = pq.top();
        pq.pop();

        if (current.id == goal) {
            cout << "Goal node " << goal << " found with cost " << current.g << endl;
            return;
        }

        if (visited[current.id]) continue;

        visited[current.id] = true;
        cout << "Exploring node " << current.id << " with g = " << current.g << " and h = " << current.h << endl;

        for (size_t i = 0; i < graph[current.id].size(); ++i) {
            int neighbor = graph[current.id][i];
            int newCost = current.g + costs[current.id][i];

            if (newCost < gCosts[neighbor]) {
                gCosts[neighbor] = newCost;
                pq.push({neighbor, newCost, heuristics[neighbor]});
            }
        }
    }

    cout << "Goal node not found." << endl;
}

int main() {
    int n = 6; // Number of nodes
    vector<vector<int>> graph(n);
    vector<vector<int>> costs(n, vector<int>(n, 0));

    // Example graph
    graph[0] = {1, 2};
    graph[1] = {3, 4};
    graph[2] = {4, 5};
    graph[3] = {};
    graph[4] = {5};
    graph[5] = {};

    // Costs between nodes
    costs[0] = {1, 4};
    costs[1] = {1, 2};
    costs[2] = {5, 1};
    costs[3] = {};
    costs[4] = {2};
    costs[5] = {};

    vector<int> heuristics = {6, 4, 3, 2, 1, 0}; // Example heuristics

    int start = 0;
    int goal = 5;

    aStarSearch(start, goal, graph, heuristics, costs);

    return 0;
}
```

#### Time and Space Complexity


| Operation             | Time Complexity | Space Complexity |
|-----------------------|-----------------|------------------|
| A* Search             | O(b^d)          | O(b^d)           |

Where:
- **b**: Branching factor (average number of children per node).
- **d**: Depth of the goal node.



**Explanation:**
- **Time Complexity**: O(b^d), as the algorithm may explore all nodes in the worst case.
- **Space Complexity**: O(b^d) for storing nodes in the priority queue and the `gCosts` array.

**Applications:**
- **Best First Search**: Used in exploring optimal technology solutions where heuristic values are used to guide the search.
- **A* with Priority Queue**: Effective for project prioritization and resource allocation where both cost and heuristic values are considered to find the optimal path or solution.
