### Lazy Propagation

**Lazy Propagation** is a technique used in segment trees to optimize range updates. It helps to perform updates and queries more efficiently by deferring updates to segments until they are actually needed.

#### C++ Code

```cpp
#include <iostream>
#include <vector>
using namespace std;

class SegmentTreeLazy {
    vector<int> tree;
    vector<int> lazy;
    int n;

public:
    // Constructor
    SegmentTreeLazy(const vector<int>& arr) {
        n = arr.size();
        tree.resize(4 * n, 0);
        lazy.resize(4 * n, 0);
        build(0, 0, n - 1, arr);
    }

    // Build the segment tree
    void build(int node, int start, int end, const vector<int>& arr) {
        if (start == end) {
            tree[node] = arr[start];
        } else {
            int mid = (start + end) / 2;
            build(2 * node + 1, start, mid, arr);
            build(2 * node + 2, mid + 1, end, arr);
            tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
        }
    }

    // Update range [l, r] by adding value
    void updateRange(int l, int r, int value) {
        updateRange(0, 0, n - 1, l, r, value);
    }

    // Recursive function to update range
    void updateRange(int node, int start, int end, int l, int r, int value) {
        if (lazy[node] != 0) {
            tree[node] += (end - start + 1) * lazy[node];
            if (start != end) {
                lazy[2 * node + 1] += lazy[node];
                lazy[2 * node + 2] += lazy[node];
            }
            lazy[node] = 0;
        }

        if (start > end || start > r || end < l) return;

        if (start >= l && end <= r) {
            tree[node] += (end - start + 1) * value;
            if (start != end) {
                lazy[2 * node + 1] += value;
                lazy[2 * node + 2] += value;
            }
            return;
        }

        int mid = (start + end) / 2;
        updateRange(node * 2 + 1, start, mid, l, r, value);
        updateRange(node * 2 + 2, mid + 1, end, l, r, value);
        tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
    }

    // Query range sum [l, r]
    int queryRange(int l, int r) {
        return queryRange(0, 0, n - 1, l, r);
    }

    // Recursive function to query range sum
    int queryRange(int node, int start, int end, int l, int r) {
        if (lazy[node] != 0) {
            tree[node] += (end - start + 1) * lazy[node];
            if (start != end) {
                lazy[2 * node + 1] += lazy[node];
                lazy[2 * node + 2] += lazy[node];
            }
            lazy[node] = 0;
        }

        if (start > end || start > r || end < l) return 0;

        if (start >= l && end <= r) return tree[node];

        int mid = (start + end) / 2;
        int left_sum = queryRange(node * 2 + 1, start, mid, l, r);
        int right_sum = queryRange(node * 2 + 2, mid + 1, end, l, r);
        return left_sum + right_sum;
    }
};

int main() {
    vector<int> arr = {1, 3, 5, 7, 9, 11};
    SegmentTreeLazy segTree(arr);

    // Update range [1, 3] by adding 10
    segTree.updateRange(1, 3, 10);

    // Query sum of range [1, 3] after update
    cout << "Sum of range [1, 3] after update: " << segTree.queryRange(1, 3) << endl;

    return 0;
}
```

#### Time and Space Complexity

| Operation          | Time Complexity | Space Complexity |
|--------------------|-----------------|------------------|
| Build              | O(n)            | O(n)             |
| Update Range       | O(log n)        | O(n)             |
| Query Range        | O(log n)        | O(n)             |

Where:
- **n**: Number of elements in the array.
