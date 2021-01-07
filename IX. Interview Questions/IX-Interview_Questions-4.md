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


Algorithm: This is a pre-order traversal of the tree except we have to traverse it with an array.

Middle of two indices x,y of an array = ceil( (y-x)/2)

```
minTree(list) {
    let root = new Node();

    minTreeHelper(list, 0, list.length-1, root);

    return root;
}

minTreeHelper(list, start, end, node) {
    do {
        let middleIndex = ceil(end-start/2);    // Calculate middle index
        node.val = list[middleIndex];   // Insert into tree
        
        node.left = new Node(null);
        minTreeHelper(list, start, middleIndex-1, node.left)    // Insert the correct node in left child
        
        node.right = new Node(null);
        minTreeHelper(list, middleIndex+1, end, node.right) // Repeate with right child
    } while(start !== end)
}
```

### 4.3 List of Depths
- Given a binary tree, design an algorithm which creates a linked list of all the nodes at each depth (e.,g., if you have a tree with depth D, you'll have D linked lists).

Method 1:
- Store linked lists in an array (an expanding one like an ArrayList)
- BFS through the tree
- keep track of height
- insert nodes at the linkedlist at list[currentHeight]
- How do we insert to the correct linked list?
    - Or rather, how do we maintain the currentHeight?
    - We know that every time you look at the left or right node, the height of that node is going to be the height of the parent + 1

In JavaScript:

Node/Linked List Implementation:
```
class Node {
    constructor(val) {
        this.val = val;
        this.next = null;
    }
}
```

```
class LinkedList {
    constructor(root) {
        this.root = root;
    }

    insert(node) {
        if(this.root === null)
            this.root = node;
        else
            let currentNode = this.root;
            while(currentNode.next !== null) {
                currentNode = currentNode.next
            }
            currentNode.next = node;
    }
}
```

```
class WrapperNode {
    constructor(node, depth) {
        this.node = node;
        this.depth = depth;
    }
}
```

```
listOfDepths(root) {
    let queue = [];

    let tempQueue = [];

    let depth = 0;
    queue.push(new WrapperNode(root, depth));

    while(queue.length > 0 {
        let currentNode = queue[0];

        // Insert nodes in LinkedList

        if(currentNode.left !== null) {
            queue.push(new WrapperNode(currentNode.left, currentNode.depth + 1))
        }
        if(currentNode.right !== null) {
            queue.push(new WrapperNode(currentNode.right, currentNode.depth + 1))
        }

        tempQueue.push(queue.peek());
        queue.shift();
    }

    // The max depth will be at the last index
    let maxDepth = tempQueue[tempQueue.length-1].depth;
    let linkedLists = [];
    for(let i = 0; i <= maxDepth; i++) {
        linkedLists.push(new LinkedList(null));
    }
    // Insert the nodes at the correct index where each index = its depth in the tree
    for(let i = 0; i < tempQueue.length; i++) {
        let depth = tempQueue[i].depth;
        linkedList[depth].insert(tempQueue[i].node);
    }
}
```

### 4.4 Check Balanced
- Implement a function to check if a binary tree is balanced. For the purposes of this question, a balanced tree is defined to be a tree such that the heights of the two subtrees of any node never differs by more than one.

Algorithm:
- A tree's height is a  defined recursively the max height of its children and the height a node is the the height of a parent's + 1

```
checkBalanced(root) {
    if(root === null) return false;

    let leftHeight = helper(root.left, 0);
    let rightHeight = helper(root.right, 0;

    if(!leftHeight || !rightHeight) {
        return false;
    } else {
        return (abs(leftHeight - rightHeight) <= 1);
    }
}

helper(node, height) {
    while(node !== null) {
        let leftHeight = helper(node.left, height + 1);
        let rightHeight = helper(node.right, height + 1;
        if(!leftHeight || !rightHeight) {
            return false;
        }  
    }

    return 0;
}
```

### 4.5 Validate BST
- Implement a function to check if a binary tree is a binary search tree

Algorithm:
- In-order traverse through potential BST
- At each traversal insert into list
- Check if list is in ascending order

```
inOrderTraversal(root, list) {
    while(root !== null) {
        inOrderTraversal(root.left, list);
        list.push(root.val);
        inOrderTraversal(root.right, list);
    }
}
```

```
validateBST(root) {
    let list = [];

    inOrderTraversal(root, list);

    let current = list[0];
    for(let i = 0; i < list.length; i++) {
        if(current < list[i]) {
            return false;
        }
    }

    return true;
}
```

### 4.6 Successor - Incomplete
- Write an algorithm to find the "next" node (i.e., in-order successor) of a given node in a binary search tree. You may assume that each node has a link to its parent.

Algorithm:
- The successor of a node is either going to be 
    1. the min of the right subtree
    2. or its parent

```
min(node) {
    if(node === null){  // node doesnt have a left subtree
        return null;
    }
    if(node.left === null) {
        return node;
    }
    while(node !== null) {
        inOrderTraversal(node.left);
    }
}
```

```
successor(node) {
    let x = min(node.right);
    if(x) 
        return x.val;
    else
        return node.parent.val; 
}
```
