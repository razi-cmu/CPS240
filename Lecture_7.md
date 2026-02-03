# Exception Handling

## Exceptions
Exceptions are runtime errors. Exception handling enables a program to deal with runtime errors and continue its normal execution. Runtime errors occur while a program is running if the JVM detects an operation that is impossible to carry out. For example, if you access an array using an index that is out of bounds, you will get a runtime error with an `ArrayIndexOutOBoundsException`. If you enter a  value when your program expects an integer, you will get a runtime error with an `InputMismatchException`. <br>
In Java, runtime errors are thrown as exceptions. An exception is an object that represents an error or a condition that prevents execution from proceeding normally. If the exception is not handled, the program will terminate abnormally.

Let's see an example of an execption.

`Driver.java`
```java	
import java.util.Scanner;

public class Driver 
{
		public static void main(String[] args) 
		{
			Scanner scanner = new Scanner(System.in);
			
			System.out.print("Enter number 1: ");
			int number1 = scanner.nextInt();
			System.out.print("Enter number 2: ");
			int number2 = scanner.nextInt();
			
			int result = number1/number2;
			
			System.out.println("Result: "+ result);
			
		}

}
```
The above program would run just fine except when we input number2 as 0. Since, we cannot divide by 0, the program crashes.
	
One of the ways of handling this issue is to use conditional statements like if else. In order to make it more readable, let's create a separate function:
	
`Driver.java`
```java	
import java.util.Scanner;

public class Driver 
{
		
		public static int quotient(int number1, int number2)
		{
			if (number2 == 0)
			{
				System.out.println("Cannot divide by 0.");
				System.exit(1);
			}
			int result = number1/number2;
			
			return result;
		}
		public static void main(String[] args) 
		{
			Scanner scanner = new Scanner(System.in);
			
			System.out.print("Enter number 1: ");
			int number1 = scanner.nextInt();
			System.out.print("Enter number 2: ");
			int number2 = scanner.nextInt();
			
			int result = quotient(number1, number2);
			
			System.out.println("Result: "+ result);
			
		}

}
```
The problem with the above approach is that the method 'quotient' would terminate the program, which is not ideal. So if there is any code after the 'quotient' method calls return to main method, that code would not execute:
	
`Driver.java`
```java	
import java.util.Scanner;

public class Driver 
{
		
		public static int quotient(int number1, int number2)
		{
			if (number2 == 0)
			{
				System.out.println("Cannot divide by 0.");
				System.exit(1);
			}
			int result = number1/number2;
			
			return result;
		}
		public static void main(String[] args) 
		{
			Scanner scanner = new Scanner(System.in);
			
			System.out.print("Enter number 1: ");
			int number1 = scanner.nextInt();
			System.out.print("Enter number 2: ");
			int number2 = scanner.nextInt();
			
			int result = quotient(number1, number2);
			
			// This code would not run
			System.out.println("Result: "+ result);
			
			// This code would not run
			System.out.println("Rest of the code");
			
		}

}
```
## Throwing an Exception
Try Catch can be used to throw and then catch an exception that might occur in a vulernable code:

`Driver.java`
```java	
package com.cmu;

import java.util.Scanner;

public class Driver 
{
		
		public static int quotient(int number1, int number2)
		{
			if (number2 == 0)
			{
				throw new ArithmeticException("Cannot divide by zero");
			}
			int result = number1/number2;
			
			return result;
		}
		public static void main(String[] args) 
		{
			Scanner scanner = new Scanner(System.in);
			
			System.out.print("Enter number 1: ");
			int number1 = scanner.nextInt();
			System.out.print("Enter number 2: ");
			int number2 = scanner.nextInt();
			
			try
			{
				int result = quotient(number1, number2);
				
				System.out.println("Result: "+ result);
			}
			catch(ArithmeticException ex)
			{
				System.out.println(ex.getMessage());
			}

			System.out.println("Rest of the code");
			
		}

}
```
The value thrown, in this case `new ArithmeticException("Cannot divide by zero")`, is called an exception. The execution of a `throw` statement is called throwing an exception. The exception is an object created from an exception class. In this case, the exception class is `java.lang.ArithmeticException`. The constructor `ArithmeticException(str)` is invoked to construct an exception object, where `str` is a message that describes the exception. <br>
When an exception is thrown, the normal execution flow is interrupted. As the name suggests, to “throw an exception” is to pass the exception from one place to another. The statement for invoking the method is contained in a  block. The `try` block contains the code that is executed in normal circumstances. The exception is caught by the `catch` block. The code in the `catch` block is executed to handle the exception. Afterward, the statement `System.out.println("Rest of the code");` after the `catch` block is executed.

## Exception Types
Exceptions are objects, and objects are defined using classes. The root class for exceptions is `java.lang.Throwable`. The `Throwable` class is the root of exception classes. All Java exception classes inherit directly or indirectly from `Throwable`. You can create your own exception classes by extending `Exception` or a subclass of `Exception`.

The exception classes can be classified into three major types: system errors, exceptions, and runtime exceptions.
- System errors are thrown by the JVM and are represented in the `Error` class. The `Error` class describes internal fatal system errors, though such errors rarely occur. If one does, there is little you can do beyond notifying the user and trying to terminate the program gracefully.
- Exceptions are represented in the `Exception` class, which describes errors caused by your program and by external circumstances. These errors are non-fatal and can be caught and handled by your program.
- Runtime exceptions are represented in the `RuntimeException` class, which describes programming errors, such as bad casting, accessing an out-of-bounds array index, and numeric errors. Runtime exceptions normally indicate programming errors.

Let's try some other types of exceptions as well. The below program will work fine as long as we are entering the integers. However, the program crashes when a double is entered:

`Driver.java`
```java	
import java.util.Scanner;

public class Driver 
{
		
		public static void main(String[] args) 
		{
			Scanner scanner = new Scanner(System.in);
			
			System.out.print("Enter a number: ");
			int number = scanner.nextInt();
			
			System.out.println("Number entered was " + number);
			
		}

}
```
We can fix the above program using try catch and putting a loop so that program keeps asking the user to enter number till user inputs the correct number

`Driver.java`
```java	
import java.util.InputMismatchException;
import java.util.Scanner;

public class Driver 
{
		
		public static void main(String[] args) 
		{
			Scanner scanner = new Scanner(System.in);
			boolean input = true;
			
			do
			{
				try
				{
					System.out.print("Enter a number: ");
					int number = scanner.nextInt();
					
					System.out.println("Number entered was " + number);
					
					input = false;
				}
				catch(InputMismatchException ex)
				{
					System.out.println("Incorrect input. Please enter integers only: ");
					scanner.nextLine();
				}
				
			} while (input);
			
		}

}
```
## The `throws` Keyword
Methods can throw exceptions without being explicitly declared using a keyword 'throws':

Driver.java
```java	
import java.util.InputMismatchException;
import java.util.Scanner;

public class Driver 
{
		public static void display(int[] numbers) throws ArrayIndexOutOfBoundsException
		{
			for (int i=0; i<numbers.length; i++)
			{
				System.out.print(numbers[i] + "\t");
			}
			
			System.out.println(numbers[5]);
		}
		
		public static void main(String[] args) 
		{
			int[] numbers = new int[] {1, 2, 3, 4, 5};
			
			try
			{
				display(numbers);
			}
			catch(ArrayIndexOutOfBoundsException ex)
			{
				System.out.println("\nArray Index out of bound");
			}

		}

}
```
Exception can also be caught and handled in the same method where it is thrown.
## Multiple catch blocks
A try block can have multiple catch blocks.

`Driver.java`
```java
import java.util.InputMismatchException;
import java.util.Scanner;

public class Driver 
{
		
		public static void main(String[] args) 
		{
			
			String myException = "divide";
			
			try
			{
				if (myException.equals("Null"))
				{
					throw new NullPointerException();
				}
				else if (myException.equals("array"))
				{
					throw new ArrayIndexOutOfBoundsException();
				}
				else if (myException.equals("divide"))
				{
					throw new ArithmeticException();
				}
			}
			catch(NullPointerException ex)
			{
				System.out.println("Null Pointer Exception");
			}
			catch(ArrayIndexOutOfBoundsException ex)
			{
				System.out.println("Array Index out of bound");
			}
			catch(Exception ex)
			{
				System.out.println("A Generic Exception caught");
			}
			
		}

}
```
As shown above, a parent exception can catch any child exception. `Exception` is the parent of `ArithmeticException` and was able to catch this exception although no explicit catch block was available for `ArithmeticException`.
## Order of catch blocks
The order to placement of these catch blocks is extremely important. If a catch block is placed in a way that it cannot never execute, we get an error:

`Driver.java`
```java
import java.util.InputMismatchException;
import java.util.Scanner;

public class Driver 
{
		
		public static void main(String[] args) 
		{
			
			String myException = "divide";
			
			try
			{
				if (myException.equals("Null"))
				{
					throw new NullPointerException();
				}
				else if (myException.equals("array"))
				{
					throw new ArrayIndexOutOfBoundsException();
				}
				else if (myException.equals("divide"))
				{
					throw new ArithmeticException();
				}
			}
			catch(Exception ex)
			{
				System.out.println("A Generic Exception caught");
			}
			catch(NullPointerException ex)
			{
				System.out.println("Null Pointer Exception");
			}
			catch(ArrayIndexOutOfBoundsException ex)
			{
				System.out.println("Array Index out of bound");
			}
		}
}
```
The above code generates an error as `Exception` is the parent class and would end up catching all the exceptions which makes `NullPointerException` and `ArrayIndexOutOfBoundsException` unreachable.

 ## The `finally` Keyword
The finally keyword in Java is useful for ensuring that specific code runs regardless of whether an exception is thrown or not. Occasionally, you may want some code to be executed regardless of whether an exception occurs or is caught. Java has a `finally` clause that can be used to accomplish this objective.

The code in the `finally` block is executed under all circumstances, regardless of whether an exception occurs in the `try` block or is caught. Consider three possible cases:  
- If no exception arises in the `try` block, the statments in the `finally` block are executed and the next statement after the `try` statement is executed.
- If a statement causes an exception in the `try` block that is caught in a `catch` block, the rest of the statements in the `try` block are skipped, the `catch` block is executed, and the  `finally` clause is executed. The next statement after the `try` statement is executed.
- If one of the statements causes an exception that is not caught in any `catch` block, the other statements in the `try` block are skipped, the `finally` clause is executed, and the exception is passed to the caller of this method.

The code in the `finally` clause is often for closing files and for cleaning up resources. The `finally` block executes even if there is a `return` statement prior to reaching the `finally` block.

Let's see a realistic example of `finally`.

`Driver.java`
```java	
package com.cmu;

import java.io.BufferedReader;
import java.io.FileInputStream;
import java.io.FileReader;
import java.io.IOException;
import java.util.InputMismatchException;
import java.util.Scanner;

public class Driver 
{
		public static void readFile(String fileName)
		{
			BufferedReader reader = null;
			
			try
			{
				reader = new BufferedReader(new FileReader(fileName));
				
				String line;
				
				while ((line = reader.readLine()) != null)
				{
					System.out.println(line);
				}	
			}
			catch(IOException ex)
			{
				System.out.println(ex.getMessage());
			}
			finally
			{
				try 
				{
					if (reader != null)
					{
						reader.close();
					}
					
				} 
				catch (IOException e) 
				{
					e.printStackTrace();
				}
			}
		}
		
		public static void main(String[] args) 
		{
			readFile("sample1.txt");
		}

}
```
The above program makes sure that reader is closed no matter an exception occurs or not. This is a real life example of using `finally` as it is highly recommended to close readers after usage.

## References
- Introduction to Java Programming and Data Structures, 13th edition, by Y Daniel Liang,
	- Chapter 12 (Inheritance and Polymorphism)
- https://www.w3schools.com/java/java_try_catch.asp
