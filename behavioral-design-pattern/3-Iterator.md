# Iterator Pattern

Let us consider the following example.

## Tree Data Structure Example

Let's create an example using the Iterator pattern to traverse a binary tree structure. This will illustrate how the pattern can be applied to collections such as trees.

### Without Iterator Pattern

In this example, we'll implement a simple binary tree and a method to traverse it in-order (left, root, right) without using the Iterator pattern.

```cpp
#include <iostream>
#include <memory>

class TreeNode {
public:
    int value;
    std::shared_ptr<TreeNode> left;
    std::shared_ptr<TreeNode> right;

    TreeNode(int val) : value(val), left(nullptr), right(nullptr) {}
};

class BinaryTree {
public:
    std::shared_ptr<TreeNode> root;

    BinaryTree() : root(nullptr) {}

    void insert(int val) {
        root = insertRec(root, val);
    }

    void inOrderTraversal(std::shared_ptr<TreeNode> node) const {
        if (node == nullptr) return;
        inOrderTraversal(node->left);
        std::cout << node->value << " ";
        inOrderTraversal(node->right);
    }

private:
    std::shared_ptr<TreeNode> insertRec(std::shared_ptr<TreeNode> node, int val) {
        if (node == nullptr) return std::make_shared<TreeNode>(val);
        if (val < node->value) node->left = insertRec(node->left, val);
        else if (val > node->value) node->right = insertRec(node->right, val);
        return node;
    }
};

int main() {
    BinaryTree tree;
    tree.insert(10);
    tree.insert(5);
    tree.insert(15);
    tree.insert(3);
    tree.insert(7);

    std::cout << "In-order traversal: ";
    tree.inOrderTraversal(tree.root);
    std::cout << std::endl;

    return 0;
}
```

### With the Iterator Pattern

Now let's refactor the code to separate the traversal logic using the Iterator pattern. We'll create a TreeIterator class to encapsulate the traversal of the binary tree.

```cpp
#include <iostream>
#include <memory>
#include <stack>

class TreeNode {
public:
    int value;
    std::shared_ptr<TreeNode> left;
    std::shared_ptr<TreeNode> right;

    TreeNode(int val) : value(val), left(nullptr), right(nullptr) {}
};

class BinaryTree {
public:
    std::shared_ptr<TreeNode> root;

    BinaryTree() : root(nullptr) {}

    void insert(int val) {
        root = insertRec(root, val);
    }

private:
    std::shared_ptr<TreeNode> insertRec(std::shared_ptr<TreeNode> node, int val) {
        if (node == nullptr) return std::make_shared<TreeNode>(val);
        if (val < node->value) node->left = insertRec(node->left, val);
        else if (val > node->value) node->right = insertRec(node->right, val);
        return node;
    }
};

class TreeIterator {
private:
    std::stack<std::shared_ptr<TreeNode>> stack;

    void pushLeft(std::shared_ptr<TreeNode> node) {
        while (node) {
            stack.push(node);
            node = node->left;
        }
    }

public:
    TreeIterator(std::shared_ptr<TreeNode> root) {
        pushLeft(root);
    }

    bool hasNext() const {
        return !stack.empty();
    }

    std::shared_ptr<TreeNode> next() {
        std::shared_ptr<TreeNode> node = stack.top();
        stack.pop();
        if (node->right) {
            pushLeft(node->right);
        }
        return node;
    }
};

int main() {
    BinaryTree tree;
    tree.insert(10);
    tree.insert(5);
    tree.insert(15);
    tree.insert(3);
    tree.insert(7);

    TreeIterator iterator(tree.root);

    std::cout << "In-order traversal using Iterator: ";
    while (iterator.hasNext()) {
        std::cout << iterator.next()->value << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

**Benefits of Using the Iterator Pattern:**

- **Encapsulation:** The traversal logic is encapsulated within the TreeIterator, making the BinaryTree class cleaner and easier to maintain.
- **Reusability:** The TreeIterator can be reused for different operations (e.g., counting nodes, finding a specific node) without modifying the tree structure.
- **Flexibility:** The Iterator pattern allows different traversal strategies (e.g., pre-order, post-order) by creating different iterator classes, all while keeping the tree structure unchanged.

## Description

The Iterator pattern is a design pattern that provides a way to access elements of a collection (e.g., an array or list) sequentially without exposing its underlying representation. This pattern is useful when you need to traverse a complex data structure without knowing its internal details.

## Class Diagram

