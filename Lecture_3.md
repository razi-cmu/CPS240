# Overview of Data Structures (Review CPS 181)

## Linked Lists
A linked list is implemented using a linked structure. A Link List is a series of nodes connected together. In a linked list, each element is contained in an object, called the node. When a new element is added to the list, a node is created to contain it. Each node is linked to its next neighbor.
We use the variable `head` to refer to the first node in the list and the variable `tail` to the last node. If the list is empty, both `head` and `tail` are `null`. Below are the steps for creating a Linked List. <br>

- Step 1: Create a new node and store the given value in it.
- Step 2: Check if the linked list is empty (the `head` is null).
- Step 3: If the list is empty, make the new node the head of the linked list.
- Step 4: If the list is not empty, start from the head node.
- Step 5: Traverse the list by moving to the next node until the last node is reached.
- Step 6: Link the last node to the new node by updating its next reference.
- Step 7: The new node is now inserted at the end of the linked list.

Let's implement a Linked List from scratch.


`LinkedList.java`
```java
class Node
{
    public int data;
    public Node next;

    public Node(int data)
    {
        this.data = data;
    }
}

public class LinkedList {

    Node head;

    LinkedList()
    {
        this.head = null;
    }

    public void insertElement(int number)
    {
        Node node = new Node(number);

        if (this.head == null)
        {
            this.head = node;
        }
        else
        {
            Node currentNode = this.head;
            while (currentNode.next != null)
            {
                currentNode = currentNode.next;
            }
            currentNode.next = node;
        }
    }
    
}
```
`Driver.java`
```java
public class Driver {
    public static void main(String[] args) {
        LinkedList list = new LinkedList();
        list.insertElement(2);
        list.insertElement(4);
        list.insertElement(6);
        list.insertElement(8);
        list.insertElement(10);

        list.print();
    }
}
```
### Printing a Linked List
We can traverse through Linked List nodes and print them one by one. We can follow the below steps:

- Step 1: Start from the head node of the linked list.
- Step 2: Check if the current node is not null.
- Step 3: Print the data stored in the current node.
- Step 4: Move to the next node in the linked list.
- Step 5: Repeat the process until the end of the list is reached.
- Step 6: All elements of the linked list are printed in order.

Below is the implementation of the print method.
```java
public void print()
{
        Node currentNode = this.head;

        while (currentNode != null)
        {
            System.out.println(currentNode.data);
            currentNode = currentNode.next;
        }
}
```

### Linked List Search Operation
Linked List can perform operations like search. Below are the steps to perform a search on a Linked List.

- Step 1: Initialize a boolean variable to track whether the value has been found.
- Step 2: Start from the head node of the linked list.
- Step 3: Check if the current node is not null.
- Step 4: Compare the data in the current node with the value being searched for.
- Step 5: If the values match, mark the value as found and stop the search.
- Step 6: If the values do not match, move to the next node in the list.
- Step 7: Repeat the process until the end of the list is reached or the value is found.
- Step 8: Return the result indicating whether the value was found.

Let's implement the search operation.

```java
public Boolean search(int number)
    {
        Boolean found = false;

        Node currentNode = this.head;

        while (currentNode != null)
        {
            if (currentNode.data == number)
            {
                found = true;
                break;
            }
            currentNode = currentNode.next;
        }

        return found;
    }
```
### Linked List Delete Operation
We can delete elements from the linkedlist as well. An important thing to note here is to connect the previous node to the next node of the deleted node to make sure that Linked List is connected. Below are the steps:

- Step 1: Start by setting two pointers, one for the current node and one for the previous node, both initially pointing to the head of the list.
- Step 2: Check if the linked list is empty.
- Step 3: If the list is empty, display a message indicating that there are no elements to delete.
- Step 4: Check if the value to be deleted is stored in the head node.
- Step 5: If the head node contains the value, update the head to point to the next node.
- Step 6: Traverse the linked list while the current node is not null.
- Step 7: If the current node’s value does not match the value to be deleted, move both pointers forward.
- Step 8: If the current node’s value matches the value to be deleted, link the previous node to the next node, skipping the current node.
- Step 9: Remove the current node by setting it to null.
- Step 10: The node containing the specified value is now deleted from the linked list.

Below is the implementation code for the complete Linked List.
`LinkedList.java`
```java
class Node
{
    public int data;
    public Node next;

    public Node(int data)
    {
        this.data = data;
    }
}


public class LinkedList {

    Node head;

    LinkedList()
    {
        this.head = null;
    }

    public void insertElement(int number)
    {
        Node node = new Node(number);

        if (this.head == null)
        {
            this.head = node;
        }
        else
        {
            Node currentNode = this.head;
            while (currentNode.next != null)
            {
                currentNode = currentNode.next;
            }
            currentNode.next = node;
        }
    }
    public void print()
    {
        Node currentNode = this.head;

        if (this.head == null)
        {
            System.out.println("List is empty");
        }

        while (currentNode != null)
        {
            System.out.println(currentNode.data);
            currentNode = currentNode.next;
        }
    }

    public Boolean search(int number)
    {
        Boolean found = false;

        Node currentNode = this.head;

        while (currentNode != null)
        {
            if (currentNode.data == number)
            {
                found = true;
                break;
            }
            currentNode = currentNode.next;
        }

        return found;
    }

    public void delete(int number)
    {
        Node currentNode = this.head;
        Node previousNode = this.head;

        if (this.head == null)
        {
            System.out.println("List is empty");
        }

        if (head.data == number)
        {
            this.head = this.head.next;
        }

        while (currentNode != null)
        {
            if (currentNode.data != number)
            {
                previousNode = currentNode;
                currentNode = currentNode.next;
            }
            else
            {
                previousNode.next = currentNode.next;
                currentNode = null;
            }
        }
        
    }
    
}
```
`Driver.java`
```java
public class Driver {
    public static void main(String[] args) {
        LinkedList list = new LinkedList();
        list.insertElement(2);
        list.insertElement(4);
        list.insertElement(6);
        list.insertElement(8);
        list.insertElement(10);

        System.out.println("Before deleting...");
        list.print();

        list.delete(2);
        list.delete(6);
        list.delete(8);
        list.delete(4);
        list.delete(10);
        System.out.println("After deleting...");
        list.print();
    }
}
```
## References:
- Introduction to Java Programming and Data Structures, 13th edition, by Y Daniel Liang,
    - Chapter 24 (Implementing Lists, Stacks, Queues and Priority Queues)
        - 24.4 : Linked List

- https://www.geeksforgeeks.org/java/linked-list-in-java/
