# Hashing - HashMap
Hash is a Data Structure just like arrays, linked list and trees etc. It is which is used to store large amount of data efficiently. Hashing technique consists of a Hash Table which is used to store values with keys. Hashing is different than arrays, since arrays only uses indexes for
storing value and they have fixed size. Hashing is different than Linked List since Linked List are slow in processing and can be inefficient in searching. Hashing is a combination of array and linked list. Theoretically Hash Tables can store infinite values however, managing the Hash Table is very important. Hash Table normally uses a function to store values in right places. These functions can be any but should be efficient enough.

Let's see an example implementation of HashMap in java.

`HashMap.java`
```java

class Node
{
    int key;
    int value;
    Node next;

    Node(int key, int value)
    {
        this.key = key;
        this.value = value;
    }
    int getValue()
    {
        return this.value;
    }
    int getKey()
    {
        return this.key;
    }
    void setValue(int value)
    {
        this.value = value;
    }
}

public class HashMap {

    Node table[];
    int size;

    HashMap(int size)
    {
        this.size = size;
        table = new Node[size];
    }

    void put(int key, int value)
    {
        int hash = key % size;

        Node node = table[hash];
        
        if (node == null)
        {
            // insert a new node at the beginning
            table[hash] = new Node(key, value);
        }
        else
        {
            while (node.next != null)
            {
                // update the value of the key
                if (node.getKey() == key)
                {
                    node.setValue(value);
                    return;
                }
                node = node.next;
            }

            // update the value of the first key
            if (node.getKey() == key)
            {
                node.setValue(value);
                return;
            }

            // insert a new node at the end of the list for that key
            node.next = new Node(key, value);
        }
    }
    
    int get(int key)
    {
        int hash = key % size;
        Node node = table[hash];

        if (node == null)
        {
            return -1;
        }

        while (node != null)
        {
            if (node.getKey() == key)
            {
                return node.getValue();
            }
            node = node.next;
        }
        return -1;
    }
    
}
```

`Driver.java`
```java
public class Driver 
{
    public static void main(String[] args) {
        HashMap map = new HashMap(10);
        map.put(1, 25);
        map.put(2, 38);
        map.put(3, 46);
        map.put(4, 59);
        map.put(5, 94);
        map.put(1, 98);
        int value = 381;
        int key = 6;

        
        if ((value = map.get(key)) == -1)
        {
            System.out.println("No value found at that key");
        }
        else
        {
            System.out.printf("Value at %d: %d", key, value);
        }
    }
}
```
Java has a built-in HashMap class available as well. A HashMap is a part of Java’s Collection Framework and implements the Map interface. It stores elements in key-value pairs, where, Keys are unique. and Values can be duplicated. Internally uses Hashing, hence allows efficient key-based retrieval, insertion, and removal with an average of O(1) time. HashMap is not thread-safe, to make it synchronized, use Collections.synchronizedMap(). Insertion order is not preserved in HashMap. To preserve the insertion order, LinkedHashMap is used and to maintain sorted order, TreeMap is used.

## References
- https://www.w3schools.com/java/java_hashmap.asp
- https://www.geeksforgeeks.org/java/java-util-hashmap-in-java-with-examples/