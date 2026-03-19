# Event Driven Programming
Suppose you wish to write a GUI program that lets the user enter a loan amount, annual interest rate, and number of years then click the Calculate button to obtain the monthly payment and total payment. How do you accomplish the task? You have to use event-driven programming to write the code to respond to the button-clicking event. <br>
To respond to a button click, you need to write the code to process the button-clicking action. The button is an event source object where the action originates. You need to create an object capable of handling the action event on a button. This object is called an event handler. <br>
Not all objects can be handlers for an action event. To be a handler of an action event, two requirements must be met:
- The object must be an instance of the `EventHandler<T extends Event>` interface. This interface defines the common behavior for all handlers. `<T extends Event>` denotes that `T` is a generic type that is a subtype of `Event`.
- The `EventHandler` object `handler` must be registered with the event source object using the method `source.setOnAction(handler)`.

The `EventHandler<ActionEvent>` interface contains the `handle(ActionEvent)` method for processing the action event. Your handler class must override this method to respond to the event. 

## Event Handling
The `EventHandler` interface contains the  method for processing the action event `handle(ActionEvent)`. Your handler class must override this method to respond to the event.

`Driver.java`
```java
package com.cmu.test;

import javafx.application.Application;
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.scene.Group;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.layout.Pane;
import javafx.scene.paint.Color;
import javafx.scene.shape.Circle;
import javafx.scene.text.Font;
import javafx.scene.text.Text;
import javafx.stage.Stage;


class ButtonHandler implements EventHandler<ActionEvent>
{
	@Override
	public void handle(ActionEvent event) 
	{
		System.out.println("Button Clicked");
	}
	
}

public class Driver extends Application
{
	public static void main(String[] args) 
	{
		launch(args);
	}
	@Override
	public void start(Stage primaryStage) throws Exception 
	{
		Button btnShow = new Button("Show Text");
		btnShow.setLayoutX(100);
		btnShow.setLayoutY(150);
		
		btnShow.setOnAction(new ButtonHandler());
		
		Pane pane = new Pane(btnShow);
		
		Scene scene = new Scene(pane, 300,300);
		
		primaryStage.setTitle("My GUI");
		primaryStage.setScene(scene);
		primaryStage.show();	
	}
}

```

In the above program, we created a separate `ButtonHandler` class that implements `EventHandler<ActionEvent>` and perform the required action, e.g., show message on the console. Let's modify this code to show that message on a `Text` instead of a console.

`Driver.java`
```java
package com.cmu.test;

import javafx.application.Application;
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.Pane;
import javafx.scene.text.Text;
import javafx.stage.Stage;


class ButtonHandler implements EventHandler<ActionEvent>
{
	private Text txt;
	ButtonHandler(Text txt)
	{
		this.txt = txt;
	}
	@Override
	public void handle(ActionEvent event) 
	{
		System.out.println("Button Clicked");
		txt.setText("Button Clicked");
	}
}

public class Driver extends Application
{
	public static void main(String[] args) 
	{
		launch(args);
	}
	@Override
	public void start(Stage primaryStage) throws Exception 
	{
		Text txt = new Text("Initial Text");
		txt.setLayoutX(100);
		txt.setLayoutY(80);
		
		Button btnShow = new Button("Show Text");
		btnShow.setLayoutX(100);
		btnShow.setLayoutY(150);
		
		btnShow.setOnAction(new ButtonHandler(txt));
		
		Pane pane = new Pane(btnShow, txt);
		
		Scene scene = new Scene(pane, 300,300);
		
		primaryStage.setTitle("My GUI");
		primaryStage.setScene(scene);
		primaryStage.show();	
	}
}
```
In the above program, we created a separate class for handling button action event, however, it can be simplified by implementing the `EventHandler` in the same class.

`Driver.java`
```java	
package com.cmu.test;

import javafx.application.Application;
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.Pane;
import javafx.scene.text.Text;
import javafx.stage.Stage;

public class Driver extends Application implements EventHandler<ActionEvent>
{
	private Text txt;
	public static void main(String[] args) 
	{
		launch(args);
	}
	@Override
	public void start(Stage primaryStage) throws Exception 
	{
		txt = new Text("Initial Text");
		txt.setLayoutX(100);
		txt.setLayoutY(80);
		
		Button btnShow = new Button("Show Text");
		btnShow.setLayoutX(100);
		btnShow.setLayoutY(150);
		
		btnShow.setOnAction(this);
		
		Pane pane = new Pane(btnShow, txt);

		Scene scene = new Scene(pane, 300,300);
		
		primaryStage.setTitle("My GUI");
		primaryStage.setScene(scene);
		primaryStage.show();
	}
	@Override
	public void handle(ActionEvent event) 
	{
		System.out.println("Button Clicked");
		txt.setText("Button Clicked");
	}
}

```		
## Anonymous Inner Class Handler
In order to simplify the handling of events, annonymous inner classes can be used to handle the action events. The above code can be simplified using annonymous inner class handlers as below:

`Driver.java`
```java
package com.cmu.test;

import javafx.application.Application;
import javafx.event.ActionEvent;
import javafx.event.EventHandler;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.Pane;
import javafx.scene.text.Text;
import javafx.stage.Stage;

public class Driver extends Application
{
	private Text txt;
	public static void main(String[] args) 
	{
		launch(args);
	}
	@Override
	public void start(Stage primaryStage) throws Exception 
	{
		txt = new Text("Initial Text");
		txt.setLayoutX(100);
		txt.setLayoutY(80);
		
		Button btnShow = new Button("Show Text");
		btnShow.setLayoutX(100);
		btnShow.setLayoutY(150);
		
		btnShow.setOnAction(new EventHandler<ActionEvent>() {

			@Override
			public void handle(ActionEvent event) 
			{
				System.out.println("Button Clicked");
				txt.setText("Button Clicked");
			}
		});
		
		Pane pane = new Pane(btnShow, txt);
		
		Scene scene = new Scene(pane, 300,300);
		
		primaryStage.setTitle("My GUI");
		primaryStage.setScene(scene);
		primaryStage.show();	
	}
}

```

## Event Handlers and Lambda Expressions
We remember from our previous lectures that if an interface has only one method, we can use lambda expressions to further simplify their implementation.

`Driver.java`
```java
package com.cmu.test;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.Pane;
import javafx.scene.text.Text;
import javafx.stage.Stage;

public class Driver extends Application
{
	private Text txt;
	public static void main(String[] args) 
	{
		launch(args);
	}
	@Override
	public void start(Stage primaryStage) throws Exception 
	{
		txt = new Text("Initial Text");
		txt.setLayoutX(100);
		txt.setLayoutY(80);
		
		Button btnShow = new Button("Show Text");
		btnShow.setLayoutX(100);
		btnShow.setLayoutY(150);
		
		btnShow.setOnAction(event-> {
				System.out.println("Button Clicked");
				txt.setText("Button Clicked");
			
		});
		
		Pane pane = new Pane(btnShow, txt);
		
		Scene scene = new Scene(pane, 300,300);
		
		primaryStage.setTitle("My GUI");
		primaryStage.setScene(scene);
		primaryStage.show();	
	}
}
```

You might notice we used `event->` instead of `()->` for this lambda funcion. The reason for this is that `handle` function requires a parameter unlike the `run` function in `Runnable`. Below is the definition of the handle function that shows it needs a parameter to run and hence we provided it a parameter `event->` instead of no parameter, e.g., `()->`:
```java
void handle(T event)
	Invoked when a specific event of the type for which this handler is registered happens.
	
	Parameters:
	event - the event which occurred
```

## Handling Events with Shapes
Let's now work with shapes and event handling. Let's create a Java program that resizes the circle on the click of a button.

`Driver.java`
```java	
package com.cmu.test;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.Pane;
import javafx.scene.paint.Color;
import javafx.scene.shape.Circle;
import javafx.stage.Stage;

public class Driver extends Application
{

	public static void main(String[] args) 
	{
		launch(args);
	}
	@Override
	public void start(Stage primaryStage) throws Exception 
	{
		Circle circle = new Circle(50);
		
		Button btnShow = new Button("Reduce Size");
		btnShow.setLayoutX(110);
		btnShow.setLayoutY(220);
		
		btnShow.setOnAction(event-> {
			circle.setRadius(30);
			circle.setFill(new Color(0.5,0.0,0.0,0.5));
			
		});
			
		Pane pane = new Pane();
		
		circle.centerXProperty().bind(pane.widthProperty().divide(2));
		circle.centerYProperty().bind(pane.heightProperty().divide(2));
		
		pane.getChildren().add(btnShow);
		pane.getChildren().add(circle);

		Scene scene = new Scene(pane, 300,300);
		
		primaryStage.setTitle("My GUI");
		primaryStage.setScene(scene);
		primaryStage.show();
	}
}

```
In the above program, the circle always stays at the center of the pane because the center of the circle is bound to the center of the pane, e.g., pane's width and height divided by 2. Furthermore, you might have noticed the color of the circle which is not using built-in colors and instead creating a color based on the values of red, green, blue and opacity.

## Mouse Events
A Mouse Event is fired whenever a mouse button is pressed, released, clicked, moved, or dragged on a node or a scene. Let's write a program that shows on a text a message whenenver cursor enters the circle and changes it back to initial text when cursor exits the circle.

`Driver.java`
```java	
package com.cmu.test;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.Pane;
import javafx.scene.paint.Color;
import javafx.scene.shape.Circle;
import javafx.scene.text.Text;
import javafx.stage.Stage;

public class Driver extends Application
{
	public static void main(String[] args) 
	{
		launch(args);
	}
	@Override
	public void start(Stage primaryStage) throws Exception 
	{
		Circle circle = new Circle(50);
		
		Text txt = new Text("Initial Text");
		txt.setLayoutX(120);
		txt.setLayoutY(280);
		
		Button btnShow = new Button("Reduce Size");
		btnShow.setLayoutX(110);
		btnShow.setLayoutY(220);
		
		btnShow.setOnAction(event-> {
			circle.setRadius(30);
			circle.setFill(Color.web("#FF000080"));
			
		});
			
		Pane pane = new Pane();
		
		circle.centerXProperty().bind(pane.widthProperty().divide(2));
		circle.centerYProperty().bind(pane.heightProperty().divide(2));
		
		pane.getChildren().add(btnShow);
		pane.getChildren().add(circle);
		pane.getChildren().add(txt);
		
		circle.setOnMouseEntered(event -> {
			txt.setText("Cursor entered circle");
		});
		
		circle.setOnMouseExited(event -> {
			txt.setText("Initial Text");
		});

		Scene scene = new Scene(pane, 300,300);
		
		primaryStage.setTitle("My GUI");
		primaryStage.setScene(scene);
		primaryStage.show();
	}
}

```

## Key Events

Besides capturing the mouse events, we can capture keyboard's key events. Let's create a program that moves the circle up and down on pressing the UP and DOWN arrow keys on the keyboard.

`Driver.java`
```java	
package com.cmu.test;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.layout.Pane;
import javafx.scene.shape.Circle;
import javafx.stage.Stage;

public class Driver extends Application
{

	public static void main(String[] args) 
	{
		launch(args);
	}
	@Override
	public void start(Stage primaryStage) throws Exception 
	{
		Circle circle = new Circle(150,150,20);

		Pane pane = new Pane();
		
		pane.getChildren().add(circle);

		Scene scene = new Scene(pane, 300,300);
		
		scene.setOnKeyPressed(e -> {
			switch (e.getCode())
			{
				case UP: 
					circle.setCenterY(circle.getCenterY()-10); 
					break;
				case DOWN:
					circle.setCenterY(circle.getCenterY()+10);
					break;
				default:
					break;
			}
		});
		
		primaryStage.setTitle("My GUI");
		primaryStage.setScene(scene);
		primaryStage.show();
	}
}

```
In the above code, on the press of UP arrow on the keyboard, circle's vertical center (Y) is changed by 10 units by subtracting 10 from the current vertical center of the circle. Same goes with press of DOWN arrow on the keyboard, except 10 units are adding to the current vertical center of the circle.

`Note`: An important thing to note here is that you cannot keep X and Y bound to the panel and then try to set the values of X or Y manually. For example, you'll get an error in the following code because the code is asking JavaFX to manage the X and Y positions of the circle by binding them to the pane and then later trying to modify the values manually:

```java
public void start(Stage primaryStage) throws Exception 
{
		Circle circle = new Circle(20);

		Pane pane = new Pane();
		
		circle.centerXProperty().bind(pane.widthProperty().divide(2));
		circle.centerYProperty().bind(pane.heightProperty().divide(2));
		
		pane.getChildren().add(circle);

		Scene scene = new Scene(pane, 300,300);
		
		scene.setOnKeyPressed(e -> {
			switch (e.getCode())
			{
				case UP: 
					circle.setCenterY(circle.getCenterY()-10); 
					break;
				case DOWN:
					circle.setCenterY(circle.getCenterY()+10);
					break;
				default:
					break;
			}
		});
		
		primaryStage.setTitle("My GUI");
		primaryStage.setScene(scene);
		primaryStage.show();
}

```

The above can be solved using Translation as below which will keep the circle centered and still allow moving it UP or DOWN by modifying the values of X and Y.

```java
public void start(Stage primaryStage) throws Exception 
{
		Circle circle = new Circle(20);

		Pane pane = new Pane();
		
		circle.centerXProperty().bind(pane.widthProperty().divide(2));
		circle.centerYProperty().bind(pane.heightProperty().divide(2));
		
		pane.getChildren().add(circle);

		Scene scene = new Scene(pane, 300,300);
		
		scene.setOnKeyPressed(e -> {
			switch (e.getCode())
			{
			case UP:
			    circle.setTranslateY(circle.getTranslateY() - 10);
			    break;
			case DOWN:
			    circle.setTranslateY(circle.getTranslateY() + 10);
			    break;
				default:
					break;
			}
		});
		
		primaryStage.setTitle("My GUI");
		primaryStage.setScene(scene);
		primaryStage.show();
}

```