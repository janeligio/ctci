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

### 3.1