# Java Generic
Generics let you parameterize types. With this capability, you can define a class or a method with generic types that the compiler can replace with concrete types. For example, Java defines a generic `ArrayList` class for storing the elements of a generic type. From this generic class, you can create an `ArrayList` object for holding strings, and an `ArrayList` object for holding numbers. Here, strings and numbers are concrete types that replace the generic type.

The key benefit of generics is to enable errors to be detected at compile time rather than at runtime. A generic class or method permits you to specify allowable types of objects that the class or method can work with. If you attempt to use an incompatible object, the compiler will detect that error.

## Java Generic
Generics enable you to write code that works with any type while ensuring type safety.

`Driver.java`
```java
package com.cmu;

class X
{
		private int x;

		X(int x)
		{
			this.x = x;
		}
		int getX()
		{
			return this.x;
		}
}
class Y
{
		private int y;

		Y(int y)
		{
			this.y = y;
		}
		int getY()
		{
			return this.y;
		}
}
public class Driver 
{
		public static void main(String[] args) 
		{
			X obj1 = new X(5);
			Y obj2 = new Y(6);
			
			int result = obj1.getX() + obj2.getY();
			
			System.out.println("Result: " + result);	
		}
}
```

The above program would work fine for integers. However, if we try to calculate the sum using double as below, we get an error:
```java
public class Driver 
{
		public static void main(String[] args) 
		{
			X obj1 = new X(5.5);
			Y obj2 = new Y(6.2);
			
			int result = obj1.getX() + obj2.getY();
			
			System.out.println("Result: " + result);	
		}
}
```

In a traditional program, we have to create separate X and Y classes to hold double or at least a separate data member which is double.

This program can be solved using Java Generic.

```java
package com.cmu;
class X<T>
{
		private T x;
		
		X(T x)
		{
			this.x = x;
		}
		T getX()
		{
			return this.x;
		}
}
class Y<T>
{
		private T y;
		
		Y(T y)
		{
			this.y = y;
		}
		T getY()
		{
			return this.y;
		}
}
public class Driver 
{
		public static void main(String[] args) 
		{
			X<Integer> obj1 = new X<>(5);
			Y<Integer> obj2 = new Y<>(6);
			
			int result = obj1.getX() + obj2.getY();
			
			System.out.println("Result: " + result);			
		}
}
```

If we want to change the data type to double, we simply change the type to Double as below:

```java
package com.cmu;

class X<T>
{
		private T x;
		
		X(T x)
		{
			this.x = x;
		}
		T getX()
		{
			return this.x;
		}
}
class Y<T>
{
		private T y;
		
		Y(T y)
		{
			this.y = y;
		}
		T getY()
		{
			return this.y;
		}
}
public class Driver 
{
		public static void main(String[] args) 
		{
			X<Double> obj1 = new X<>(5.6);
			Y<Double> obj2 = new Y<>(6.2);
			
			double result = obj1.getX() + obj2.getY();
			
			System.out.println("Result: " + result);			
		}
}
```

The above code would even work for String:

```java
public class Driver 
{
		public static void main(String[] args) 
		{
			X<String> obj1 = new X<>("Hello");
			Y<String> obj2 = new Y<>("World");
			
			String result = obj1.getX() + obj2.getY();
			
			System.out.println("Result: " + result);	
		}
}

```
## Creating an ArrayList

Let's create an ArrayList from scratch. To start with, let's create an ArrayList of int:

```java
package com.cmu;

class MyArrayList
{
		int[] myArray;
		int size;
		
		MyArrayList(int capacity)
		{
			this.size = 0;
			this.myArray = new int[capacity];
		}
		public void add(int element)
		{
			if (this.size < myArray.length)
			{
				myArray[size++] = element;
			}
			else
			{
				System.out.println("Cannot add element. Array is full");
			}
		}
		public int get(int index)
		{
			if (index >= 0 && index < size)
			{
				return myArray[index];
			}
			else
			{
				throw new IndexOutOfBoundsException("Index " + index +  " is out of bound");
			}
		}
		public int getSize()
		{
			return this.size;
		}
		
		public void print()
		{
			for (int i=0; i<this.size; i++)
			{
				System.out.print(myArray[i] + " ");
			}
		}
}
public class Driver 
{
		public static void main(String[] args) 
		{
			MyArrayList list = new MyArrayList(5);
			list.add(2);
			list.add(4);
			list.add(6);
			list.add(8);
			list.add(10);
			
			list.print();
			System.out.println("\nElement at index 2: " + list.get(2));
			System.out.println("Size of the list: " + list.getSize());	
		}
}
```
Trying to add a double value to this ArrayList would result in errors. Below is the code that would produce an error:

```java
public static void main(String[] args) 
{
		MyArrayList list = new MyArrayList(5);
		list.add(2.1);
		list.add(4.1);
		list.add(6.1);
		list.add(8.1);
		list.add(10.1);
		
		list.print();
		System.out.println("\nElement at index 2: " + list.get(2));
		System.out.println("Size of the list: " + list.getSize());
}
```

The above issue can be resolved using Java Generic.

```java
package com.cmu;

class MyArrayList<T>
{
		T[] myArray;
		int size;
		
		MyArrayList(int capacity)
		{
			this.size = 0;
			this.myArray = (T[])new Object[capacity]; // This is to ensure type safety
		}
		public void add(T element)
		{
			if (this.size < myArray.length)
			{
				myArray[size++] = element;
			}
			else
			{
				System.out.println("Cannot add element. Array is full");
			}
		}
		public T get(int index)
		{
			if (index >= 0 && index < size)
			{
				return myArray[index];
			}
			else
			{
				throw new IndexOutOfBoundsException("Index " + index +  " is out of bound");
			}
		}
		public int getSize()
		{
			return this.size;
		}
		
		public void print()
		{
			for (int i=0; i<this.size; i++)
			{
				System.out.print(myArray[i] + " ");
			}
		}
}
public class Driver 
{
		public static void main(String[] args) 
		{
			MyArrayList<Double> list = new MyArrayList<>(5);
			list.add(2.1);
			list.add(4.1);
			list.add(6.1);
			list.add(8.1);
			list.add(10.1);
			
			list.print();
			System.out.println("\nElement at index 2: " + list.get(2));
			System.out.println("Size of the list: " + list.getSize());
		}
}
```
## Java Generics and JavaFX
Using Java Generics with JavaFX for creating GUIs can add a layer of flexibility and type safety to your JavaFX applications.

Let's create a `TableView` that takes in a generic `Item` object as items to it.

`Item.java`
```java
package com.cmu.test;

public class Item<T> 
{
	private T data;
	public Item(T data)
	{
		this.data = data;
	}
	public T getData()
	{
		return this.data;
	}
	public void setData(T data)
	{
		this.data = data;
	}
	@Override
	public String toString()
	{
		return data.toString();
	}
}
```

`Driver.java`
```java
package com.cmu.test;

import java.util.ArrayList;
import javafx.application.Application;
import javafx.beans.property.SimpleStringProperty;
import javafx.scene.Scene;
import javafx.scene.control.TableColumn;
import javafx.scene.control.TableView;
import javafx.scene.layout.*;
import javafx.stage.Stage;

public class Driver extends Application {
    @Override
    public void start(Stage stage) 
    {
    	BorderPane pane = new BorderPane();
    	
    	TableView<Item<String>> tableView = new TableView<>();
    	
    	TableColumn<Item<String>, String> colName = new TableColumn<>("Name");
    	colName.setCellValueFactory(cellData -> new SimpleStringProperty(cellData.getValue().getData().toString()));
    	
    	tableView.getColumns().add(colName);
    	
    	ArrayList<Item<String>> items = new ArrayList<>();
    	items.add(new Item<>("Toyota"));
    	items.add(new Item<>("Honda"));
    	items.add(new Item<>("Audi"));
    	items.add(new Item<>("Hyundai"));
    	items.add(new Item<>("Suzuki"));
    	
    	tableView.getItems().addAll(items);
    	
    	
    	pane.setCenter(tableView);
      
    	Scene scene = new Scene(pane, 300, 300);

        stage.setScene(scene);
        stage.setTitle("MyGUI");
        stage.show();
    }

    public static void main(String[] args) {
        launch();
    }
}
```
The `Item` in above `TableView` is a generic object and can hold anything, e.g., String, int etc. Infact, it can also hold an object of another class. Let's demonstrate that by holding a `Fruit` class object in that generic `Item` object.

`Fruit`
```java
package com.cmu.test;

public class Fruit 
{
	private String name;
	private String color;
	private int quantity;
	
	Fruit(String name, String color, int quantity)
	{
		this.name = name;
		this.color = color;
		this.quantity = quantity;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getColor() {
		return color;
	}

	public void setColor(String color) {
		this.color = color;
	}

	public int getQuantity() {
		return quantity;
	}

	public void setQuantity(int quantity) {
		this.quantity = quantity;
	}
	
	public String toString()
	{
		return this.name + ", " + this.color + ", " + this.quantity;
	}
}
```
`Item.java`
```java
package com.cmu.test;

public class Item<T> 
{
	private T data;
	public Item(T data)
	{
		this.data = data;
	}
	public T getData()
	{
		return this.data;
	}
	public void setData(T data)
	{
		this.data = data;
	}
	@Override
	public String toString()
	{
		return data.toString();
	}
}
```
`Driver.java`
```java
package com.cmu.test;

import java.util.ArrayList;
import javafx.application.Application;
import javafx.beans.property.SimpleIntegerProperty;
import javafx.beans.property.SimpleStringProperty;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.TableColumn;
import javafx.scene.control.TableView;
import javafx.scene.layout.*;
import javafx.stage.Stage;


public class Driver extends Application {
    @Override
    public void start(Stage stage) 
    {
    	BorderPane pane = new BorderPane();
    	
    	TableView<Item<Fruit>> tableView = new TableView<>();
    	
    	TableColumn<Item<Fruit>, String> colName = new TableColumn<>("Name");
    	colName.setCellValueFactory(cellData -> new SimpleStringProperty(cellData.getValue().getData().getName()));
    	
    	TableColumn<Item<Fruit>, String> colColor = new TableColumn<>("Color");
    	colColor.setCellValueFactory(cellData -> new SimpleStringProperty(cellData.getValue().getData().getColor()));
    	
    	TableColumn<Item<Fruit>, Integer> colQuantity = new TableColumn<>("Quantity");
    	colQuantity.setCellValueFactory(cellData -> new SimpleIntegerProperty(cellData.getValue().getData().getQuantity()).asObject());
    	
    	tableView.getColumns().addAll(colName, colColor, colQuantity);
    	
    	ArrayList<Item<Fruit>> items = new ArrayList<>();
    	items.add(new Item<>(new Fruit("Apple", "Red", 4)));
    	items.add(new Item<>(new Fruit("Banana", "Yellow", 3)));
    	items.add(new Item<>(new Fruit("Cherry", "Red", 6)));
    	items.add(new Item<>(new Fruit("Melon", "Green", 2)));
    	
    	tableView.getItems().addAll(items);
    	
    	Button btnRemove = new Button("Remove Item");
    	
    	btnRemove.setOnAction(e -> {
    		Item<Fruit> item = tableView.getSelectionModel().getSelectedItem();
    		if (item != null)
    		{
    			items.remove(item);
    			tableView.getItems().remove(item);
    		}
    	});
    	
    	VBox vBox = new VBox(10);
    	vBox.setAlignment(Pos.CENTER);
    	vBox.getChildren().addAll(tableView, btnRemove);
    	vBox.setPadding(new Insets(20));
    	
    	pane.setCenter(vBox);
      
    	Scene scene = new Scene(pane, 300, 300);

        stage.setScene(scene);
        stage.setTitle("MyGUI");
        stage.show();
    }

    public static void main(String[] args) {
        launch();
    }
}
```

Generics give you the capability to parameterize types. You can define a class or a method with generic types, which are substituted with concrete types.The key benefit of generics is to enable errors to be detected at compile time rather than at runtime.A generic class or method permits you to specify allowable types of objects that the class or method can work with. If you attempt to use a class or method with an incompatible object, the compiler will detect the error.A generic type defined in a class, interface, or a static method is called a formal generic type, which can be replaced later with an actual concrete type. Replacing a generic type is called a generic instantiation.

## References
- Introduction to Java Programming and Data Structures, 13th edition, by Y Daniel Liang,
	- Chapter 19 (Generics)