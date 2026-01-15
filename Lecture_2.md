# I/O, Arrays, Complexities, Sorting and Searching

## File Processing (I/O Operations)
Data stored in the program are temporary; they are lost when the program terminates. To permanently store the data created in a program, you need to save them in a file on a disk or other permanent storage device. The file can then be transported and read later by other programs. <br>
The `File` class is intended to provide an abstraction that deals with most of the machine-dependent complexities of files and path names in a machine-independent fashion. It contains the methods for obtaining file and directory properties, and for renaming and deleting files and directories.

## Reading Files
Java provides an efficient way of reading from files using built-in classes. The `FileReader` class in Java is used to read data from a file in the form of characters. It is a character-oriented stream that makes it ideal for reading text files. If you want to read binary data (like images or videos), use `FileInputStream` instead.
```java
	import java.io.BufferedReader;
	import java.io.FileNotFoundException;
	import java.io.FileReader;
	import java.io.IOException;

	public class Driver {

		public static void main(String[] args) 
		{
			FileReader fileReader;
			try 
			{
				fileReader = new FileReader("sample.txt");
				BufferedReader reader = new BufferedReader(fileReader);
				String line;
				
				while ((line = reader.readLine()) != null)
				{
					System.out.println(line);
				}
			} catch (IOException e) {
				System.out.println("An error occurred");
				e.printStackTrace();
			}
			
		}
	}
```
The `BufferedReader` class in Java helps read text efficiently from files or user input. It stores data in a buffer, making reading faster and smoother instead of reading one character at a time. The key advantages of `BufferedReader` are:
- Faster Reading: Reads large chunks of data at once, reducing the number of read operations.
- Easy to Use: Can read text line by line using the readLine() method.

## Writing Files
Just like reading, Java provides built-in classes for writing to the files as well. The `FileWriter` class in Java is used to write character data to files. It extends `OutputStreamWriter` and handles characters directly, making it ideal for writing text files with either the default or a specified encoding.
```java
	import java.io.BufferedReader;
	import java.io.BufferedWriter;
	import java.io.FileNotFoundException;
	import java.io.FileReader;
	import java.io.FileWriter;
	import java.io.IOException;

	public class Driver {

		public static void main(String[] args) 
		{
			try 
			{
				FileWriter fileWriter = new FileWriter("sample.txt");
				BufferedWriter writer = new BufferedWriter(fileWriter);
				writer.write("Postal Code: 48859");
				System.out.println("Data written to the file");
				writer.close();
				fileWriter.close();
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
			
		}
	}
```
In order to append to the existing file, a second parameter as `true` can be provided to `FileWriter` constructor as below:
```java
fileWriter = new FileWriter("sample.txt", true);
```
`BufferedWriter` class in Java is used to write text efficiently to character-based output streams. It stores characters in a buffer before writing them to the destination, thereby reducing the number of I/O operations and improving performance. Below are some of the advantages of using `BufferedWriter`:

- Faster Writing: Writes large chunks of data at once instead of writing character by character.
- Easy to Use: Provides methods like write() and newLine() to simplify text output.

## Array
Often you will have to store a large number of values during the execution of a program. Suppose, for instance, that you need to read 100 numbers, compute their average, and find out how many numbers are above the average. Your program first reads the numbers and computes their average, then compares each number with the average to determine whether it is above the average. In order to accomplish this task, the numbers must all be stored in variables. You have to declare 100 variables and repeatedly write almost identical code 100 times. Writing a program this way would be impractical. So, how do you solve this problem? <br>
 Java and most other high-level languages provide a data structure, the `array`, which stores a fixed-size sequential collection of elements of the same type. 

```java
	public class Driver 
	{
		public static void main(String[] args) 
		{
			int[] numbers = new int[5];
			numbers[0] = 2;
			numbers[1] = 4;
			numbers[2] = 6;
			numbers[3] = 8;
			numbers[4] = 10;
			
			for (int i=0; i<numbers.length; i++)
			{
				System.out.println(numbers[i]);
			}		
		}
	}
```
Once an array is created, its size is fixed. An array reference variable is used to access the elements in an array using an index. All elements in the array will have the same data type. As shown in the code above, an integer array was created so all the elements of the array are integers.

Arrays can be used as data members of a class as well:

`Numbers.java`
 
```java
	import java.util.Scanner;

	public class Numbers 
	{
		private int[] numbers;
		
		Numbers(int size)
		{
			numbers = new int[size];
		}
		
		public void insert_elements()
		{
			Scanner scanner = new Scanner(System.in);
			for (int i=0; i<numbers.length; i++)
			{
				System.out.printf("Enter elements at index %d: ", i);
				numbers[i] = scanner.nextInt();
			}
			
		}
		
		public void print_elements()
		{
			for (int i=0; i<numbers.length; i++)
			{
				System.out.println(numbers[i]);
			}
		}
		
		public boolean search_element(int element)
		{
			for (int number: numbers)
			{
				if (number == element)
				{
					return true;
				}
			}
			return false;
		}

	}
```
`Driver.java`
```java
	public class Driver {

		public static void main(String[] args) 
		{
			Numbers num = new Numbers(5);
			num.insert_elements();
			num.print_elements();
			System.out.println(num.search_element(5)? "Found" : "Not Found");
		}
	}
```
In Java, the `Scanner` class is present in the java.util package is used to obtain input for primitive types like int, double, etc., and strings. We can use this class to read input from a user or a file. <br>
The ternary operator is a compact alternative to the if-else statement. It evaluates a condition and returns one of two values depending on whether the condition is true or false. It is commonly used for:
- Conditional value assignment
- Simple decision-making logic
- Replacing short if-else blocks

## Complexities using Big-Oh
Big O describes an upper bound on the time. Big O just describes the rate of increase. For this reason, we drop the constants in runtime. An algorithm that one might have described as `0(2N)` is actually `O(N)`. Big O helps in answering “How does the runtime of a program grows with increasing input?” <br>
Linear Complexity is when runtime increases with the size. 
```
𝑇 = 𝑎𝑛 + 𝑏
```
• We only consider the fastest growing term which is `n` in this case. • Drop any constants with the fastest growing term. Below are some of the examples:
```
7n - 2
```
Big O of the above equation is `O(n)`.
```
3n3 + 20n2 + 5
```
Big O of the above equation is `O(n³)`.
```
3 log n + 5
```
Big O of the above equation is `O(log n)`.

Below are some more examples of Big O from coding world:
```java
public class Driver 
{
	public static int constant_function(int arr[])
	{
		int number = 0; // O(1)
		return number; // O(1)
		// total time = O(1) + O(1) = O(2) which is constant
	}
	public static void main(String[] args) 
	{
		int arr[] = new int[5];
		constant_function(arr);
	}
}

```
The above is an example of Linear Time `O(1)` where the processing time stays constant no matter the size of the array.
```java
public static int linear_function(int arr[])
{
	int sum = 0; // O(1)
	for (int i=0; i<arr.length; i++) // this runs n times
	{
		sum+=i; // O(1)
	}
	return sum; // O(1)
	// total time = O(1) + n x O(1) + O(1) = O(n)
}

```
The above is an example of Linear Time `O(n)` where the processing time increases with the increase in the size of the array. The linear search algorithm compares the key with the elements in the array sequentially until the key is found or the array is exhausted. If the key is not in the array, it requires `n` comparisons for an array of size `n`. If the key is in the array, it requires `n/2` comparisons on average. The algorithm’s execution time is proportional to the size of the array. If you double the size of the array, you will expect the number of comparisons to double. The algorithm grows at a linear rate. The growth rate has an order of magnitude of `n`.

## Sorting
Sorting is a classic subject in computer science. There are three reasons to study sorting algorithms.  First, sorting algorithms illustrate many creative approaches to problem solving, and these approaches can be applied to solve other problems. Second, sorting algorithms are good for practicing fundamental programming techniques using selection statements, loops, methods, and arrays. Third, sorting algorithms are excellent examples to demonstrate algorithm performance. There are several sorting techniques:
- Simple Sorting Techniques
	- Bubble Sort
	- Insertion Sort
	- Selection Sort
- Advanced Sorting Techniques
	- Merge Sort
	- Quick Sort

## Selection Sort
Selection Sort is a comparison-based sorting algorithm. It sorts by repeatedly selecting the smallest (or largest) element from the unsorted portion and swapping it with the first unsorted element.
- Find the smallest element and swap it with the first element. This way we get the smallest element at its correct position.
- Then find the smallest among remaining elements (or second smallest) and swap it with the second element.
- We keep doing this until we get all elements moved to correct position.
```java
public void selection_sort()
{
		int size = numbers.length;
		
		for (int i=0; i<size; i++)
		{
			int smallest = i;
			
			for (int j = i + 1; j<size; j++)
			{
				if (numbers[smallest] > numbers[j])
				{
					smallest = j;
				}
			}
			
			int temp = numbers[smallest];
			numbers[smallest] = numbers[i];
			numbers[i] = temp;
		}
}
```
## Binary Search
Binary Search is a searching algorithm that operates on a sorted or monotonic search space, repeatedly dividing it into halves to find a target value or optimal answer in logarithmic time `O(log N)`. To apply Binary Search algorithm:
- The data structure must be sorted.
- Access to any element of the data structure should take constant time.

Below is the step-by-step algorithm for Binary Search:
- Divide the search space into two halves by finding the middle index "mid". 
- Compare the middle element of the search space with the key. 
- If the key is found at middle element, the process is terminated.
- If the key is not found at middle element, choose which half will be used as the next search space.
	- If the key is smaller than the middle element, then the left side is used for next search.
	- If the key is larger than the middle element, then the right side is used for next search.
- This process is continued until the key is found or the total search space is exhausted.

```java
public boolean binary_search(int element)
{
		int head = 0;
		int tail = numbers.length - 1;
		
		
		while (head <= tail)
		{
			int mid = (head + tail) / 2;
			if (element == numbers[mid])
			{
				return true;
			}
			else if (element < numbers[mid])
			{
				tail = mid - 1;
			}
			else
			{
				head = mid + 1;
			}
		}
		return false;
		
}
```
