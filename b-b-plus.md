### B-Tree and B+ Tree

**B-Trees** and **B+ Trees** are self-balancing tree data structures commonly used for efficient data storage and retrieval in databases and file systems. They are designed to keep data sorted and allow for efficient insertion, deletion, and search operations.

#### B-Tree

A **B-Tree** is a balanced tree data structure where each node can have multiple children and can store multiple keys. It maintains balance by ensuring that all leaf nodes are at the same depth, which provides efficient search, insertion, and deletion operations.

##### Properties
- Each node has a variable number of child nodes within a specified range.
- The tree remains balanced with all leaf nodes at the same depth.
- Efficient for read and write operations on large datasets.

#### C++ Code for B-Tree

```cpp
#include <iostream>
#include <vector>

using namespace std;

// A B-Tree Node
class BTreeNode {
    vector<int> keys;
    vector<BTreeNode*> children;
    int t; // Minimum degree
    bool leaf;

public:
    BTreeNode(int _t, bool _leaf);

    void traverse();

    void insertNonFull(int k);

    void splitChild(int i, BTreeNode* y);

    friend class BTree;
};

class BTree {
    BTreeNode* root;
    int t; // Minimum degree

public:
    BTree(int _t) { root = new BTreeNode(_t, true); t = _t; }

    void traverse() { root->traverse(); }

    void insert(int k) {
        if (root->keys.size() == 2 * t - 1) {
            BTreeNode* s = new BTreeNode(t, false);
            s->children.push_back(root);
            s->splitChild(0, root);
            root = s;
        }
        root->insertNonFull(k);
    }
};

BTreeNode::BTreeNode(int _t, bool _leaf) {
    t = _t;
    leaf = _leaf;
}

void BTreeNode::traverse() {
    int i;
    for (i = 0; i < keys.size(); i++) {
        if (!leaf)
            children[i]->traverse();
        cout << " " << keys[i];
    }
    if (!leaf)
        children[i]->traverse();
}

void BTreeNode::insertNonFull(int k) {
    int i = keys.size() - 1;
    if (leaf) {
        keys.push_back(0);
        while (i >= 0 && k < keys[i]) {
            keys[i + 1] = keys[i];
            i--;
        }
        keys[i + 1] = k;
    } else {
        while (i >= 0 && k < keys[i])
            i--;
        if (children[i + 1]->keys.size() == 2 * t - 1) {
            splitChild(i + 1, children[i + 1]);
            if (k > keys[i + 1])
                i++;
        }
        children[i + 1]->insertNonFull(k);
    }
}

void BTreeNode::splitChild(int i, BTreeNode* y) {
    BTreeNode* z = new BTreeNode(y->t, y->leaf);
    for (int j = 0; j < t - 1; j++)
        z->keys.push_back(y->keys[j + t]);
    if (!y->leaf) {
        for (int j = 0; j < t; j++)
            z->children.push_back(y->children[j + t]);
    }
    y->keys.resize(t - 1);
    y->children.resize(t);
    children.insert(children.begin() + i, z);
    keys.insert(keys.begin() + i, y->keys[t - 1]);
}

int main() {
    BTree t(3); // A B-Tree with minimum degree 3

    t.insert(10);
    t.insert(20);
    t.insert(5);
    t.insert(6);
    t.insert(15);
    t.insert(30);

    cout << "Traversal of the constructed B-Tree is:";
    t.traverse();
    cout << endl;

    return 0;
}
```

#### B+ Tree

A **B+ Tree** is an extension of the B-Tree where all values are stored at the leaf nodes, and internal nodes store only keys. This structure allows for efficient range queries and more straightforward in-order traversal.

##### Properties
- All values are stored at the leaf level.
- Internal nodes only store keys and have pointers to child nodes.
- Leaf nodes are often linked for efficient range queries.

#### C++ Code for B+ Tree

```cpp
#include <iostream>
#include <vector>

using namespace std;

// A B+ Tree Node
class BPlusTreeNode {
    vector<int> keys;
    vector<BPlusTreeNode*> children;
    BPlusTreeNode* next; // For leaf nodes only
    int t; // Minimum degree
    bool leaf;

public:
    BPlusTreeNode(int _t, bool _leaf);

    void traverse();

    void insertNonFull(int k);

    void splitChild(int i, BPlusTreeNode* y);

    friend class BPlusTree;
};

class BPlusTree {
    BPlusTreeNode* root;
    int t; // Minimum degree

public:
    BPlusTree(int _t) { root = new BPlusTreeNode(_t, true); t = _t; }

    void traverse() { root->traverse(); }

    void insert(int k) {
        if (root->keys.size() == 2 * t - 1) {
            BPlusTreeNode* s = new BPlusTreeNode(t, false);
            s->children.push_back(root);
            s->splitChild(0, root);
            root = s;
        }
        root->insertNonFull(k);
    }
};

BPlusTreeNode::BPlusTreeNode(int _t, bool _leaf) {
    t = _t;
    leaf = _leaf;
    next = nullptr;
}

void BPlusTreeNode::traverse() {
    int i;
    if (leaf) {
        for (i = 0; i < keys.size(); i++)
            cout << " " << keys[i];
        cout << endl;
    } else {
        for (i = 0; i < keys.size(); i++) {
            if (i > 0)
                cout << " ";
            cout << keys[i];
            children[i]->traverse();
        }
        children[i]->traverse();
    }
}

void BPlusTreeNode::insertNonFull(int k) {
    int i = keys.size() - 1;
    if (leaf) {
        keys.push_back(0);
        while (i >= 0 && k < keys[i]) {
            keys[i + 1] = keys[i];
            i--;
        }
        keys[i + 1] = k;
    } else {
        while (i >= 0 && k < keys[i])
            i--;
        if (children[i + 1]->keys.size() == 2 * t - 1) {
            splitChild(i + 1, children[i + 1]);
            if (k > keys[i + 1])
                i++;
        }
        children[i + 1]->insertNonFull(k);
    }
}

void BPlusTreeNode::splitChild(int i, BPlusTreeNode* y) {
    BPlusTreeNode* z = new BPlusTreeNode(y->t, y->leaf);
    for (int j = 0; j < t - 1; j++)
        z->keys.push_back(y->keys[j + t]);
    if (!y->leaf) {
        for (int j = 0; j < t; j++)
            z->children.push_back(y->children[j + t]);
    }
    y->keys.resize(t - 1);
    y->children.resize(t);
    children.insert(children.begin() + i, z);
    keys.insert(keys.begin() + i, y->keys[t - 1]);
    z->next = y->next;
    y->next = z;
}

int main() {
    BPlusTree t(3); // A B+ Tree with minimum degree 3

    t.insert(10);
    t.insert(20);
    t.insert(5);
    t.insert(6);
    t.insert(15);
    t.insert(30);

    cout << "Traversal of the constructed B+ Tree is:";
    t.traverse();
    cout << endl;

    return 0;
}
```

#### Time and Space Complexity

```markdown
### Time and Space Complexity

| Operation       | Time Complexity | Space Complexity |
|-----------------|-----------------|------------------|
| Search          | O(log n)        | O(n)             |
| Insertion       | O(log n)        | O(n)             |
| Deletion        | O(log n)        | O(n)             |

Where:
- **n**: Number of elements in the tree.

```

**Explanation:**
- **Time Complexity**: 
  - **Search, Insertion, Deletion**: O(log n), where `n` is the number of elements. The height of the B-Tree and B+ Tree is log(n), and operations are performed in logarithmic time.
  
- **Space Complexity**: O(n), as the space is used to store all nodes and keys in the tree.

**Applications:**
- **Efficient Data Storage**: Suitable for databases and file systems where efficient data retrieval and updates are crucial.
- **Indexing**: Used in indexing large datasets to enable quick search and retrieval operations.

B-Trees

 and B+ Trees are foundational data structures for managing large sets of data and are crucial for optimizing search and retrieval in various applications.
