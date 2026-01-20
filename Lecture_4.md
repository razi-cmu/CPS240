# Overview of Data Structures (Review CPS 181)

## Binary Search Trees
A binary tree is a hierarchical structure. It either is empty or consists of an element, called the root, and two distinct binary trees, called the left subtree and right subtree, either or both of which may be empty. A special type of binary tree called a binary search tree (BST) is often useful. A BST (with no duplicate elements) has the property that for every node in the tree, the value of its left child is less than the value of the node, and the value of its right child is greater than the value of the node. A binary search tree is more efficient than a list for search, insertion, and deletion operations. <br>
A binary tree can be represented using linked nodes. Each node contains a value and two links named left and right that reference the left child and right child, respectively. We use the variable `root` to refer to the root node of the tree. If the tree is empty, `root` is `null`.

Binary Search Trees can be implemented using recursive and iterative approach. Below are the steps for the iterative approach. 
- Step 1: Create a new node containing the given value.
- Step 2: Check if the binary search tree is empty (the root is null).
- Step 3: If the tree is empty, set the new node as the root and stop.
- Step 4: If the tree is not empty, start traversing from the root node.
- Step 5: Keep track of the current node and its parent.
- Step 6: Compare the value to be inserted with the current node’s value.
- Step 7: If the value is smaller, move to the left child; otherwise, move to the right child.
- Step 8: Repeat the comparison and traversal until a null child is reached.
- Step 9: Insert the new node as the left or right child of the parent node based on the comparison.
- Step 10: The new value is now correctly placed according to binary search tree rules.

Let's implement a Binary Search Tree and insert some elements to it.

`BinarySearchTree.java`
```java
class Node
{
    int value;
    Node left;
    Node right;

    Node(int value)
    {
        this.value = value;
    }
}

public class BinarySearchTree 
{
    Node root;

    public void insertNode(int value)
    {
        Node node = new Node(value);

        if (root == null)
        {
            root = node;
        }
        else
        {
            Node currentNode = root;
            Node parent;
            while (true)
            {
                parent = currentNode;
                if (value < currentNode.value)
                {
                    currentNode = currentNode.left;
                    if (currentNode == null)
                    {
                        parent.left = node;
                        return;
                    }
                }
                else
                {
                    currentNode = currentNode.right;
                    if (currentNode == null)
                    {
                        parent.right = node;
                        return;
                    }
                }
            }
        }
    }
}
```

### Printing Binary Search Tree
In order to print the elements of a Binary Search Tree, we can use an iterative approach that prints all the elements of the left subtree, then root and finally the right subtree. Below are the steps for printing the elements of a Binary Search Tree

- Step 1: Create an empty stack to help with tree traversal.
- Step 2: Set the current node to the root of the binary search tree.
- Step 3: Continue the process while the current node is not null or the stack is not empty.
- Step 4: If the current node is not null, push it onto the stack and move to its left child.
- Step 5: Repeat pushing nodes and moving left until a null node is reached.
- Step 6: When the current node is null, pop a node from the stack.
- Step 7: Print the value of the popped node.
- Step 8: Move to the right child of the popped node.
- Step 9: Repeat the process until all nodes have been visited.
- Step 10: All nodes are printed in ascending order (in-order traversal).

Let's write code for printing tree nodes.
```java
public void printNodes()
{
        Stack<Node> stack = new Stack<>();
        Node currentNode = root;

        while (currentNode!=null || !stack.empty())
        {
            if (currentNode!=null)
            {
                stack.push(currentNode);
                currentNode = currentNode.left;
            }
            else
            {
                currentNode = stack.pop();
                System.out.println(currentNode.value);
                currentNode = currentNode.right;
            }
        }
}

```
`Driver.java`

```java
public class Driver {

    public static void main(String[] args) {
        BinarySearchTree tree = new BinarySearchTree();
        tree.insertNode(5);
        tree.insertNode(10);
        tree.insertNode(2);
        tree.insertNode(11);
        tree.insertNode(1);
        tree.insertNode(3);
        tree.insertNode(20);
        tree.insertNode(15);
        tree.insertNode(90);

        tree.printNodes();
    }
}
```
### Binary Search Tree Search Operation
Binary Search Trees are very efficient when it comes to searching as they'll only search half of the tree pretty much similar to binary search. Below are the steps for searching an element in a BST.

- Step 1: Start the search from the root node of the binary search tree.
- Step 2: Compare the target value with the value of the current node.
- Step 3: If the values are equal, the node has been found.
- Step 4: If the target value is smaller than the current node’s value, move to the left child.
- Step 5: If the target value is greater than the current node’s value, move to the right child.
- Step 6: Repeat the comparison and traversal until the value is found or a null node is reached.
- Step 7: If a null node is reached, return null to indicate the value is not in the tree.
- Step 8: If the value is found, return the node containing that value.

Below is the implementation of the search function.
```java
public Node search(int value)
{
        Node currentNode = root;

        while (currentNode.value != value)
        {
            if (value < currentNode.value)
            {
                currentNode = currentNode.left;
            }
            else
            {
                currentNode = currentNode.right;
            }

            if (currentNode == null)
            {
                return null;
            }
        }
        return currentNode;
}
```

### Binary Search Tree Insertion using Recursion
Recursion can be used to simplify the code for insertion as below:
```java
    public void insertion(int value)
    {
        root = insertNode_recursion(root, value);
    }

    public Node insertNode_recursion(Node root, int value)
    {
        if (root == null)
        {
            root = new Node(value);
            return root;
        }
        else if (value < root.value)
        {
            root.left = insertNode_recursion(root.left, value);
        }
        else
        {
            root.right = insertNode_recursion(root.right, value);
        }
        return root;
    }
```
### Binary Search Tree Search using Recursion
Recursion can be used for Search as well
```java
    public Node search_recursion(Node root, int value)
    {
        if (root == null || root.value == value)
        {
            return root;
        }
        if (value < root.value)
        {
            return search_recursion(root.left, value);
        }
        else
        {
            return search_recursion(root.right, value);
        }
    }

    public void search_it(int value)
    {
        if (search_recursion(root, value) == null)
        {
            System.out.println("\nElement not found");
        }
        else
        {
            System.out.println("\nElement found");
        }
    }
```
## References:
- Introduction to Java Programming and Data Structures, 13th edition, by Y Daniel Liang,
    - Chapter 25 (Binary Search Tree)

- https://www.geeksforgeeks.org/java/java-program-to-construct-a-binary-search-tree/
