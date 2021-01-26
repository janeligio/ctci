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
> Given a directed graph, design an algorithm to find out whether there is a route between two nodes.

Algorithm:
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
> Given a sorted (increasing order) array with unique integer elements, write an algorithm to create a binary search tree with minimal height.


Algorithm: This is a pre-order traversal of the tree except we have to traverse it with an array.

Middle of two indices x,y of an array = `ceil((y-x)/2)`

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
> Given a binary tree, design an algorithm which creates a linked list of all the nodes at each depth (e.,g., if you have a tree with depth D, you'll have D linked lists).

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
> Implement a function to check if a binary tree is balanced. For the purposes of this question, a balanced tree is defined to be a tree such that the heights of the two subtrees of any node never differs by more than one.

Algorithm:
- A tree's height is defined recursively as the max height of its children and the height of a node is the the height of a parent's + 1

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

```
function checkBalanced(root) {
    if(root === null) {
        return 1;
    }

    return abs((checkBalanced(root.left) + 1), (checkBalanced(root.right) + 1)) < 2;
}
```

### 4.5 Validate BST
> Implement a function to check if a binary tree is a binary search tree

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

Optimized version:
- Do any type of traversal
- During the traversal, check if the values are what they would be if it was a BST. That is, root < root. right && root > root.left

```
function validateBST(root) {
    if(root === null) {
        return root;
    }

    let middle = root;
    let left = validateBST(root.left.val);
    left right = validateBST(root.right.val);

    if(!(middle < right) || !(middle > left)) {
        return false;
    }

}
```

### 4.6 Successor
> Write an algorithm to find the "next" node (i.e., in-order successor) of a given node in a binary search tree. You may assume that each node has a link to its parent.

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
        min(node.left);
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

### 4.7 Build Order
> You are given a list of projects and a list of dependencies (which is a list of pairs of projects, where the second project is dependent on the first project). All of a project's dependencies must be built before the project is. Find a build order that will allow the projects to be built. If there is no valid build order, return an error.

```
class Project {
    constructor(project, parents, children) {
        this.project = project;
        this.parents = [...parents];
        this.children = [...children];
    }
}

function buildOrder(projects, dependencies) {
    let projectNodes = [];

    for(let i = 0; i < projects.length; i++) {
        let a = projects[i];
        let parents = [];
        let children = [];
        for(let j= 0; j < dependencies.length; j++) {
            if(dependencies[i][0] === a) {
                children.push(dependencies[i][1]);
            }
            if(dependencies[i][1] === a) {
                parents.push(dependencies[i][0]);
            }
        }

        projectNodes.push(new Project(a, parents, children));
    }

    let buildOrder = [];

    for(let i = 0; i < projectNodes.length; i++) {
        if(projectNodes[i].parents.length === 0) {
            buildOrder.push(projectNodes[i].project)
        }
    }

    let i = 0;
    while(buildOrder.length < projects.length) {
        let children = buildOrder[i].children;
        if(children.length > 0) {
            for(let child in children) {
                if(buildOrder.indexOf(child.project) > -1)
                    buildOrder.push(projectNodes[projectNodes.indexOf(child.project)])
            }
        }
        i++;
    }
}


```

### 4.8 First Common Ancestor
> Design an algorithm and write code to find the first common ancestor of two nodes in a binary tree. Avoid storing additional nodes in a data structure. NOTE: This is not necessarily a binary search tree.

Brute-Force Algorithm:
1. Start at root and dfs for both nodes (should return true) so maybe skip this part
2. dfs on the left subtree and the right subtree
    1. if dfs returns false return parent
    2. if dfs is true, then dfs on left and right subtree again and so on
Another explanation:
1. Traverse through tree depth by depth like breadth first search
2. At each node, run dfs to find both nodes
3. Keep track of the nodes in which dfs finds both nodes to be found
4. If dfs does not find both nodes in one row, the first common ancestor is in the height above
    1. return the first common ancestor in the row above

```
dfs(root, searchFor) {
    if(root === null) return;
    if(root === searchFor) {
        return true;
    } else {
        dfs(root.left, searchFor);
        dfs(root.right, searchFor);
    }
}

firstCommonAncestor(root, nodeA, nodeB) {
    let isLeft = dfs(root.left, nodeA);
    let isRight = dfs(root.right, nodeA);

    if(isLeft && isRight)
        return root;
    
    if(isLeft && !isRight)
        firstCommonAncestor(root.left, nodeA, nodeB)
    else if(isRight && !isLeft)
        firstCommonAncestor(root.right, nodeA, nodeB)
}
```

### 4.9 BST Sequences
> A binary search tree was created by traversing through an array from left to right and inserting each element. Given a binary search tree with distinct element, print all possible arrays that could have led to this tree.

Algorithm:
1. The root is always going to be the first value in the array
2. The left subtree can be inserted first
3. The right subtree can also be inserted first
4. Therefore, you can traverse the tree in order of root, right, left or root, left, right

```
traverseLeft(node) {
    if(node === null) return;
    else
        print(node.val);
        print(traverseLeft(node.left))
        print(traverseLeft(node.right))
}
traverseRight(node) {
    if(node === null) return;
    else
        print(node.val);
        print(traverseLeft(node.right))
        print(traverseLeft(node.left))
}
```

```
BSTsequences(node) {
    traverseLeft(node);
    traverseRight(node);
}
```

### 4.10 Check Subtree - Incomplete
> T1 and T2 are two very large binary trees, with T1 much bigger than T2. Create an algorithm to determine if T2 is a subtree of T1.
> A tree T2 is a subtree of T1 if there exists a node n in T1 such that the subtree of n is identical to T2. That is, if you cut off the tree at node n, the two trees would be identical.

Brute-Force Algorithm:
1. Create function to check if two trees are equal (in value)
2. Find all nodes in T1 equal to T2. root
3. Check if those subtrees are equal


### 4.11 Random Node - Incomplete
> You are implementing a binary tree class from scratch which, in addition to insert, find, and delete, has a method getRandomNode() which returns a random node from the tree. All nodes should be equally likely to be chosen. Design and implement an algorithm for getRandomNode, and explain how you would implement the rest of the methods.

1. Keep track of the number of nodes in the binary tree
    

### 4.12 Paths with Sum
> You are given a binary tree in which each node contains an integer value (which might be positive or negative). Design an algorithm to count the number of paths that sum to a given value. The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

Algorithm:
1. Modify DFS
    1. Instead of looking for a node, it is given an integer as a parameter
    2. It will require a helper function that takes another integer value as a parameter that will act as a sum
    3. For each recursion, sum the value from the root node and the node it traverses.
    4. Return the node if the sum === the given integer value or if sum > the given integer value.
2. Run modified DFS on all nodes

```
function pathsWithSum(root, sum) {
    return dfs(root, sum, 0);
}

functions dfs(root, sum, total) {
    if(root === null) return root.val;
    if(total > sum) return 0;
    if(sum === total) {
        return 1;
    }

    return dfs(root.left, sum, total + root.val) + dfs(root.right, sum total + root.val);
}
```