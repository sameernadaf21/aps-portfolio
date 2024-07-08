### Rod Cutting: Max Product

**Rod Cutting** is a classic dynamic programming problem where you are given a rod of length `n` and a list of prices for each length from `1` to `n`. The goal is to determine the maximum revenue obtainable by cutting the rod into pieces and selling them, such that the sum of the prices of these pieces is maximized.

In the **Max Product** variation of this problem, the objective is to maximize the product of the lengths of the pieces, rather than the sum of their prices.

#### C++ Code

Here's a C++ implementation for finding the maximum product obtainable by cutting a rod:

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

// Function to find the maximum product of cutting a rod of length n
int maxProduct(int n) {
    if (n <= 1) return 0;

    vector<int> dp(n + 1, 0);

    // Fill the dp array with maximum product values
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= i / 2; ++j) {
            dp[i] = max(dp[i], max(j * (i - j), j * dp[i - j]));
        }
    }

    return dp[n];
}

int main() {
    int length = 10; // Example rod length
    cout << "Maximum product for a rod of length " << length << " is " << maxProduct(length) << endl;
    return 0;
}
```

#### Time and Space Complexity


| Operation           | Time Complexity | Space Complexity |
|---------------------|-----------------|------------------|
| Initialization      | O(n^2)          | O(n)             |
| Max Product Computation | O(n^2)      | O(n)             |

Where:
- **n**: Length of the rod.


In this implementation:
- `dp[i]` represents the maximum product obtainable for a rod of length `i`.
- The nested loops fill the `dp` array by checking every possible cut length `j` and comparing the product of `j * (i - j)` with `j * dp[i - j]` to determine the maximum product for the current length `i`.

**Key Points:**
- The algorithm uses a dynamic programming approach to build up solutions for smaller subproblems and combine them to solve larger problems.
- It efficiently computes the maximum product for each length by considering all possible cuts and storing intermediate results to avoid redundant computations.
