# Design Patterns I
Design patterns are typical solutions to common problems in software design. Each pattern is like a blueprint that you can customize to solve a particular design problem in your code. Patterns are a toolkit of solutions to common problems in software design. They define a common language that helps your team communicate more efficiently. You can’t just find a pattern and copy it into your program, the way you can with off-the-shelf functions or libraries. The pattern is not a specific piece of code, but a general concept for solving a particular problem. You can follow the pattern details and implement a solution that suits the realities of your own program. Patterns are often confused with algorithms, because both concepts describe typical solutions to some known problems. While an algorithm always defines a clear set of actions that can achieve some goal, a pattern is a more high-level description of a solution. The code of the same pattern applied to two different programs may be different. Most patterns are described very formally so people can reproduce them in many contexts. Here are the sections that are usually present in a pattern description:
- Intent of the pattern briefly describes both the problem and the solution.
- Motivation further explains the problem and the solution the pattern makes possible.
- Structure of classes shows each part of the pattern and how they are related.
- Code example in one of the popular programming languages makes it easier to grasp the idea behind the pattern.

## History of Patterns
The concept of patterns was first described by Christopher Alexander in `A Pattern Language: Towns, Buildings, Construction`. The book describes a “language” for designing the urban environment. The units of this language are patterns. They may describe how high windows should be, how many levels a building should have, how large green areas in a neighborhood are supposed to be, and so on. The idea was picked up by four authors: Erich Gamma, John Vlissides, Ralph Johnson, and Richard Helm. In 1994, they published `Design Patterns: Elements of Reusable Object-Oriented Software`, in which they applied the concept of design patterns to programming. The book featured 23 patterns solving various problems of object-oriented design and became a best-seller very quickly. Due to its lengthy name, people started to call it “the book by the gang of four” which was soon shortened to simply “the GoF book”.

## Classification of Patterns
Design patterns differ by their complexity, level of detail and scale of applicability to the entire system being designed. All patterns can be categorized by their intent, or purpose. This book covers three main groups of patterns:
- Creational patterns provide object creation mechanisms that increase flexibility and reuse of existing code.
- Structural patterns explain how to assemble objects and classes into larger structures, while keeping these structures flexible and efficient.
- Behavioral patterns take care of effective communication and the assignment of responsibilities between objects.

### Creational Patterns
Creational design patterns provide various object creation mechanisms, which increase flexibility and reuse of existing code. Below are some of the most popular creational patterns:
- Factory Method
- Singleton
- Builder
- Prototype
- Abstract Factory

#### Builder Pattern
Builder Pattern helps in building an object step by step. The pattern allows you to produce different types and representations of an object using the same construction code. In a regular manner we have something like this:
```java
class Tea
{
		private String type;
		private int sugar;
		private int milk;
		private String size;

		Tea(String type, int sugar, int milk, String size)
		{
			this.type = type;
			this.sugar = sugar;
			this.milk = milk;
			this.size = size;
		}
		public String toString()
		{
			return this.size + " " + this.type + " tea with " + this.sugar + " sugar, " + this.milk + " milk "; 
		}
}
	
public class Driver 
{
		public static void main(String[] args) 
		{
			Tea karak = new Tea("Karak", 1, 1, "Large");
			System.out.println(karak.toString());

			Tea tea = new Tea("Regular", 0, 0, "");
			System.out.println(tea.toString());
		}
}
```
The above has a problem because, it would require us to pass all the parameters even if I do not want to. We can update my Tea as below if I want to pass only type and milk.

```java
public class Tea
{
		private String type;
		private int sugar;
		private int milk;
		private String size;

		Tea(String type, int sugar, int milk, String size)
		{
			this.type = type;
			this.sugar = sugar;
			this.milk = milk;
			this.size = size;
		}
		Tea(String type, int milk)
		{
			this.type = type;
			this.milk = milk;
			this.size = "Medium";
		}
		public String toString()
		{
			return this.size + " " + this.type + " tea with " + this.sugar + " sugar, " + this.milk + " milk "; 
		}
}
```
What if I want to provide only type and sugar, it wont allow as I already have a constructor with String and int. Below will give me an error
```java
public class Tea
{
		private String type;
		private int sugar;
		private int milk;
		private String size;

		Tea(String type, int sugar, int milk, String size)
		{
			this.type = type;
			this.sugar = sugar;
			this.milk = milk;
			this.size = size;
		}
		// This constructor is similar to the one below it
		Tea(String type, int milk)
		{
			this.type = type;
			this.milk = milk;
			this.size = "Medium";
		}
		// This constructor is similar to the one above it
		Tea(String type, int sugar)
		{
			this.type = type;
			this.milk = sugar;
			this.size = "Medium";
		}

		public String toString()
		{
			return this.size + " " + this.type + " tea with " + this.sugar + " sugar, " + this.milk + " milk "; 
		}
}
```	
Builders come to rescue. We create our object step by step:

`Tea.java`
```java	
public class Tea 
{
		private String type;
		private int sugar;
		private int milk;
		private String size;
		
		Tea(String type, int sugar, int milk, String size) {
			this.type = type;
			this.sugar = sugar;
			this.milk = milk;
			this.size = size;
		}

		@Override
		public String toString() {
			return this.size + " " + this.type + " tea with " + this.sugar + " sugar, " + this.milk + " milk ";
		}

}
```
`TeaBuilder.java`
```java
public class TeaBuilder 
{
		String type;
		int sugar;
		int milk;
		String size;
		
		public TeaBuilder()
		{
			this.type = "Tea";
			this.sugar = 0;
			this.milk = 0;
			this.size = "Medium";
		}
		
		public TeaBuilder setType(String type) 
		{
			this.type = type;
			return this;
		}
		public TeaBuilder setSugar(int sugar) 
		{
			this.sugar = sugar;
			return this;
		}
		public TeaBuilder setMilk(int milk) 
		{
			this.milk = milk;
			return this;
		}
		public TeaBuilder setSize(String size) 
		{
			this.size = size;
			return this;
		}
		
		public Tea build()
		{
			return new Tea(type, sugar, milk, size);

		}

}
```	
`Driver.java`
```java
public class Driver 
	{
		public static void main(String[] args) 
		{
			Tea karak = new TeaBuilder().setType("Karak").setMilk(1).build();
			System.out.println(karak);

			Tea black = new TeaBuilder().setType("Black").setSize("Large").build();
			System.out.println(black);
		}

}
```
#### StringBuilder in Java
Let's see another example of builder. Let's assume we have a String below and we want to modify it:
```java
	public class Driver {

		public static void main(String[] args) 
		{
			String str = "Central Michigan University";
			System.out.println(str);
			
			str[0] = "c";
		}

	}
```	
The above will give me a compile time error as I cannot change Strings like arrays. Let's add something to this string using +=
```java
public class Driver {

		public static void main(String[] args) 
		{
	        String str = "Central Michigan University";
	        System.out.println("Before: " + str);
	        System.out.println("HashCode before: " + System.identityHashCode(str));
	        
	        str += ", Mt. Pleasant";
	        
	        System.out.println("After: " + str);
	        System.out.println("HashCode after: " + System.identityHashCode(str));
    	}

}
```
The above program does not change the original str, instead creates a new string and copies the content of the old + add the new content to it. Strings in Java are immutable. This means that once a String object is created, it cannot be changed. Strings are heavily used in File paths, Database connections, Network connections, Authentication and Class loading etc. If Strings were mutable, malicious code could change them after validation. Immutability prevents tampering.
	
Having said, there are scenarios where creating new objects are not desirable. In order to achieve something like this, Java provides StringBuilder class. StringBuilder is mutable, meaning you can modify the contents without creating a new object. This leads to more efficient memory usage and performance when concatenating or modifying strings multiple times.
```java
public class Driver 
{

		public static void main(String[] args) 
		{
	        StringBuilder sb = new StringBuilder("Central Michigan University");
	        System.out.println("Before: " + sb);
	        System.out.println("HashCode before: " + System.identityHashCode(sb));
	        
	        sb.append(", Mt. Pleasant");
	        
	        System.out.println("After: " + sb);
	        System.out.println("HashCode after: " + System.identityHashCode(sb));
    	}

}
```	
StringBuilder uses Builder design pattern. Below is a simple example of how StringBuilder supports Builder design pattern:
```java

public class Driver {

    public static void main(String[] args) 
    {
    	StringBuilder str = new StringBuilder("Central Michigan University");
    	System.out.println("Before: " + str);
    	System.out.println("Hashcode Before: " + System.identityHashCode(str));
    	
		// append return the current object and allows building step by step.
    	str.append(", Mt. Pleasant").append(", Michigan").append("USA");
    	
    	System.out.println("After: " + str);
    	System.out.println("Hashcode After: " + System.identityHashCode(str));
    	
    }
}

```
Below are some of the pros and cons of Builder pattern:
- Pros
	- Step by Step object creation
	- Reusability of same construction code
	- Isolation of complex construction code
- Cons
	- Creation of multiple classes

#### Singleton Pattern
The Singleton Design Pattern ensures that a class has only one instance and provides a global point of access to that instance.

`Printer.java`
```java	
public class Printer 
`{
		Printer()
		{
			System.out.println("Printer Initialized");
		}
		
		public void print(String document)
		{
			System.out.println(document + " is printing");
		}

}
```	
`Driver.java`
```java	
public class Driver 
{
		public static void main(String[] args) 
		{
			Printer printer1 = new Printer();
			printer1.print("Document 1");
			
			Printer printer2 = new Printer();
			printer1.print("Document 2");
			
			System.out.println("Both printers are the same: " + (printer1 == printer2));
			
		}

}
```	
In the above code, a separate instance of printer class would have following issues:
- Printer is initialized for each instance. What if the initialization requires expensive / heavy codebase, e.g., connecting to the Internet or Database
- If Printer has some internal settings, each instance would have to maintain those internal settings for each instance
- What if multiple people print at the same time? This would create issues of random print mixup.
		
Above problems can be solved using SingleTon Pattern which helps in creating a single instance of a Printer and used across multiple requests:
	
`Printer.java`
```java		
public class Printer 
{
			private static Printer printer;
			
			private Printer()
			{
				System.out.println("Printer Initialized");
			}
			
			public static Printer getInstance()
			{
				if (printer == null)
				{
					printer = new Printer();
				}
				
				return printer;
			}
			
			public void print(String document)
			{
				System.out.println(document + " is printing");
			}

}
```		
`Driver.java`
```java	
public class Driver 
{
			public static void main(String[] args) 
			{
				Printer printer1 = Printer.getInstance();
				printer1.print("Document 1");
				
				Printer printer2 = Printer.getInstance();
				printer1.print("Document 2");
				
				System.out.println("Both printers are the same: " + (printer1 == printer2));
				
			}

}
```		
Using SingleTon Pattern:
- Only one instance of Printer is created, ensuring that resources are managed efficiently
- All parts of the application access the same Printer instance, ensuring consistent settings and behavior across the application
- The singleton instance can be accessed from anywhere in the application, simplifying how the printer is used
- Note how constructor of Printer class is private not public.

#### Logger Singleton
Let's see another example of SingleTon Pattern:

`Logger.java`
```java	
	import java.io.FileWriter;
	import java.io.IOException;
	import java.time.LocalDateTime;
	import java.time.format.DateTimeFormatter;

	public class Logger 
	{
		private static Logger logger;
		FileWriter writer;
		
		private Logger()
		{
			try 
			{
				
				writer = new FileWriter("app.log", true);
			} catch (IOException e) 
			{
				e.printStackTrace();
			}
		}
		
		public static Logger getInstance()
		{
			if (logger == null)
			{
				logger = new Logger();
			}
			return logger;
		}

		public void log(String message)
		{
			try 
			{
				String timestamp = LocalDateTime.now().format(DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss"));
				writer.write(timestamp + " - " + message + "\n");
			} 
			catch (IOException e) 
			{
				e.printStackTrace();
			}
		}
		public void close()
		{
			try 
			{
				writer.close();
			} 
			catch (IOException e) 
			{
				e.printStackTrace();
			}
		}
	}
```	
`Driver.java`
```java	
	public class Driver 
	{
		public static void main(String[] args) 
		{
			Logger logger = Logger.getInstance();
			
			logger.log("Application started");
			logger.log("Performing operations");
			logger.log("Application ended");
			
			logger.close();
			
		}

	}
```	
Logger SingleTon makes sure:
- that only one logger instance exists, preventing conflicts when multiple parts of the application try to log messages simultaneously.
- the logger can be accessed from anywhere in the application without creating multiple instances.
- to centralizes logging functionality in one place, making it easy to maintain and modify.

## References
- https://refactoring.guru/design-patterns
