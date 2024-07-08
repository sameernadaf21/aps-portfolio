### Sliding Window

**Sliding Window** is a technique used to optimize algorithms that need to process a contiguous block of elements from a collection. It is commonly used for problems involving finding the maximum or minimum value within a window of a fixed size or dynamic size in an array.

#### C++ Code

```cpp
#include <iostream>
#include <deque>
#include <vector>
using namespace std;

// Function to find the maximum values in a sliding window of size k
vector<int> slidingWindowMax(const vector<int>& nums, int k) {
    vector<int> result;
    deque<int> dq;

    for (int i = 0; i < nums.size(); ++i) {
        // Remove elements outside the current window
        if (!dq.empty() && dq.front() == i - k) {
            dq.pop_front();
        }

        // Remove elements smaller than the currently being added element (nums[i])
        while (!dq.empty() && nums[dq.back()] <= nums[i]) {
            dq.pop_back();
        }

        // Add the current element at the back of the deque
        dq.push_back(i);

        // The front of the deque is the largest element for the current window
        if (i >= k - 1) {
            result.push_back(nums[dq.front()]);
        }
    }

    return result;
}

int main() {
    vector<int> nums = {1, 3, -1, -3, 5, 3, 6, 7};
    int k = 3;

    vector<int> max_values = slidingWindowMax(nums, k);

    cout << "Maximum values in each sliding window of size " << k << ": ";
    for (int val : max_values) {
        cout << val << " ";
    }
    cout << endl;

    return 0;
}
```

#### Time and Space Complexity


| Operation             | Time Complexity | Space Complexity |
|-----------------------|-----------------|------------------|
| Sliding Window Max    | O(n)            | O(k)             |

Where:
- **n**: Number of elements in the input array.
- **k**: Size of the sliding window.
