# Collections in Java
The `Collection` defines the common operations for lists, vectors, stacks, queues, priority queues, and sets.

## Collection Interface
The `Collection<T>` interface allows you to write code that applies to all Java collections so that you do not have to rewrite the code for each specific collection type. The `Collection` interface provides the basic operations for adding and removing elements in a collection. The `add` method adds an element to the collection. The `addAll` method adds all the elements in the specified collection to this collection. The `remove` method removes an element from the collection. The `removeAll` method removes the elements from this collection that are present in the specified collection. The `retainAll` method retains the elements in this collection that are also present in the specified collection. All these methods return `boolean`. The return value is `true` if the collection is changed as a result of the method execution. The `clear` method simply removes all the elements from the collection.

List is one of the collections that can help in implementing an ArrayList class as below:

`Driver.java`
```java	
 package com.cmu;

import java.util.ArrayList;
import java.util.List;

public class Driver 
{
		public static void main(String[] args) 
		{
			List<String> fruits = new ArrayList<>();
			fruits.add("Apple");
			fruits.add("Banana");
			fruits.add("Grapes");
			fruits.add("Cherry");
			
			// Adds an item at a specific index
			fruits.add(1, "Melon");
			
			System.out.println(fruits);
			
			// Gets an item at a specific index
			System.out.println(fruits.get(2));
			
			// Replaces an item
			fruits.set(1, "Water Melon");
			
			System.out.println(fruits);
			
			// Removes an item at a specific index
			fruits.remove(2);
			System.out.println(fruits);
			
			// Returns the size of the list 
			System.out.println("Size: " + fruits.size());
			
			// Returns true if an item is present in the list
			System.out.println("Contains: " + fruits.contains("Apple"));
		}

}
```
## Lists
The `List` interface extends the `Collection` interface and defines a collection for storing elements in a sequential order. To create a list, use one of its two concrete classes: `ArrayList` or `LinkedList`. `ArrayList` stores elements in an array. The array is dynamically created. If the capacity of the array is exceeded, a larger new array is created and all the elements from the current array are copied to the new array. `LinkedList` stores elements in a linked list. Which of the two classes you use depends on your specific needs. If you need to support random access through an index without inserting or removing elements at the beginning of the list, `ArrayList` is the most efficient. If, however, your application requires the insertion or deletion of elements at the beginning of the list, you should choose `LinkedList`. A list can grow or shrink dynamically. Once it is created, an array is fixed. If your application does not require the insertion or deletion of elements, an array is the most efficient data structure.

Lists can be passed to a function as a parameter. Below is an example of this:
Driver.java
```java	
	package com.cmu;

	import java.util.ArrayList;
	import java.util.List;

	public class Driver 
	{
		public static void display(List<String> list)
		{
			System.out.println(list);
		}
		public static void main(String[] args) 
		{
			List<String> fruits = new ArrayList<>();
			fruits.add("Apple");
			fruits.add("Banana");
			fruits.add("Grapes");
			fruits.add("Cherry");
			
			display(fruits);
		}

	}
```

`ArrayList` is a resizable-array implementation of the `List` interface. It also provides methods for manipulating the size of the array used internally to store the list. Each `ArrayList` instance has a capacity, which is the size of the array used to store the elements in the list. It is always at least as large as the list size. As elements are added to an `ArrayList`, its capacity grows automatically. An `ArrayList` does not automatically shrink. <br>

`LinkedList` is a linked list implementation of the interface. In addition to implementing the interface, this class provides the methods for retrieving, inserting, and removing elements from both ends of the list.


## Sorting an ArrayList
We can sort an ArrayList as well

`Driver.java`
```java	
package com.cmu;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Driver 
{
		public static void sort(ArrayList<Integer> list)
		{
			Collections.sort(list);
		}
		public static void main(String[] args) 
		{
			ArrayList<Integer> numbers = new ArrayList<>();
			numbers.add(2);
			numbers.add(1);
			numbers.add(3);
			numbers.add(5);
			numbers.add(4);
			
			System.out.println("Original List: " + numbers);
			
			sort(numbers);
			
			System.out.println("Modified List: " + numbers);
			
		}

}
```
The above code will sort the list in ascending order. In order to sort in descending order, we have to sort in asecending order first and then reverse the list using `Collections.reverse(list)`. However, there is a better way of sorting in descending order which is by using `Comparator`.

## Using Comparator with ArrayList
We can use a comparator to sort a List in descending order. `Comparator` can be used to compare the objects of a class that doesn’t implement `Comparable` or define a new criteria for comparing objects.

Driver.java
```java	
	package com.cmu;

	import java.util.ArrayList;
	import java.util.Collections;
	import java.util.Comparator;
	import java.util.List;

	public class Driver 
	{
		public static void sort(ArrayList<Integer> list)
		{
			Collections.sort(list, new Comparator<Integer>() {
				@Override
				public int compare(Integer o1, Integer o2) {
					return o2.compareTo(o1);
				}
			});
		}
		public static void main(String[] args) 
		{
			ArrayList<Integer> numbers = new ArrayList<>();
			numbers.add(2);
			numbers.add(1);
			numbers.add(3);
			numbers.add(5);
			numbers.add(4);
			
			System.out.println("Original List: " + numbers);
			
			sort(numbers);
			
			System.out.println("Modified List: " + numbers);
			
		}

	}
```
Since `Comperator` is a functional interface, the code can be simplified using a lambda expression.

## Linked Lists
LinkedList is a handy data structure that can be used when frequent insertion and removal are required:

`Driver.java`
```java	
	package com.cmu;

	import java.util.LinkedList;

	class Student
	{
		String name;
		int age;
		
		Student(String name, int age)
		{
			this.name = name;
			this.age = age;
		}
		
		@Override
		public String toString() {
			return "(" + name + ", " + age + ")";
		}
	}

	public class Driver 
	{
		
		public static void main(String[] args) 
		{
			LinkedList<Student> students = new LinkedList<>();
			students.add(new Student("Steve", 22));
			students.add(new Student("John", 23));
			students.add(new Student("Lizzy", 21));
			students.add(new Student("Elon", 20));
			students.add(new Student("Angela", 24));	
			
			System.out.println(students);
		}

	}
```
## Linked Lists as parameter to a function
Just like ArrayList, we can pass LinkedList to function as parameters as well

`Driver.java`
```java	
	package com.cmu;

	import java.util.LinkedList;

	class Student
	{
		String name;
		int age;
		
		Student(String name, int age)
		{
			this.name = name;
			this.age = age;
		}
		
		@Override
		public String toString() {
			return "(" + name + ", " + age + ")";
		}
	}

	public class Driver 
	{
		
		public static Student youngestStudent(LinkedList<Student> students)
		{
			Student youngest = students.get(0);
			for (Student student: students)
			{
				if (student.age < youngest.age)
				{
					youngest = student;
				}
			}
			return youngest;
		}
		
		public static void main(String[] args) 
		{
			LinkedList<Student> students = new LinkedList<>();
			students.add(new Student("Steve", 22));
			students.add(new Student("John", 23));
			students.add(new Student("Lizzy", 21));
			students.add(new Student("Elon", 20));
			students.add(new Student("Angela", 24));	
			
			System.out.println(students);
			
			System.out.println("Youngest Student: " + youngestStudent(students));
		}

	}
```
## Sorting a Linked List
Sort works for Linked List as well. However, since we are not sorting a simple list of integers, we cannot use `Collections.sort()` in its plain form. We have to use `Comparator` for sorting as it provides flexibility when it comes to dealing with objects.

`Driver.java`
```java	
package com.cmu;

import java.util.Collections;
import java.util.Comparator;
import java.util.LinkedList;

class Student
{
		String name;
		int age;
		
		Student(String name, int age)
		{
			this.name = name;
			this.age = age;
		}
		
		@Override
		public String toString() {
			return "(" + name + ", " + age + ")";
		}
}

public class Driver 
{
		
		public static Student youngestStudent(LinkedList<Student> students)
		{
			Student youngest = students.get(0);
			for (Student student: students)
			{
				if (student.age < youngest.age)
				{
					youngest = student;
				}
			}
			return youngest;
		}
		
		public static void sortStudents(LinkedList<Student> students)
		{
			Collections.sort(students, new Comparator<Student>() {
				public int compare(Student s1, Student s2) {
					return Integer.compare(s1.age, s2.age);
					
				};
			});
		}
		
		public static void main(String[] args) 
		{
			LinkedList<Student> students = new LinkedList<>();
			students.add(new Student("Steve", 22));
			students.add(new Student("John", 23));
			students.add(new Student("Lizzy", 21));
			students.add(new Student("Elon", 20));
			students.add(new Student("Angela", 24));	
			
			System.out.println(students);
			
			System.out.println("Youngest Student: " + youngestStudent(students));
			
			sortStudents(students);
			System.out.println(students);
		}

}
```

Note how `Comparator` with lambda expression saved us from writing a new class that implements `Comparator` interface. If we were to NOT use Comparator interface with lambda function, below will be the implementation which would be extra work for creating a `MyComparator` class first and then using it.
`Driver.java`
```java
package com.cmu.test;

import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.LinkedList;
import java.util.List;

class Student
{
	String name;
	int age;
	
	
	
	Student(String name, int age)
	{
		this.name = name;
		this.age = age;
	}
	public String toString()
	{
		return "(" + this.name + ", " + this.age + ")";
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public int getAge() {
		return age;
	}
	public void setAge(int age) {
		this.age = age;
	}
}

class MyComparator implements Comparator<Student>
{

	@Override
	public int compare(Student o1, Student o2) 
	{
		return Integer.compare(o2.getAge(), o1.getAge());
	}
	
}
public class Driver 
{
	public static Student youngest_student(List<Student> students)
	{
		Student youngest = students.get(0);
		
		for (Student student: students)
		{
			if (student.age < youngest.age)
			{
				youngest = student;
			}
		}
		return youngest;
	}
	
	public static void sort_students(List<Student> students)
	{
		MyComparator comp = new MyComparator();
		Collections.sort(students,comp);
	}
	public static void main(String[] args) 
	{
		List<Student> students = new LinkedList<>();
		students.add(new Student("Cunningham", 25));
		students.add(new Student("Robinson", 22));
		students.add(new Student("Thompson", 23));
		students.add(new Student("Sasser", 19));
		students.add(new Student("Stewart", 24));
		
		System.out.println(students);
		
		System.out.println("Youngest: " + youngest_student(students));
		
		sort_students(students);
		System.out.println(students);
 

	}
}

```

## References
- Introduction to Java Programming and Data Structures, 13th edition, by Y Daniel Liang,
	- Chapter 20.2 (Collections)
