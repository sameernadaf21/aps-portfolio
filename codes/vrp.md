### Vehicle Routing Problem (VRP)

The Vehicle Routing Problem (VRP) involves finding the optimal set of routes for a fleet of vehicles to service a set of customers with known demands. The goal is to minimize the total distance traveled or cost, subject to various constraints such as vehicle capacity and route limits.

#### C++ Code

For simplicity, we'll use a heuristic approach for solving VRP. The following code assumes a basic scenario with a fixed number of vehicles and customers, aiming to demonstrate a straightforward solution strategy.

```cpp
#include <iostream>
#include <vector>
#include <limits>
#include <algorithm>

using namespace std;

#define INF numeric_limits<int>::max()

// Function to find the minimum cost route for a given vehicle
int findMinCostRoute(const vector<vector<int>>& costMatrix, vector<int>& route, vector<bool>& visited, int start, int vehicleCount) {
    int n = costMatrix.size();
    int minCost = INF;
    int minIndex = -1;

    for (int i = 0; i < n; ++i) {
        if (!visited[i] && costMatrix[start][i] < minCost) {
            minCost = costMatrix[start][i];
            minIndex = i;
        }
    }

    if (minIndex != -1) {
        route.push_back(minIndex);
        visited[minIndex] = true;
        minCost += findMinCostRoute(costMatrix, route, visited, minIndex, vehicleCount);
    }

    return minCost;
}

// VRP Heuristic function
void vehicleRoutingProblem(const vector<vector<int>>& costMatrix, int vehicleCount) {
    int n = costMatrix.size();
    vector<vector<int>> routes(vehicleCount);
    vector<bool> visited(n, false);

    for (int i = 0; i < vehicleCount; ++i) {
        vector<int> route;
        visited[0] = true; // Start from depot
        route.push_back(0);

        int cost = findMinCostRoute(costMatrix, route, visited, 0, vehicleCount);

        cout << "Route for Vehicle " << i + 1 << ": ";
        for (int node : route) {
            cout << node << " ";
        }
        cout << endl;
        cout << "Cost for Vehicle " << i + 1 << ": " << cost << endl;
    }
}

int main() {
    vector<vector<int>> costMatrix = {
        {0, 10, 15, 20, 25},
        {10, 0, 35, 25, 30},
        {15, 35, 0, 30, 20},
        {20, 25, 30, 0, 15},
        {25, 30, 20, 15, 0}
    };

    int vehicleCount = 2; // Number of vehicles
    vehicleRoutingProblem(costMatrix, vehicleCount);

    return 0;
}
```

#### Time and Space Complexity


| Operation           | Time Complexity           | Space Complexity  |
|---------------------|---------------------------|-------------------|
| Route Calculation   | O(V^2)                    | O(V)              |
| Heuristic Algorithm | O(V^3) (heuristic)        | O(V^2)            |

Where:
- **V**: Number of vertices (nodes/customers) in the graph.

