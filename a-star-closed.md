### A* Search Algorithm without Closed List

The **A* Search Algorithm** is a popular pathfinding and graph traversal algorithm used in various applications, including real-time monitoring and decision-making. It finds the shortest path from a start node to a goal node by using a heuristic to guide the search. 

When implementing A* without a closed list, the algorithm does not keep track of visited nodes, which can lead to inefficiencies but might be simpler in some scenarios. This version can be useful in situations where memory is constrained or the search space is manageable.

#### C++ Code

Here’s a C++ implementation of the A* algorithm without a closed list:

```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <cmath>
#include <functional>

using namespace std;

// Define the structure for a node in the search space
struct Node {
    int x, y;
    int g; // Cost from start node
    int h; // Heuristic cost to goal
    int f; // Total cost (g + h)
    Node* parent;

    Node(int x, int y, int g, int h, Node* parent = nullptr)
        : x(x), y(y), g(g), h(h), f(g + h), parent(parent) {}

    // Comparison operator to prioritize nodes with lower f values
    bool operator<(const Node& other) const {
        return f > other.f;
    }
};

// Heuristic function (Manhattan distance)
int heuristic(int x1, int y1, int x2, int y2) {
    return abs(x1 - x2) + abs(y1 - y2);
}

// Function to perform A* search
vector<Node*> aStarSearch(int startX, int startY, int goalX, int goalY) {
    priority_queue<Node*> openSet;
    vector<vector<bool>> visited(10, vector<bool>(10, false)); // Assuming a 10x10 grid

    Node* startNode = new Node(startX, startY, 0, heuristic(startX, startY, goalX, goalY));
    openSet.push(startNode);

    while (!openSet.empty()) {
        Node* current = openSet.top();
        openSet.pop();

        if (current->x == goalX && current->y == goalY) {
            // Reconstruct path
            vector<Node*> path;
            while (current) {
                path.push_back(current);
                current = current->parent;
            }
            reverse(path.begin(), path.end());
            return path;
        }

        visited[current->x][current->y] = true;

        // Explore neighbors
        vector<pair<int, int>> neighbors = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}}; // 4-connectivity
        for (const auto& offset : neighbors) {
            int newX = current->x + offset.first;
            int newY = current->y + offset.second;

            if (newX >= 0 && newX < 10 && newY >= 0 && newY < 10 && !visited[newX][newY]) {
                int g = current->g + 1; // Assuming each move has a cost of 1
                int h = heuristic(newX, newY, goalX, goalY);
                Node* neighbor = new Node(newX, newY, g, h, current);
                openSet.push(neighbor);
            }
        }
    }

    return {}; // Return empty path if no path found
}

int main() {
    vector<Node*> path = aStarSearch(0, 0, 9, 9);

    if (!path.empty()) {
        cout << "Path found:" << endl;
        for (const auto& node : path) {
            cout << "(" << node->x << ", " << node->y << ")" << endl;
        }
    } else {
        cout << "No path found." << endl;
    }

    // Free allocated memory
    for (auto node : path) {
        delete node;
    }

    return 0;
}
```

#### Time and Space Complexity

```markdown
### Time and Space Complexity

| Operation             | Time Complexity    | Space Complexity  |
|-----------------------|--------------------|-------------------|
| A* Search (without Closed List) | O(b^d)            | O(b^d)            |

Where:
- **b**: Branching factor (number of neighbors for each node).
- **d**: Depth of the shallowest goal node.

```

**Explanation:**
- **Time Complexity**: O(b^d), where `b` is the branching factor and `d` is the depth of the goal node. Without a closed list, the algorithm may revisit nodes, potentially increasing the search space.
- **Space Complexity**: O(b^d), as the algorithm may store a large number of nodes in the open set, depending on the branching factor and depth.

**Applications:**
- **Real-Time Monitoring**: Used for pathfinding in environments where the state can change dynamically, such as in robotics or automated systems.
- **Decision-Making**: Useful in scenarios requiring optimal pathfinding or navigation without pre-computed constraints.

The A* Search Algorithm without a closed list provides a straightforward approach to pathfinding, suitable for real-time applications where memory constraints are less critical or the search space is relatively small.
