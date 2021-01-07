# IX. Interview Questions - Chapter 4 | Trees and Graphs

## Trees
Recursive definition:
- each tree has a root node
- root has zero or more child nodes
- each child node has zero or more child nodes
- a `leaf` is a node with no children
- Aclycic

## Binary Tree
- A tree where each node has at most two children

## Binary Search Tree (BST)
- a binary tree in which every node n' left descendents are <= n and > n in its right descendents

## Balanced vs Unbalanced Trees
- the max height of the tree <= floor(log_2(n)) where n = # nodes

## Complete Binary Trees
- a binary tree in which every level of the tree is fully filled, but not necessarily the last level
- the last level is filled from left to right

## Full Binary Trees
- a binary tree in which every node has either zero or two children

## Perfect Binary Trees
- a binary tree that is both full and complete
- must have 2^k -1 nodes where k = number of levels

## In-Order Traversal
- visiting left branch, then current node, then right branch
- when performed on BST, visits nodes in ascending order
```
inOrderTraversal(Node node) {
    if(node !== null)
        inOrderTraversal(node.left)
        visit(node)
        inOrderTraversal(node.right)
}
```

## Pre-Order Traversal
- Visits current node before child nodes
- root is always the first node visited
```
preOrderTraversal(Node node) {
    if(node !== null)
        visit(node);
        preOrderTraversal(node.left)
        preOrderTraversal(node.right)
}
```

## Post-Order Traversal
- visits current node after child nodes
- the root is always last node visited
```
postOrderTraversal(Node node) {
    if(node !== null)
        postOrderTraversal(node.left)
        postOrderTraversal(node.right)
        visit(node);
}
```

## Binary Heaps (Min-Heaps and Max-Heaps)
- a `min-heap` is a `complete binary tree` where each node is smaller than its children
- therefore the min element is the root

**Two Key Operations**
- `insert`
    - start by inserting element at the bottom
    - insert at rightmost spot to maintain a complete tree
    - percolate node up if parent is > than node and continue until its children are < that node
- `extract-min`
    - remove the root and swap the last element in heap in its place
    - percolate root down until min-heap property is satisfied
        - when choosing which child to swap with, swap with smaller child to maintain min-heap property

## Tries (Prefix Trees)
- a type of n-ary tree in which characters are stored at each node
- each path down the tre mayh represent a node
- a * node (null node) often used to indicate incomplete words

Utility
- very commonly used to store the entire English language for quick prefix lookups


## Graphs
- a collection of nodes with edges between them
- can be either direct or undirected
    - direct edges are like a one-way street
    - undirected are a two-way street
- `connected graph` a path between every pair of vertices
- may have a cycles

### Programming Representation

**Adjacency List**
- every node stores a list of adjacent vertices
    - in an undirect graph, an edge would be stored twice (think about why)

```
class Graph {
    public Node[] nodes;
}

class Node {
    String name;
    Node[] children;
}
```

**Adjacency Matrix**
- NxN matrix where N = number of nodes
- a true value at matrix[i][j] indicates an edge from node i to node j
- In an undirected graph, an adjacency matrix will be symmetric

## Graph Search

**Depth-first Search (DFS)**
- start at root and explore each branch completely before moving on to next branch

Pseudocode Implementation
```
dfs(Node root) {
    if (root === null) return;
    visit(root);
    root.visited = true;
    for each (Node n in root.adjacent) {
        if(n.visited === false) {
            dfs(n);
        }
    }
}
```

**Breadth-first search (BFS)**
- start at root and explore each neighbor before going to any of their children
- good if you want to find the shortest path
- remember that BFS is not recursive
- use of queue

Pseudocode Implementation
```
bfs(Node root) {
    Queue queue = new Queue();
    root.marked = true;
    queue.enqueue(root);

    while(!queue.isEmpty()) {
        Node r = queue.dequeue();
        visit(r);
        foreach (Node n in r.adjacent) {
            if (n.marked === false) {
                n.marked = true;
                queue.enqueue(n);
            }
        }
    }
}
```

**Bidirectional Search**
- used to find the shortest path between a source and a destination node
- works by running two simultaneous breadth-first searches, one from each node
    - when their search collide, a path is found


### 4.1 Route Between Nodes
- Given a directed graph, design an algorithm to find out whether there is a route between two nodes.

Answer:
- implement DFS/BFS to find a specific node
- run DFS/BFS on both nodes and return true if both searches are true

Assuming the use of adjacency list and DFS implementation:
```
dfs(Node root, Node nodeToFind) {
    if (root === null) return;
    visit(root);
    root.visited = true;
    for each (Node n in root.adjacent) {
        if(n.visited === false) {
            if(n = nodeToFind) {
                return true;
            }
            dfs(n);
        }
    }
    return false;
}

routeBetweenNodes(Node n, Node m) {
    return dfs(n, m) && dfs(m, n);
}
```

### 4.2 Minimal Tree
- Given a sorted (increasing order) array with unique integer elements, write an algorithm to create a binary search tree with minimal height


Algorithm: 