# Classes and Objects in Java

Object-oriented programming enables you to develop large-scale software and GUIs effectively. It is essentially a technology for developing reusable software. 

## Classes and Objects
Object-oriented programming (OOP) involves programming using objects. An object represents an entity in the real world that can be distinctly identified. For example, a student, a desk, a circle, a button, and even a loan can all be viewed as objects. An object has a unique identity, state, and behavior.
The state of an object (also known as its properties or attributes) is represented by data fields with their current values. A circle object, for example, has a data field , which is the property that characterizes a circle. A rectangle object, for example, has the data fields  and , which are the properties that characterize a rectangle. The behavior of an object (also known as its actions) is defined by methods. To invoke a method on an object is to ask the object to perform an action. For example, you may define methods named  and  for circle objects. A circle object may invoke  to return its area and  to return its perimeter. You may also define the  method. A circle object can invoke this method to change its radius. <br>
Objects of the same type are defined using a common class. A class is a template, blueprint, or contract that defines what an object’s data fields and methods will be. An object is an instance of a class. You can create many instances of a class. Creating an instance is referred to as instantiation. The terms object and instance are often interchangeable. The relationship between classes and objects is analogous to that between an apple-pie recipe and apple pies: You can make as many apple pies as you want from a single recipe. <br>
A Java class uses variables to define data fields and methods to define actions. In addition, a class provides methods of a special type, known as constructors, which are invoked to create a new object. A constructor can perform any action, but constructors are designed to perform initializing actions, such as initializing the data fields of objects.

Let's take an example of a `Student` class that has two attributes, e.g., name and age of the student.


`Student.java`
 
```java
	public class Student 
	{
		private String name;
		private int age;
		
		Student()
		{
			name = "";
			age = 0;
		}
		
		Student(String n, int a)
		{
			name = n;
			age = a;
		}
		
		public String getName()
		{
			return name;
		}
		public int getAge()
		{
			return age;
		}
		public void setName(String n)
		{
			name = n;
		}
		public void setAge(int a)
		{
			age = a;
		}
		
		public void display()
		{
			System.out.println("Name: " + name + ", Age: " + age);
		}

	}
```

`Driver.java`

```java
	public class Driver {
		
		public static void main(String[] args) 
		{
			Student student = new Student();
			student.setName("John");
			student.setAge(22);
			student.display();
		}
		
	}
```

In OOP terminology, an object’s member refers to its data fields and methods. After an object is created, its data can be accessed and its methods can be invoked using the dot operator (.), also known as the object member access operator.

## Static Variables, Constants and Methods
A static variable is shared by all objects of the class. A static method cannot access instance members (i.e., instance data fields and methods) of the class. <br>
If you want all the instances of a class to share data, use static variables, also known as class variables. Static variables store values for the variables in a common memory location. Because of this common location, if one object changes the value of a static variable, all objects of the same class are affected. Java supports static methods as well as static variables. Static methods can be called without creating an instance of the class.

Let's update `Student` class to hold some static variables, methods and constants.

`Student.java`
```java
public class Student 
{

    private String name;
    private int age;

    private static int studentCount = 0;

    public static final String UNIVERSITY_NAME = "CMU";

    Student() 
    {
        name = "";
        age = 0;
        studentCount++;
    }

    Student(String n, int a) 
    {
        name = n;
        age = a;
        studentCount++;
    }
    public String getName() 
    {
        return name;
    }

    public int getAge() 
    {
        return age;
    }

    public void setName(String n) 
    {
        name = n;
    }

    public void setAge(int a) 
    {
        age = a;
    }

    public void display() 
    {
        System.out.println("Name: " + name + ", Age: " + age + ", University: " + UNIVERSITY_NAME);
    }

    public static int getStudentCount() 
    {
        return studentCount;
    }
}
```
`Driver.java`
```java
public class Driver 
{
	public static void main(String[] args) 
	{	
		Student s1 = new Student("John", 22);
        Student s2 = new Student("Alice", 20);
        Student s3 = new Student("Bob", 21);

        s1.display();
        s2.display();
        s3.display();

        System.out.println("Total Students: " + Student.getStudentCount());
        System.out.println("University: " + Student.UNIVERSITY_NAME);
	}
}

```
A `final` keyword is pretty handy keyword in java. It helps in avoiding accidental change of values for a variable. You assign the value to a variable and mark it `final` so that its value cannot be changed as shown in the code above where the value of `UNIVERSITY_NAME` is set to final and static. A `final` keyword can be assigned to a variable, class or a method in Java.

## this Keyword
The keyword `this` refers to the calling object or the current instance of the class. It can also be used inside a constructor to invoke another constructor of the same class. Within the body of a method in Java, the keyword `this` is automatically defined as a reference to the instance upon which the method was invoked.

`Student.java`
```java	
	public class Student 
	{
		private String name;
		private Integer age;
		
		Student()
		{
			this.name = "";
			this.age = 0;
		}
		
		Student(String name, Integer age)
		{
			this.name = name;
			this.age = age;
		}
		
		public String getName()
		{
			return this.name;
		}
		public Integer getAge()
		{
			return this.age;
		}
		public void setName(String name)
		{
			this.name = name;
		}
		public void setAge(Integer age)
		{
			this.age = age;
		}
		
		public void display()
		{
			System.out.println("Name: " + this.name + ", Age: " + this.age);
		}

	}
```
The Driver class remains the same as non-static implementation of the Student class.

## Wrapper Classes
Wrapper classes are very handy as they provide excellent ways of dealing with regular data types. Let's consider an example below:

`Driver.java`
```java	
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Driver {
		
	public static void main(String[] args) 
	{
			
			List<Student> students = new ArrayList<>();
			Scanner scanner = new Scanner(System.in);
			
			for (int i=0; i<5; i++)
			{
				System.out.print("Enter student's name: ");
				String name = scanner.nextLine();

				System.out.print("Enter student's age: ");
				int age = scanner.nextInt();
				
				Student student = new Student(name, age);
				
				students.add(student);
			}
			
			for (Student student: students)
			{
				student.display();
			}
		   
	}
}
```
The above program would give an issue as next characters in the line are still in the buffer and hence nextInt wont work as expected. We can update it as below:
	
`Student.java`
```java	
public class Student 
{
		private String name;
		private Integer age;
		
		Student()
		{
			name = "";
			age = 0;
		}
		
		Student(String n, Integer a)
		{
			name = n;
			age = a;
		}
		
		public String getName()
		{
			return name;
		}
		public Integer getAge()
		{
			return age;
		}
		public void setName(String n)
		{
			name = n;
		}
		public void setAge(Integer a)
		{
			age = a;
		}
		
		public void display()
		{
			System.out.println("Name: " + name + ", Age: " + age);
		}

}
```
`Driver.java`

```java	
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Driver {
		
	public static void main(String[] args) 
	{
			
		List<Student> students = new ArrayList<>();
		Scanner scanner = new Scanner(System.in);
			
		for (int i=0; i<5; i++)
		{
			System.out.print("Enter student's name: ");
			String name = scanner.nextLine();

			System.out.print("Enter student's age: ");
			int age = Integer.parseInt(scanner.nextLine());
			
			Student student = new Student(name, age);
				
			students.add(student);
		}
			
		for (Student student: students)
		{
			student.display();
		}
		   
	}
		
}
```
	
Another advantage of using Wrapper classes is availability of several built-in functions.

`Student.java`
```java
	public class Student 
	{
		private String name;
		private Integer age;
		
		Student()
		{
			name = "";
			age = 0;
		}
		
		Student(String n, Integer a)
		{
			name = n;
			age = a;
		}
		
		public String getName()
		{
			return name;
		}
		public Integer getAge()
		{
			return age;
		}
		public void setName(String n)
		{
			name = n;
		}
		public void setAge(Integer a)
		{
			age = a;
		}
		
		public void display()
		{
			System.out.println("Name: " + name + ", Age: " + age);
		}

	}
```

`Driver.java`
```java	
import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

public class Driver {
		
		public static Student getYoungest(List<Student> students)
		{
			Student youngest = students.get(0);
			for (Student student: students)
			{
				if (student.getAge().compareTo(youngest.getAge()) < 0)
				{
					youngest = student;
				}
			}
			
			return youngest;
		}
		
		public static void main(String[] args) 
		{
			
			List<Student> students = new ArrayList<>();
			Scanner scanner = new Scanner(System.in);
			
			for (int i=0; i<5; i++)
			{
				System.out.print("Enter student's name: ");
				String name = scanner.nextLine();

				System.out.print("Enter student's age: ");
				int age = Integer.parseInt(scanner.nextLine());
				
				Student student = new Student(name, age);
				
				students.add(student);
			}
			
			System.out.println("\nList of Students...");
			for (Student student: students)
			{
				student.display();
			}
			
			System.out.println("\nYoungest Student:");
			
			getYoungest(students).display();
			
			scanner.close();
		   
		}
		
}
```

## Access Modifiers (Visibility Modifiers)
Visibility modifiers can be used to specify the visibility of a class and its members. You can use the `public` visibility modifier for classes, methods, and data fields to denote they can be accessed from any other classes. If no visibility modifier is used, then by default the classes, methods, and data fields are accessible by any class in the same package. This is known as package-private or package-access. In addition to the  and default visibility modifiers, Java provides the `private` and `protected` modifiers for class members.<br>
The `private` modifier makes methods and data fields accessible only from within its own class. The private modifier restricts private members from being accessed outside the class. However, there is no restriction on accessing members from inside the class. Therefore, objects instantiated in its own class can access its private members. <br>
In Student class above, `name` and `age` are kept private and can only be accessed inside the class and in order to access them outside the class, we need getters and setters, e.g., `getAge()`, `setName(String n)` etc.
	
## Packages	
Packages are a way to organize Java classes and interfaces. They provide a namespace to avoid naming conflicts. It helps in putting related classes and interfaces at one place.

`Graduate.java`
```java	
package com.cmu.Graduate;

public class Student 
	{
		private String name;
		private int age;
		private String thesis;
		
		public Student(String name, int age, String thesis)
		{
			this.name = name;
			this.age = age;
			this.thesis = thesis;
		}
		
		public String toString()
		{
			return this.name + ", " + this.age + ", " + this.thesis;
		}

}
```
`Undergraduate.java`

 ```java
package com.cmu.Undergraduate;

public class Student 
{
		private String name;
		private int age;
		
		public Student(String name, int age)
		{
			this.name = name;
			this.age = age;
		}
		
		public String toString()
		{
			return this.name + ", " + this.age;
		}

}
```

`Main.java`

```java	
package com.cmu.Driver;

import com.cmu.Graduate.Student;

public class Main {
		
		public static void main(String[] args) 
		{
			Student graduate = new Student("Steve", 26, "Internet of Things");
			com.cmu.Undergraduate.Student undergraduate = new com.cmu.Undergraduate.Student("John", 22);
				
			System.out.println(graduate);
			System.out.println(undergraduate);
			
			
		}

}
```

## Inner Classes
Inner Classes is valuable technique when implementing data structures. An inner class is a class declared inside the body of another class. The inner class has access to all members (including private) of the outer class, but the outer class can access the inner class members only through an object of the inner class.

`Student.java`
```java	
	package com.cmu;

	import java.util.Arrays;

	public class Student 
	{
		String name;
		int age;
		
		Student(String name, int age)
		{
			this.name = name;
			this.age = age;
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
		
		void display()
		{
			System.out.println("Name: " + name + ", Age: " + age);
		}
		
		public class StudentDetails
		{
			public String getFirstName()
			{
				String firstName[];
				
				firstName = name.split(" ", 2);

				return firstName[0];
			}
		}

	}
```

`Driver.java`
```java	
	package com.cmu;

	public class Driver 
	{
		public static void main(String[] args) 
		{
			Student student = new Student("Steve Allen Jobs", 55);
			student.display();
			
			Student.StudentDetails details = student.new StudentDetails();
			System.out.println(details.getFirstName());
			
		}

	}

```
