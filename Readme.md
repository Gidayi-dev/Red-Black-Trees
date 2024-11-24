# Red-Black Trees

## Overview

A **Red-Black Tree** is a self-balancing binary search tree (BST) that ensures the tree remains balanced, providing efficient operations for insertion, deletion, and search. The key to its efficiency lies in the red-black properties that govern the color and structure of its nodes.

## Why Red-Black Trees?

- **Balanced Trees**: The height of the tree is maintained to be \(O(\log n)\), where \(n\) is the number of nodes.
- **Efficient Operations**: Search, insertion, and deletion operations all have \(O(\log n)\) time complexity.
- **Widely Used**: Found in libraries like Java's `TreeMap` and C++'s `std::map`.



## Key Properties

A Red-Black Tree adheres to the following properties:

1. **Node Color**: Each node is either **red** or **black**.
2. **Root is Black**: The root of the tree is always black.
3. **Red Rule**: Red nodes cannot have red children (no two consecutive red nodes).
4. **Black Height**: Every path from a node to its descendant NULL pointers (leaves) contains the same number of black nodes.
5. **Null Pointers**: All NULL pointers are considered black.


## Implementation in JavaScript

Here’s how you can implement a basic Red-Black Tree in JavaScript:

### 1. Node Structure

Each node stores its value, references to left and right children, a reference to its parent, and a color.

```javascript
class Node {
    constructor(value) {
        this.value = value;
        this.color = "RED"; // New nodes are always red
        this.left = null;
        this.right = null;
        this.parent = null;
    }
}

```

### 2. Tree Class
The RedBlackTree class manages the operations and ensures the red-black properties are maintained.

```javascript
class RedBlackTree {
    constructor() {
        this.root = null;
    }

    // Helper method to perform a left rotation
    leftRotate(node) {
        const rightChild = node.right;
        node.right = rightChild.left;
        if (rightChild.left) rightChild.left.parent = node;

        rightChild.parent = node.parent;
        if (!node.parent) this.root = rightChild;
        else if (node === node.parent.left) node.parent.left = rightChild;
        else node.parent.right = rightChild;

        rightChild.left = node;
        node.parent = rightChild;
    }

    // Helper method to perform a right rotation
    rightRotate(node) {
        const leftChild = node.left;
        node.left = leftChild.right;
        if (leftChild.right) leftChild.right.parent = node;

        leftChild.parent = node.parent;
        if (!node.parent) this.root = leftChild;
        else if (node === node.parent.right) node.parent.right = leftChild;
        else node.parent.left = leftChild;

        leftChild.right = node;
        node.parent = leftChild;
    }

    // Fix violations after insertion
    fixInsert(node) {
        while (node.parent && node.parent.color === "RED") {
            const grandParent = node.parent.parent;

            if (node.parent === grandParent.left) {
                const uncle = grandParent.right;

                // Case 1: Uncle is red
                if (uncle && uncle.color === "RED") {
                    node.parent.color = "BLACK";
                    uncle.color = "BLACK";
                    grandParent.color = "RED";
                    node = grandParent;
                } else {
                    // Case 2: Node is a right child
                    if (node === node.parent.right) {
                        node = node.parent;
                        this.leftRotate(node);
                    }
                    // Case 3: Node is a left child
                    node.parent.color = "BLACK";
                    grandParent.color = "RED";
                    this.rightRotate(grandParent);
                }
            } else {
                const uncle = grandParent.left;

                // Case 1: Uncle is red
                if (uncle && uncle.color === "RED") {
                    node.parent.color = "BLACK";
                    uncle.color = "BLACK";
                    grandParent.color = "RED";
                    node = grandParent;
                } else {
                    // Case 2: Node is a left child
                    if (node === node.parent.left) {
                        node = node.parent;
                        this.rightRotate(node);
                    }
                    // Case 3: Node is a right child
                    node.parent.color = "BLACK";
                    grandParent.color = "RED";
                    this.leftRotate(grandParent);
                }
            }
        }
        this.root.color = "BLACK";
    }

    // Insert a value into the tree
    insert(value) {
        const newNode = new Node(value);

        if (!this.root) {
            this.root = newNode;
            this.root.color = "BLACK";
            return;
        }

        let current = this.root;
        let parent = null;

        while (current) {
            parent = current;
            if (value < current.value) current = current.left;
            else current = current.right;
        }

        newNode.parent = parent;
        if (value < parent.value) parent.left = newNode;
        else parent.right = newNode;

        this.fixInsert(newNode);
    }

    // Perform an in-order traversal
    inOrderTraversal(node = this.root) {
        if (node) {
            this.inOrderTraversal(node.left);
            console.log(`${node.value} (${node.color})`);
            this.inOrderTraversal(node.right);
        }
    }
}
```
### Example Usage
```javascript
const tree = new RedBlackTree();

tree.insert(10);
tree.insert(20);
tree.insert(30);
tree.insert(15);

console.log("In-order Traversal:");
tree.inOrderTraversal();
```
```scss
Output
10 (BLACK)
15 (RED)
20 (BLACK)
30 (RED)
```
#### Key Operations
#### 1. Insertion:

* Insert the node like a standard Binary Search Tree (BST).
* Recolor and rotate the tree to maintain red-black properties.
#### 2. Deletion:

* Deletion is more complex, involving recoloring and rotations. (Implementation omitted for brevity.)
### Advantages of Red-Black Trees
* Self-Balancing: Ensures the tree height is balanced with ``𝑂(log𝑛)`` complexity.
* Efficient Search: Search, insertion, and deletion operations are optimized to 
``O(logn)``.
* Widely Used: Used in popular libraries such as Java's TreeMap and C++'s std::map.