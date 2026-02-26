# Threads in Java (cont.)
As discussed in the last lecture, a Thread is the smallest unit of execution within a process. When we create more threads → we achieve multithreading. In Thread Based multitasking, multiple threads within a single process share resources (e.g., managing multiple tasks like UI updates, file reading). 

## Threads Communication
Threads can communicate with each other using variables. Inter-thread communication is important when you develop an application where two or more threads exchange some information. 

Let's make download thread communicate with print thread.

`Driver.java`
```java
public class Driver 
{
		private static int progress = 0;

		public static void downloadFile() {
			System.out.println("Starting file download...");

			for (int i = 0; i <= 100; i++) {
				try 
				{
					Thread.sleep(50);
					progress = i;
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
			}
			
			System.out.println("\nFile download completed.");
		}

		public static void print() 
		{
			while (progress < 100) 
			{
				System.out.print("Progress: " + progress + "% \n");
				try 
				{
					Thread.sleep(100);
				} 
				catch (InterruptedException e) 
				{
					e.printStackTrace();
				}
			}
		   
		}

		public static void main(String[] args) {
			Thread downloadThread = new Thread(new Runnable() {
				@Override
				public void run() {
					downloadFile();
				}
				
			});
			
			Thread printThread = new Thread(new Runnable() {
				@Override
				public void run() {
					print();
				}
				
			});
			
			downloadThread.start();
			printThread.start();
		}
}
```
### Lambda Functions for Threads	
We can simplify the above main method by using lambda function as well. This is easier to write and Java -8 and above support this way of writing threads but it is less readable, however much cleaner:
```java
public static void main(String[] args) 
  {
    	Thread downloadThread = new Thread(()->{
    		downloadFile();
    	});
		
		Thread printThread = new Thread(()->{
			print();
		});
        
        downloadThread.start();
        printThread.start();
  }
```
Runnable has only one method (`run()`). Because it has only one method, Java calls it a  functional interface. When an interface has only one method, Java allows us to use a lambda expression instead of writing the full anonymous class.

### Method Referencing for Threads
We can even simplify the above main method by using method referencing. This is even easier to write and provide much cleaner code. This can be used if no other functionality in the run method is required but to call the methods.
```java
public static void main(String[] args) 
  {
    	Thread downloadThread = new Thread(Driver::downloadFile);
		
		Thread printThread = new Thread(Driver::print);
        
        downloadThread.start();
        printThread.start();
  }
```
You might have noticed one problem in the above code, that progress percentages are printed several times for 1% progress. This is because variable `progress` is not synced properly between the threads. In order to sync it properly, we can use `volatile`. Please note the following:
- Without volatile:
    - Each thread may cache progress
    - One thread may not see updates from the other

- With volatile:
    - Every read comes from main memory
    - Updates become visible immediately

Below is the updated code that shows synced progress (one progress percent per sec).

`Driver.java`
```java
public class Driver 
{
    private static volatile int progress = 0;

    public static void downloadFile() {
        System.out.println("Starting file download...");

        for (int i = 1; i <= 100; i++) {
            try {
                Thread.sleep(1000);  // 1 second
                progress = i;
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        System.out.println("File download completed...");
    }

    public static void print() {
        while (progress < 100) {
            System.out.println("Progress: " + progress + "%");

            try {
                Thread.sleep(1000);  // print once per second
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        System.out.println("Progress: 100%");
    }

    public static void main(String[] args) {

        Thread t1 = new Thread(Driver::downloadFile);
        Thread t2 = new Thread(Driver::print);

        t1.start();
        t2.start();
    }
}

```

## Joining Threads
It is sometimes desirable to make sure that main thread does not exit before all the threads have completed their execution. Joining threads can be beneficial in such cases. When we invoke the `join()` method on a thread, the calling thread goes into a waiting state. It remains in a waiting state until the referenced thread terminates.

Driver.java
```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class Driver 
{

	public static void readFile()
	{
		BufferedReader reader = null;
		
		try 
		{
			reader = new BufferedReader(new FileReader("input.txt"));
			String line;
			
			while ((line = reader.readLine())!= null)
			{
				System.out.println("Read: " + line);
				Thread.sleep(500);
			}
		} 
		catch (FileNotFoundException e) 
		{
			e.printStackTrace();
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		finally
		{
			try {
				reader.close();
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	}
	
	public static void writeFile()
	{
		BufferedWriter writer = null;
		
		try 
		{
			writer = new BufferedWriter(new FileWriter("input.txt", true));
			for (int i=0; i<5;i++)
			{
				String data = "Line " + i;
				writer.write(data);
				writer.newLine();
				System.out.println("Written: " + data);
				Thread.sleep(1000);
			}
		} 
		catch (IOException e) 
		{
			e.printStackTrace();
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		finally
		{
			try {
				writer.close();
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	}
    public static void main(String[] args) 
    {
    	Thread t1 = new Thread(Driver::readFile);
    	Thread t2 = new Thread(Driver::writeFile);
    	
    	t1.start();
    	t2.start();
    	
    	
    	System.out.println("Rest of the code...");

    }
}
 ```

 You might have noticed that `Rest of the code...` is printed even before threads started. This might not be desirable if you want to order the execution of threads. `join()` can help.

`Driver.java`
 ```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class Driver 
{

	public static void readFile()
	{
		BufferedReader reader = null;
		
		try 
		{
			reader = new BufferedReader(new FileReader("input.txt"));
			String line;
			
			while ((line = reader.readLine())!= null)
			{
				System.out.println("Read: " + line);
				Thread.sleep(500);
			}
		} 
		catch (FileNotFoundException e) 
		{
			e.printStackTrace();
		} catch (InterruptedException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
		finally
		{
			try {
				reader.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}
	
	public static void writeFile()
	{
		BufferedWriter writer = null;
		
		try 
		{
			writer = new BufferedWriter(new FileWriter("input.txt", true));
			for (int i=0; i<5;i++)
			{
				String data = "Line " + i;
				writer.write(data);
				writer.newLine();
				System.out.println("Written: " + data);
				Thread.sleep(1000);
			}
		} 
		catch (IOException e) 
		{
			e.printStackTrace();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		finally
		{
			try {
				writer.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}
    public static void main(String[] args) throws InterruptedException 
    {
    	Thread t1 = new Thread(Driver::readFile);
    	Thread t2 = new Thread(Driver::writeFile);
    	
    	t1.start();
    	t2.start();
    	
    	t1.join();
    	t2.join();
    	
    	
    	System.out.println("Rest of the code...");

    }
}

 ```

 The above program will mostly run fine even without `join()` except the order of execution, because JVM waits for all the non-daemon (background) threads to complete their execution before `main` thread is terminated. However, there might be situations where you want to use background threads, e.g., logging, monitoring and saving etc.

 ```java
package com.cmu.test;

import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

public class Driver 
{

	public static void readFile()
	{
		BufferedReader reader = null;
		
		try 
		{
			reader = new BufferedReader(new FileReader("input.txt"));
			String line;
			
			while ((line = reader.readLine())!= null)
			{
				System.out.println("Read: " + line);
				Thread.sleep(500);
			}
		} 
		catch (FileNotFoundException e) 
		{
			e.printStackTrace();
		} catch (InterruptedException e) {
			e.printStackTrace();
		} catch (IOException e) {
			e.printStackTrace();
		}
		finally
		{
			try {
				reader.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}
	
	public static void writeFile()
	{
		BufferedWriter writer = null;
		
		try 
		{
			writer = new BufferedWriter(new FileWriter("input.txt", true));
			for (int i=0; i<5;i++)
			{
				String data = "Line " + i;
				writer.write(data);
				writer.newLine();
				System.out.println("Written: " + data);
				Thread.sleep(1000);
			}
		} 
		catch (IOException e) 
		{
			e.printStackTrace();
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		finally
		{
			try {
				writer.close();
			} catch (IOException e) {
				e.printStackTrace();
			}
		}
	}
    public static void main(String[] args) throws InterruptedException 
    {
    	Thread t1 = new Thread(Driver::readFile);
    	Thread t2 = new Thread(Driver::writeFile);
    	
    	t1.setDaemon(true);
    	t2.setDaemon(true);
    	
    	t1.start();
    	t2.start();    	
    	
    	System.out.println("Rest of the code...");

    }
}

 ```

 The above program will terminate even before the threads start because main thread does not wait for the background threads to complete. In order to wait for background threads to complete before main thread can terminate, `join()` can be used.

 ```java
public static void main(String[] args) throws InterruptedException 
{
    	Thread t1 = new Thread(Driver::readFile);
    	Thread t2 = new Thread(Driver::writeFile);
    	
    	t1.setDaemon(true);
    	t2.setDaemon(true);
    	
    	t1.start();
    	t2.start();
    	
    	t1.join();
    	t2.join();
    	
    	
    	System.out.println("Rest of the code...");

}

 ```

## References
- https://www.geeksforgeeks.org/java/joining-threads-in-java/
- https://www.tutorialspoint.com/java/java_thread_communication.htm