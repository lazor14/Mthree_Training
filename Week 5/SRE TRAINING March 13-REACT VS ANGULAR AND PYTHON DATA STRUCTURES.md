# SRE TRAINING March 13 - REACT V/S ANGULAR && PYTHON DATA STRUCTURES
## Differences Between React and Angular

| Feature         | React  | Angular  |
|---------------|--------|---------|
| Type         | Library | Framework |
| Language     | JavaScript (JSX) | TypeScript |
| Architecture | Component-based | MVC-based |
| Data Binding | One-way binding | Two-way binding |
| Performance  | Faster due to Virtual DOM | Slower due to real DOM manipulation |
| Learning Curve | Easier to learn | Steeper learning curve |
| Use Case     | Best for SPAs and lightweight applications | Best for large-scale enterprise apps |

---

## What is MVC?
MVC (Model-View-Controller) is a design pattern used for structuring applications:

- **Model**: Represents data and business logic.
- **View**: Displays data to the user.
- **Controller**: Handles user input and updates the model.

This separation allows better code maintainability and scalability.

---

## Data Binding
Data binding is a mechanism that connects UI elements with data sources, ensuring they stay synchronized. There are two types:

1. **One-way binding**: Data flows in one direction (React follows this approach).
2. **Two-way binding**: Data updates in both directions automatically (Angular uses this).

---

# Python Data Structures

## Stacks
A stack follows the **LIFO (Last In, First Out)** principle. Operations:

- **Push**: Add an element to the top.
- **Pop**: Remove the top element.
- **Peek**: View the top element.
- **isEmpty**: Check if the stack is empty.
- **Size**: Get the number of elements.

### Stack Implementation in Python:
```python
class Stack:
    def __init__(self):
        self.items = []

    def is_empty(self):
        return len(self.items) == 0

    def push(self, item):
        self.items.append(item)

    def pop(self):
        return self.items.pop() if not self.is_empty() else None

    def peek(self):
        return self.items[-1] if not self.is_empty() else None

    def size(self):
        return len(self.items)

# Example usage
stack = Stack()
stack.push(10)
stack.push(20)
print(stack.pop())  # Output: 20
```

## Queues
A queue follows the **FIFO (First In, First Out)** principle. Operations:

- **Enqueue**: Add an element to the end.
- **Dequeue**: Remove an element from the front.
- **Front**: Peek at the first element.

## Linked Lists
A linked list consists of nodes, where each node contains data and a reference to the next node.

- **Singly Linked List**: Each node points to the next node.
- **Doubly Linked List**: Nodes have pointers to both previous and next nodes.

---

# Trees
A tree is a hierarchical data structure consisting of nodes.

- **Root**: The top node of the tree.
- **Node**: An element in the tree.
- **Branch**: A connection between nodes.
- **Leaf**: A node with no children.

### Common Tree Traversal Algorithms:
1. **Inorder Traversal (Left-Root-Right)**
2. **Preorder Traversal (Root-Left-Right)**
3. **Postorder Traversal (Left-Right-Root)**
4. **Level Order Traversal**

### Binary Search Tree (BST)
A BST is a tree where each node has at most two children, and left < root < right.

#### ASCII Representation of a BST
```
        50
       /  \
     30    70
    /  \   /  \
   20  40 60  80
```

### BST Implementation in Python:
```python
class TreeNode:
    def __init__(self, value):
        self.value = value
        self.left = None
        self.right = None

    def insert(self, value):
        if value < self.value:
            if self.left:
                self.left.insert(value)
            else:
                self.left = TreeNode(value)
        else:
            if self.right:
                self.right.insert(value)
            else:
                self.right = TreeNode(value)

    def inorder_traversal(self):
        if self.left:
            self.left.inorder_traversal()
        print(self.value, end=" ")
        if self.right:
            self.right.inorder_traversal()

# Example usage
root = TreeNode(50)
root.insert(30)
root.insert(70)
root.inorder_traversal()  # Output: 30 50 70
```

## Trie
A Trie is a tree-like data structure used for storing strings efficiently.

- **Each node represents a character.**
- **Words are stored as paths from the root.**

#### ASCII Representation of a Trie (for 'hello' and 'hey')
```
        (root)
       /  |  \
      h   a   b
     /       \
    e         e
   / \        \
  l   y        l
 /             \
 l              o
/                
o                
```

### Trie Implementation in Python:
```python
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                node.children[char] = TrieNode()
            node = node.children[char]
        node.is_end_of_word = True

    def search(self, word):
        node = self.root
        for char in word:
            if char not in node.children:
                return False
            node = node.children[char]
        return node.is_end_of_word

# Example output
trie = Trie()
trie.insert("hello")
print(trie.search("hello"))  # Output: True
print(trie.search("hell"))   # Output: False
```



