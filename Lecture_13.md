# Threads in Java
A Thread is the smallest unit of execution within a process. When we create more threads → we achieve multithreading. All threads share Heap and Static variables. Each thread has its Own stack and Own program counter. A single process can contain multiple threads running concurrently, sharing the same memory and resources. In Process Based multitasking, each process runs independently and isolates memory space (e.g.,
running multiple applications). In Thread Based multitasking, multiple threads within a single process share resources (e.g., managing multiple tasks like UI updates, file reading). For example, a video player can use one thread to play the video while another handles user input (play/pause).

## Single threaded Application
Single threaded environments work in sequence. Task 1 needs to complete before Task 2. Below is example where downloadFile needs to complete before print function can work:

`Driver.java`

```java
public class Driver 
{
		public static void downloadFile()
		{
			System.out.println("Starting file download...");
			
			try 
			{
				Thread.sleep(5000);
			} 
			catch (InterruptedException e) 
			{
				e.printStackTrace();
			}
			
			System.out.println("File download completed.");
		}
		public static void print()
		{
			for (int i=0; i<100; i++)
			{
				System.out.printf("%d ", i);
			}
		}
		public static void main(String[] args) 
		{
			downloadFile();
			print();
		}

}
```
## Multithreaded Application
The above program can be made more efficient if both functions can work in parallel. For this we have to create separate classes to run each function in their own threads
	
`Driver.java`

```java
class DownloadThread extends Thread
{
		public void run()
		{
			downloadFile();
		}
		public void downloadFile()
		{
			System.out.println("Starting file download...");
			
			try 
			{
				Thread.sleep(5000);
			} 
			catch (InterruptedException e) 
			{
				e.printStackTrace();
			}
			
			System.out.println("File download completed.");
		}
	}

	class PrintThread extends Thread
	{
		public void run()
		{
			print();
		}
		
		public void print()
		{
			for (int i=0; i<100; i++)
			{
				System.out.printf("%d ", i);
			}
			System.out.println();
		}
	}

	public class Driver 
	{
		public static void main(String[] args) 
		{
			DownloadThread download = new DownloadThread();
			download.start();
			
			PrintThread print = new PrintThread();
			print.start();
		}

}
```
In the above example DownloadThread is a thread. But conceptually, it is not a thread. It is a download task. A task is not the same thing as a thread. In software design, we prefer: One class → one responsibility. However, in the above example, that is not the case. Let's fix it using `Runnable`.

### Runnable Interface
Runnable is not a thread. It is description of work that can be executed. Let's take an analogy of a student in a class. Imagine you write instructions on a piece of paper: “Clean the classroom.” That paper is like a `Runnable`. Now imagine a student who actually performs the cleaning. That student is like a Thread. The paper describes the task. The student performs the task. 
```
Runnable = instructions
Thread = worker
```
Runnable cannot start running on its own. It is passive. It just waits to be given to something that can execute it (like a Thread or an Executor).

We can use `Runnable` Interface to separate thread from a task. 

`Driver.java`

```java
class DownloadThread implements Runnable
{
	public void run()
	{
		downloadFile();
	}
	public void downloadFile()
	{
		System.out.println("Starting file download...");
		
		try 
		{
			Thread.sleep(5000);
		} 
		catch (InterruptedException e) 
		{
			e.printStackTrace();
		}
		
		System.out.println("File download completed...");
	}
}
class PrintThread implements Runnable
{
	public void run()
	{
		print();
	}
	public void print()
	{
		for (int i=0; i<50; i++)
		{
			System.out.print(i + "\t");	
		}
		System.out.println();
		
	}
}
public class Driver 
{
	public static void main(String[] args) 
	{
		DownloadThread download = new DownloadThread();
		PrintThread print = new PrintThread();
		
		Thread t1 = new Thread(download);
		Thread t2 = new Thread(print);
		
		t1.start();
		t2.start();
	}
}

```

In the above code, although we have separate thread from the task but we are still writing a lot of code. The above code can be simplied as below using `Runnable`.

`Driver.java`
```java	
public class Driver 
{
		public static void downloadFile()
		{
			System.out.println("Starting file download...");
			
			try 
			{
				Thread.sleep(5000);
			} 
			catch (InterruptedException e) 
			{
				e.printStackTrace();
			}
			
			System.out.println("File download completed.");
		}
		public static void print()
		{
			for (int i=0; i<100; i++)
			{
				System.out.printf("%d ", i);
			}
			System.out.println();
		}
		public static void main(String[] args) 
		{
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

## Thread Lifecycle
A thread in Java can exist in any one of the following states at any given time. A thread lies only in one of the shown states at any instant:
- New: Thread object is created.
- Runnable: After calling the start() method.
- Running: Actively executing the task.
- Blocked: Waiting for resources (I/O, locks).
- Terminated: Execution completes or thread is killed.

Let's see it in action.

`Driver.java`
```java
package com.cmu.test;

public class Driver 
{
	public static void downloadFile()
	{
		System.out.println("Starting file download...");
		
		try 
		{
			Thread.sleep(2000);
		} 
		catch (InterruptedException e) 
		{
			e.printStackTrace();
		}
		
		System.out.println("File download completed...");
	}
	
	public static void main(String[] args) 
	{
	
		Thread t1 = new Thread(new Runnable(){
			@Override
			public void run() {
				downloadFile();
				
			}
		});
		
		System.out.println("State after creation: " + t1.getState());
		
		t1.start();
		System.out.println("State after start: " + t1.getState());
		
		try 
		{
			Thread.sleep(200);
		} 
		catch (InterruptedException e) 
		{
			e.printStackTrace();
		}
		
		System.out.println("State during sleep: " + t1.getState());
		
		try 
		{
			Thread.sleep(2000);
		} 
		catch (InterruptedException e) 
		{
			e.printStackTrace();
		}
		
		System.out.println("State after termination: " + t1.getState());

	}

}

```
## References

- https://www.geeksforgeeks.org/java/java-threads/
- https://www.geeksforgeeks.org/java/lifecycle-and-states-of-a-thread-in-java/