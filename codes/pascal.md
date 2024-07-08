### Catalan Numbers

**Catalan Numbers** are a sequence of natural numbers that have many applications in combinatorial mathematics. The \( n \)-th Catalan number can be defined using the following formula:

\[ C_n = \frac{1}{n+1} \binom{2n}{n} \]

where \(\binom{2n}{n}\) is the binomial coefficient. The Catalan number can also be computed recursively:

\[ C_n = \sum_{i=0}^{n-1} C_i \cdot C_{n-1-i} \]

with \( C_0 = 1 \).

#### C++ Code

**Recursive Approach:**

```cpp
#include <iostream>
using namespace std;

// Recursive function to calculate the nth Catalan number
long long catalanRecursive(int n) {
    if (n <= 1)
        return 1;
    long long res = 0;
    for (int i = 0; i < n; i++)
        res += catalanRecursive(i) * catalanRecursive(n - 1 - i);
    return res;
}

int main() {
    int n = 5;
    cout << "Catalan number C(" << n << ") using recursion is: " << catalanRecursive(n) << endl;
    return 0;
}
```

**Dynamic Programming Approach:**

```cpp
#include <iostream>
#include <vector>
using namespace std;

// Function to calculate the nth Catalan number using dynamic programming
long long catalanDP(int n) {
    vector<long long> C(n + 1, 0);
    C[0] = C[1] = 1;
    
    for (int i = 2; i <= n; i++) {
        for (int j = 0; j < i; j++) {
            C[i] += C[j] * C[i - 1 - j];
        }
    }
    
    return C[n];
}

int main() {
    int n = 5;
    cout << "Catalan number C(" << n << ") using dynamic programming is: " << catalanDP(n) << endl;
    return 0;
}
```

#### Time and Space Complexity

```markdown
### Time and Space Complexity

| Operation              | Time Complexity   | Space Complexity  |
|------------------------|-------------------|-------------------|
| Recursive Approach     | O(2^n)            | O(n)              |
| Dynamic Programming    | O(n^2)            | O(n)              |

Where:
- **n**: The index of the Catalan number to be computed.
```

