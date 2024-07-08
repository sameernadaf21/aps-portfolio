## Subset Sum Problem Code

The Subset Sum Problem is a classic example of dynamic programming used to determine if there's a subset of a given set with a sum equal to a specified value.

### C++ Code

```cpp
#include <iostream>
#include <vector>
using namespace std;

// Function to check if a subset with sum equal to the given sum exists
bool isSubsetSum(vector<int>& set, int sum) {
    int n = set.size();
    vector<vector<bool>> subset(n+1, vector<bool>(sum+1));

    // If sum is 0, then answer is true
    for (int i = 0; i <= n; i++)
        subset[i][0] = true;

    // If sum is not 0 and set is empty, then answer is false
    for (int i = 1; i <= sum; i++)
        subset[0][i] = false;

    // Fill the subset table in bottom-up manner
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= sum; j++) {
            if (j < set[i-1])
                subset[i][j] = subset[i-1][j];
            else
                subset[i][j] = subset[i-1][j] || subset[i-1][j-set[i-1]];
        }
    }

    return subset[n][sum];
}

int main() {
    vector<int> set = {3, 34, 4, 12, 5, 2};
    int sum = 9;
    if (isSubsetSum(set, sum))
        cout << "Found a subset with the given sum.";
    else
        cout << "No subset with the given sum.";
    return 0;
}
