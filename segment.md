### Segment Tree

**Segment Tree** is a data structure used for efficient range queries and updates. It is particularly useful for problems involving intervals or segments.

#### C++ Code

```cpp
#include <iostream>
#include <vector>
using namespace std;

class SegmentTree {
    vector<int> tree;
    vector<int> data;
    int n;

public:
    // Constructor
    SegmentTree(const vector<int>& arr) {
        n = arr.size();
        data = arr;
        tree.resize(4 * n);
        build(0, 0, n - 1);
    }

    // Build the segment tree
    void build(int node, int start, int end) {
        if (start == end) {
            tree[node] = data[start];
        } else {
            int mid = (start + end) / 2;
            build(2 * node + 1, start, mid);
            build(2 * node + 2, mid + 1, end);
            tree[node] = tree[2 * node + 1] + tree[2 * node + 2];
        }
    }

    // Update a value at index idx
    void update(int idx, int value) {
        update(0, 0, n - 1, idx, value);
    }

    // Recursive function to update a value
    void update(int node, int start, int end, int idx, int value) {
        if (start == end) {
            data[idx] = value;
            tree[node] = value;
        } else {
            int mid = (start + end) / 2;
            if (start <= idx && idx <= mid) {
                update(node * 2 + 1, start, mid, idx, value);
            } else {
                update(node * 2 + 2, mid + 1, end, idx, value);
            }
            tree[node] = tree[node * 2 + 1] + tree[node * 2 + 2];
        }
    }

    // Query the sum in range [l, r]
    int query(int l, int r) {
        return query(0, 0, n - 1, l, r);
    }

    // Recursive function to query the range sum
    int query(int node, int start, int end, int l, int r) {
        if (r < start || l > end) {
            return 0;
        }
        if (l <= start && end <= r) {
            return tree[node];
        }
        int mid = (start + end) / 2;
        int leftSum = query(node * 2 + 1, start, mid, l, r);
        int rightSum = query(node * 2 + 2, mid + 1, end, l, r);
        return leftSum + rightSum;
    }
};

int main() {
    vector<int> arr = {1, 3, 5, 7, 9, 11};
    SegmentTree segTree(arr);

    // Query sum of range [1, 3]
    cout << "Sum of range [1, 3]: " << segTree.query(1, 3) << endl;

    // Update value at index 2
    segTree.update(2, 6);

    // Query sum of range [1, 3] after update
    cout << "Sum of range [1, 3] after update: " << segTree.query(1, 3) << endl;

    return 0;
}
```

#### Time and Space Complexity

```markdown
### Time and Space Complexity

| Operation          | Time Complexity | Space Complexity |
|--------------------|-----------------|------------------|
| Build              | O(n)            | O(n)             |
| Update             | O(log n)        | O(n)             |
| Query              | O(log n)        | O(n)             |

Where:
- **n**: Number of elements in the array.
```
