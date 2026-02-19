# Design Patterns II
As discussed in the previous lecture, Design patterns are typical solutions to common problems in software design. Each pattern is like a blueprint that you can customize to solve a particular design problem in your code. We talked about creational design patterns in the previous lecture. This lecture is mostly dedicated to Structural and Behavioral design patterns.

## Structural Design Patterns
Structural design patterns explain how to assemble objects and classes into larger structures, while keeping these structures flexible and efficient. These design patterns deal with class and object composition and define ways to compose objects for new functionality. Below are some of the popular structural design patterns:
- Adapter
- Composite
- Proxy
- Bridge
- Decorator

### Adapter Pattern
Adapter Patterns help in making incompatible types work together. An adapter wraps one of the objects to hide the complexity of conversion happening behind the scenes. The wrapped object isn’t even aware of the adapter. For example, you can wrap an object that operates in meters and kilometers with an adapter that converts all of the data to imperial units such as feet and miles. Adapters can not only convert data into various formats but can also help objects with different interfaces collaborate.
Let's take an example of Adapter patterns. Let's assume we have a sensor that give us temperature in celsius and we have a display that shows us temperature only in Fahrenheit. Since both the sensor and the display cannot directly talk to each other, we can create an adapter for these two incompatible types. For simplicity, all the interfaces and classes are placed in the same `Driver.java` class, but you should always put them in their separate java files.


`Driver.java`
```java	
interface Sensor
{
	double getTempCelsius();
}

class Celsius implements Sensor
{

	@Override
	public double getTempCelsius() 
	{
		return 25.0;
	}	
}

interface FahrenheitTemp
{
	double getTempFahrenheit();
}

class FahrenheitAdapter implements FahrenheitTemp
{
	Sensor sensor;
	
	FahrenheitAdapter(Sensor sensor) 
	{
		this.sensor = sensor;
	}
	
	@Override
	public double getTempFahrenheit() 
	{
		double celsius = sensor.getTempCelsius();
		return celsius * (9.0/5) + 32;
	}
	
}

public class Driver {

    public static void main(String[] args) 
    {
    	Sensor sensor = new Celsius();
    	FahrenheitTemp fahrenheit = new FahrenheitAdapter(sensor);
    	
    	double temp = fahrenheit.getTempFahrenheit();
    	
    	System.out.println("25.0 Celsius in Fahrenheit is " + temp);
    }

}

```
Let's take another example of Adapter that is built-into Java.

`Driver.java`
```java	
import java.util.Arrays;
import java.util.List;

public class Driver 
{
		public static void main(String[] args) 
		{
			String[] fruits = {"Apple", "Banana", "Cherry"};
					
			System.out.println(fruits);
			
		}

}
```
The above program would simply display the object as string instead of showing the elements of the array as regular arrays do not have any built-in method for showing their elements except manually iterating through them.. We can modify the above program to use an Adapter that can help above array work with a List interface which has better methods to display elements of the array.
```java	
import java.util.Arrays;
import java.util.List;

public class Driver 
{
		public static void main(String[] args) 
		{
			String[] fruits = {"Apple", "Banana", "Cherry"};
			
			List<String> list = Arrays.asList(fruits);
			
			System.out.println(list);
			
		}

}
```

## Behavioral Design Patterns
Behavioral design patterns are concerned with algorithms and the assignment of responsibilities between objects. These design patterns deal with Class and Object communication. Below are some of the popular behavioral design patters:
- Chain of Responsibility
- Iterator
- State
- Observer
- Visitor

### Chain of Responsiblity
Chain of Responsibility is a behavioral design pattern that lets you pass requests along a chain of handlers. Upon receiving a request, each handler decides either to process the request or to pass it to the next handler in the chain. Like many other behavioral design patterns, the Chain of Responsibility relies on transforming particular behaviors into stand-alone objects called handlers. The pattern suggests that you link these handlers into a chain. Each linked handler has a field for storing a reference to the next handler in the chain. In addition to processing a request, handlers pass the request further along the chain. The request travels along the chain until all handlers have had a chance to process it. Here’s the best part: a handler can decide not to pass the request further down the chain and effectively stop any further processing.

Let's implement a chain of responsbility design pattern. Let's assume we have some purchase requests which are to be approved by each chain
- Manager handles requests < $100
- Director handles requests < $1000
- President handles requests > $5000
		
`PurchaseRequest.java`
```java	
public class PurchaseRequest
{
		private double amount;

		PurchaseRequest(double amount)
		{
			this.amount = amount;
		}
		public double getAmount()
		{
			return this.amount;
		}
}	
```	
`PurchaseHandler.java`
```java	
public interface PurchaseHandler 
{
		void setNextHandler(PurchaseHandler handler);
		void handleRequest(PurchaseRequest request);
}
```	
`Manager.java`
```java	
public class Manager implements PurchaseHandler
{
		private static final double ALLOWABLE_AMOUNT = 100;
		private PurchaseHandler nextHandler;

		@Override
		public void setNextHandler(PurchaseHandler handler) 
		{
			this.nextHandler = handler;

		}

		@Override
		public void handleRequest(PurchaseRequest request) 
		{
			if (request.getAmount()<= ALLOWABLE_AMOUNT)
			{
				System.out.println("Manager approves the request of $"+ request.getAmount());
			}
			else if (nextHandler != null)
			{
				nextHandler.handleRequest(request);
			}
			else
			{
				System.out.println("Request exceeds the allowable limit");
			}
		}
		
}
```
`Director.java`
```java	
public class Director implements PurchaseHandler
{
		private static final double ALLOWABLE_AMOUNT = 1000;
		private PurchaseHandler nextHandler;

		@Override
		public void setNextHandler(PurchaseHandler handler) 
		{
			this.nextHandler = handler;

		}

		@Override
		public void handleRequest(PurchaseRequest request) 
		{
			if (request.getAmount()<= ALLOWABLE_AMOUNT)
			{
				System.out.println("Director approves the request of $"+ request.getAmount());
			}
			else if (nextHandler != null)
			{
				nextHandler.handleRequest(request);
			}
			else
			{
				System.out.println("Request exceeds the allowable limit");
			}
		}
		
}
```
`President.java`
```java	
public class President implements PurchaseHandler
{
		private static final double ALLOWABLE_AMOUNT = 5000;

		@Override
		public void setNextHandler(PurchaseHandler handler) 
		{

		}

		@Override
		public void handleRequest(PurchaseRequest request) 
		{
			if (request.getAmount() <= ALLOWABLE_AMOUNT)
			{
				System.out.println("President approves the request of $"+ request.getAmount());
			}
			else
			{
				System.out.println("Request cannot be approved");
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
			PurchaseHandler manager = new Manager();
			PurchaseHandler director = new Director();
			PurchaseHandler president = new President();

			manager.setNextHandler(director);
			director.setNextHandler(president);

			PurchaseRequest request1 = new PurchaseRequest(100);
			PurchaseRequest request2 = new PurchaseRequest(1000);
			PurchaseRequest request3 = new PurchaseRequest(5000);

			manager.handleRequest(request1);
			manager.handleRequest(request2);
			manager.handleRequest(request3);

			
		}
}
```

Let's take another example of Chain of Responsibilities (CoR) that is built-into Java. It is not a pure example of CoR built-into Java but should give you a good understanding of how CoRs work.

`Driver.java`
```java	
package com.cmu.test;

public class Driver {

	static void level3() 
	{
		try 
		{
			level2();
		} 
		catch (ArrayIndexOutOfBoundsException ex) 
		{
			System.out.println("Array Index Out of Bound Exception caught at level 3.");
		}

	}

	static void level2() 
	{
		try 
		{
			level1();
		} 
		catch (IllegalArgumentException ex) 
		{
			System.out.println("Catching only Argument Exceptions");
		} 
		finally 
		{
			System.out.println("Exception received at level 2, throwing it to level 3");
		}
	}

	static void level1() throws ArrayIndexOutOfBoundsException {
		try 
		{
			int[] arr = new int[5];
			arr[10] = 1; // ArrayIndexOutOfBoundsException actually thrown here
		} 
		catch (NullPointerException ex) 
		{
			System.out.println("Catching only Null Exceptions");
		} 
		finally 
		{
			System.out.println("Exception occurred at level 1, throwing it to level 2");
		}

	}

	public static void main(String[] args) 
	{
		level3();

	}

}
```
At each level the exception is thrown to the upper level if it is not caught at that level. Please note that `finally` keyword is used to make sure the message is printed on the console in order for you to see the propagation at each leve.

## References
- https://refactoring.guru/design-patterns
