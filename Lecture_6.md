# Inheritance and Polymorphism

## Inheritance
Object-oriented programming allows you to define new classes from existing classes. This is called inheritance. Java also offers inheritance for a modular and hierarchical organization. <br>
Inheritance enables you to define a general class (i.e., a superclass) and later extend it to more specialized classes (i.e., subclasses). You use a class to model objects of the same type. Different classes may have some common properties and behaviors, which can be generalized in a class that can be shared by other classes. You can define a specialized class that extends the generalized class. The specialized classes inherit the properties and methods from the general class. <br>
A superclass is also referred to as a parent class or a base class, and a subclass as a child class, an extended class, or a derived class. A subclass inherits accessible data fields and methods from its superclass and may also add new data fields and methods.

Let's see an example implementation of inheritance in Java where a parent class `Vehicle` is used to derive child class like `Car`.

Vehicle.java
```java
public class Vehicle 
{
		String name;
		int kms;

		Vehicle()
		{
			this.name = "";
			this.kms = 0;
		}

		Vehicle(String name, int kms)
		{
			this.name = name;
			this.kms = kms;
		}

		void display()
		{
			System.out.println("Name: " + this.name + ", KMs: " + this.kms);
		}
}
```

Car.java
```java
public class Car extends Vehicle 
{
		String type;

		Car()
		{
			this.type = "";
		}
		Car(String name, int kms, String type)
		{
			super(name, kms);
			this.type = type;
		}
		void display()
		{
			super.display();
			System.out.println("Type: " + type);
		}
}
```
Driver.java
```java
public class Driver 
{
		
		public static void main(String[] args) {
			Car car  = new Car("Toyota Rav4", 1234, "SUV");
			car.display();
		}
}
```

The keyword `super` refers to the superclass and can be used to invoke the superclass's methods and constructors. A constructor is used to construct an instance of a class. Unlike properties and methods, the constructors of a superclass are not inherited by a subclass. They can only be invoked from the constructors of the subclasses using the keyword `super`.

A subclass inherits methods from a superclass. Sometimes, it is necessary for the subclass to modify the implementation of a method defined in the superclass. This is referred to as method overriding. The `display` method in `Vehicle` class is overridden in `Car` class in the above example since we wanted to display the `type` of the car in addition to name and kms.

### Multi-Level Inheritance
Java also offer multi-level inheritance where grant child can access `public` and `protected` members of parent and grand parent.

Phone.java
```java
public class Phone 
{
		
		void makeCalls()
		{
			System.out.println("I can make calls");
		}
}
```

SmartPhone.java
```java
public class SmartPhone extends Phone 
{
		void browseInternet()
		{
			System.out.println("I can browse Internet");
		}

}
```
Android.java
```java
public class Android extends SmartPhone
{
		
		void iAmAndroid()
		{
			System.out.println("I am Android");
		}
}
```

Driver.java
```java
public class Driver 
{
		
		public static void main(String[] args) {
			Android android = new Android();
			android.makeCalls();
			android.browseInternet();
			android.iAmAndroid();
		}
}
```

### Protected Data and Methods
Often it is desirable to allow subclasses to access data fields or methods defined in the superclass, but not to allow nonsubclasses in different packages to access these data fields and methods. To accomplish this, you can use the `protected` keyword. This way you can access protected data fields or methods in a superclass from its subclasses.

Let's see an example of protected.

`Vehicle.java`
```java
package com.cmu;

public class Vehicle 
{
		protected String name;
		protected int kms;

		Vehicle()
		{
			this.name = "";
			this.kms = 0;
		}

		Vehicle(String name, int kms)
		{
			this.name = name;
			this.kms = kms;
		}

		void display()
		{
			System.out.println("Name: " + this.name + ", KMs: " + this.kms);
		}
}

```
`Car.java`
```java
package com.cmu;

public class Car extends Vehicle 
{
		String type;

		public Car()
		{
			this.type = "";
		}
		public Car(String name, int kms, String type)
		{
			super(name, kms);
			this.type = type;
		}
		public void display()
		{
			super.display();
			System.out.println("Type: " + type);
		}
}

```
`Driver.java`
```java
package com.cmu.test;

import com.cmu.Car;

public class Driver 
{
	public static void main(String[] args) 
	{
			Car car  = new Car("Toyota Rav4", 1234, "SUV");
			car.display();
			
			// this is not allowed because name is a protected member of Vehicle class.
			car.name = "Honda";
		
	}
}

```
One more important thing to note over here is that `Vehicle` and `Car` are in `com.cmu` package and `Driver` is in `com.cmu.test` package. The access to any protected member of `Vehicle` class would be given to `Driver` class if the `Driver` class is in the same package as `Vehicle` not matter it is the subclass or `Vehicle` or not.

## Polymorphism
Polymorphism means that a variable of a supertype can refer to a subtype object. Java supports Polymorphism, e.g., An object of parent class can hold the reference of a child class. The key concept of Polymorphism is that method calls are resolved at runtime, not compile time, for overridden methods.

Driver.java
```java
public class Driver 
{
		public static void main(String[] args) {

			Car car = new Car("Honda Civic", 45454, "Sedan");

			Vehicle v = car;
			v.display();
		}
	}
```

The above program assumes we have Vehicle and Car classes written with Car being a child of Vehicle.
	
Using Polymorphism at run time, Java knows which exact function to call based on the reference it has.

Shape.java
```java
public class Shape {
		double height, width;
		String name;

		Shape()
		{
			this.height = this.width = 0.0;
			this.name = "";
		}
		Shape(double height, double width, String name)
		{
			this.height = height;
			this.width = width;
			this.name = name;
		}

		double calculateArea()
		{
			return 0.0;
		}
}
```
Rectangle.java
```java
public class Rectangle extends Shape 
{
		Rectangle()
		{
			super();
		}

		Rectangle(double height, double width, String name)
		{
			super(height, width, name);
		}

		double calculateArea()
		{
			return height * width;
		}
}
```

Triangle.java
```java
public class Triangle extends Shape
{
		
		Triangle()
		{
			super();
		}

		Triangle(double height, double width, String name)
		{
			super(height, width, name);
		}

		double calculateArea()
		{
			return (height* width)/2;
		}
}
```

Driver.java
```java
public class Driver 
{
		public static void main(String[] args) {
			
			Shape shapes[] = new Shape[5];
			shapes[0] = new Triangle(10.0, 5.0, "Triangle 1");
			shapes[1] = new Triangle(15.0, 15.0, "Triangle 2");
			shapes[2] = new Rectangle(10.0, 10.0, "Rectangle 1");
			shapes[3] = new Rectangle(20.0, 1.0, "Rectangle 2");
			shapes[4] = new Shape(10.0, 1.0, "General Shape");

			for (int i = 0; i <5; i++)
			{
				System.out.println("Shape is " + shapes[i].name);
				System.out.println("Area is " + shapes[i].calculateArea());
			}
		}
}
```

## Abstract Classes and Methods
Java provides a mechanism of organizing the code in a way that forces the child classes to implement certain methods. This is the concept of Abstract classes and methods.

Shape.java
```java	
package com.cmu;

public abstract class Shape 
{
		double height, width;
		String name;
		
		Shape()
		{
			this.height = this.width = 0.0;
			this.name = "";
		}
		Shape(double height, double width, String name)
		{
			this.height = height;
			this.width = width;
			this.name = name;
		}
		abstract double calculateArea();

}
```	
Rectangle.java
```java	
package com.cmu;

public class Rectangle extends Shape 
{
		Rectangle()
		{
			super();
		}
		Rectangle(double height, double width, String name)
		{
			super(height, width, name);
		}
		double calculateArea()
		{
			return height * width;
		}

}
```	
Triangle.java
```java	
package com.cmu;

public class Triangle extends Shape
{
		
		Triangle()
		{
			super();
		}
		Triangle(double height, double width, String name)
		{
			super(height, width, name);
		}
		double calculateArea()
		{
			return (height * width) / 2;
		}

}
```
Driver.java
```java	
	package com.cmu;

	public class Driver 
	{
		public static void main(String[] args) 
		{
			Shape shapes[] = new Shape[4];
			shapes[0] = new Triangle(10.0, 5.0, "Triangle 1");
			shapes[1] = new Triangle(15.0, 10.0, "Triangle 2");
			shapes[2] = new Rectangle(10.0, 5.0, "Rectangle 1");
			shapes[3] = new Rectangle(15.0, 10.0, "Rectangle 2");
			
			for (Shape shape: shapes)
			{
				System.out.println("Shape is " + shape.name);
				System.out.println("Area is " + shape.calculateArea());
				
			}
		}

	}
```	
The above code forces Rectangle and Triangle classes to implement the calculateArea method, else compiler produces an error. Also, abstract class Shape cannot be instantiated.

## Interfaces in Java
An interface is a reference type in Java, similar to a class, that can contain only constants, method signatures, default methods, static methods, and nested types. It cannot contain instance fields or constructors.

Account.java
```java	
package com.cmu;

public interface Account 
{
		void deposit(double amount);
		void withdraw(double amount);
		double getBalance();
		String getAccountType();

}
```
Checking.java
```java	
package com.cmu;

public class Checking implements Account
{

		private double balance;
		private String accountNumber;
		
		Checking(String accountNumber)
		{
			this.balance = 0.0;
			this.accountNumber = accountNumber;
		}
		@Override
		public void deposit(double amount) 
		{
			balance += amount;
			System.out.println("Deposited $" + amount);
		}

		@Override
		public void withdraw(double amount) 
		{
			// TODO Auto-generated method stub
			balance -= amount;
			System.out.println("Withdrew $" + amount);
			
		}

		@Override
		public double getBalance() {
			return balance;
		}

		@Override
		public String getAccountType() {
			return "Checking";
		}
		

}
```
Saving.java
```java	
package com.cmu;

public class Saving implements Account 
{

		private double balance;
		private double interestRate;

		public Saving (double initialBalance, double interestRate) {
			this.balance = initialBalance;
			this.interestRate = interestRate;
		}

		@Override
		public void deposit (double amount) 
		{    
			balance += amount;
			System.out.println("Deposited $" + amount);
			
		}

		@Override
		public void withdraw(double amount) {
			
			balance -= amount;
			System.out.println("Withdrew $" + amount);
			
		}

		@Override
		public double getBalance() {
			return balance;
		}

		@Override
		public String getAccountType() {
			return "Savings";
		}
		
		public void applyInterest() {
			balance += balance * interestRate;
			System.out.println("Applied interest to savings account.");
		}

}
```	
Driver.java

 ```java
package com.cmu;

public class Driver 
{
		public static void main(String[] args) 
		{
			Account[] accounts = new Account[2];
			accounts[0] = new Checking("CHK1234");
			accounts[1] = new Saving(1000.0, 0.05);
			
			accounts[0].deposit(200.0);
			accounts[0].withdraw(50.0);
			System.out.println("Checking account balance: $" + accounts[0].getBalance());
			System.out.println("Account type: " + accounts[0].getAccountType());


			accounts[1].deposit(500.0);
			accounts[1].withdraw(100.0);
			((Saving) accounts[1]).applyInterest(); // Specific to SavingsAccount
			System.out.println("Savings account balance: $" + accounts[1].getBalance());
			System.out.println("Account type: " + accounts[1].getAccountType());
		}

}
```

## References:
- Introduction to Java Programming and Data Structures, 13th edition, by Y Daniel Liang,
	- Chapter 11 (Inheritance and Polymorphism)

