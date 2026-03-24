# Java GUI Layouts
JavaFX provides many types of panes for automatically laying out nodes in a desired location and size. Panes and groups are the containers for holding nodes. The `Group` class is often used to group nodes and to perform transformation and scale as a group. Panes and UI control objects are resizable, but group, shape, and text objects are not resizable. JavaFX provides many types of panes for organizing nodes in a container. `Pane` is the base class for all specialized panes.


## StackPane
The `StackPane` places the nodes in the center of the pane on top of each other. The node added first is placed at the bottom of the stack and the next node is placed on top of it. The class named `StackPane` of the package `javafx.scene.layout` represents the stack pane layout. Let's see the working of `StackPane`.

`MyGUI.java`
```java

package com.cmu;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Label;
import javafx.scene.layout.StackPane;
import javafx.scene.paint.Color;
import javafx.scene.shape.Circle;
import javafx.stage.Stage;

public class MyGUI extends Application
{

		@Override
		public void start(Stage primaryStage) throws Exception 
		{
			Label lblText = new Label("CMU");
			lblText.setTextFill(Color.WHITE);
			
			Circle circle = new Circle(50);
			
			StackPane pane = new StackPane();
			
			pane.getChildren().addAll(circle, lblText);
			
			Scene scene = new Scene(pane, 200, 200);
			
			primaryStage.setTitle("My GUI");
			primaryStage.setScene(scene);
			primaryStage.show();
			
		}
		public static void main(String[] args) 
		{
			launch(args);
			
		}
}

```
`StackPane` uses `setAlignment()` method to set the position of its children

```java
pane.setAlignment(Pos.TOP_CENTER);
```

`StackPane` does not have a `fill` method, so in order to fill color in a `StackPane` we can use `setStyle` method as below:

```java
StackPane pane = new StackPane();
pane.setStyle("-fx-background-color: lightblue;");
pane.setAlignment(Pos.TOP_CENTER);
pane.getChildren().addAll(circle, lbl);

```

## FlowPane
FlowPane arranges the nodes in the pane horizontally from left to right, or vertically from top to bottom, in the order in which they were added. When one row or one column is filled, a new row or column is started. You can specify the way the nodes are placed horizontally or vertically using one of two constants: `Orientation.HORIZONTAL` or `Orientation.VERTICAL`. You can also specify the gap between the nodes in pixels.

Data fields `alignment`, `orientation`, `hgap` and `vgap` are binding properties. Recall that each binding property in JavaFX has a getter method (e.g., `getHgap()`) that returns its value, a setter method (e.g., `setHgap(double)`) for setting a value, and a getter method that returns the property itself (e.g., `hgapProperty()`). For a data field of `ObjectProperty<T>` type, the value getter method returns a value of type `T`, and the property getter method returns a property value of type `ObjectProperty<T>`. Let's use `FlowPane` in our code.


`MyGUI.java`
```java
package com.cmu;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.control.TextField;
import javafx.scene.layout.FlowPane;
import javafx.stage.Stage;

public class MyGUI extends Application
{

		@Override
		public void start(Stage primaryStage) throws Exception 
		{
			FlowPane pane = new FlowPane();
			
			pane.getChildren().addAll(new Label("User Name"), new TextField());
			pane.getChildren().addAll(new Label("Password"), new TextField());
			
			Scene scene = new Scene(pane, 200, 200);
			
			primaryStage.setTitle("My GUI");
			primaryStage.setScene(scene);
			primaryStage.show();
			
		}
		public static void main(String[] args) 
		{
			launch(args);
			
		}
}
```

Paddings and Gaps can be set for FlowPane:

```Java
@Override
public void start(Stage primaryStage) throws Exception 
{
		FlowPane pane = new FlowPane();
		
		pane.getChildren().addAll(new Label("User Name"), new TextField());
		pane.getChildren().addAll(new Label("Password"), new TextField());
		
		pane.setPadding(new Insets(11, 12, 13, 14));
		
		pane.setVgap(15);
		
		Scene scene = new Scene(pane, 200, 200);
		
		primaryStage.setTitle("My GUI");
		primaryStage.setScene(scene);
		primaryStage.show();
}

```

Layouts can be mixed and matched to create GUI which can sometimes be complex in nature.

```java
@Override
public void start(Stage primaryStage) throws Exception 
{
		FlowPane userNamePane = new FlowPane(Orientation.HORIZONTAL);
	    userNamePane.getChildren().addAll(new Label("User Name"), new TextField());
	    userNamePane.setHgap(10);  
	    userNamePane.setAlignment(Pos.CENTER);

	    FlowPane passwordPane = new FlowPane(Orientation.HORIZONTAL);
	    passwordPane.getChildren().addAll(new Label("Password"), new PasswordField());
	    passwordPane.setHgap(18);
	    passwordPane.setAlignment(Pos.CENTER);
	    
	    FlowPane buttonsPane = new FlowPane(Orientation.HORIZONTAL);
	    buttonsPane.getChildren().addAll(new Button("Login"), new Button("Cancel"));
	    buttonsPane.setHgap(18);
	    buttonsPane.setPadding(new Insets(0,0,0,60));
	    buttonsPane.setAlignment(Pos.CENTER);
	    
	    FlowPane pane = new FlowPane(Orientation.VERTICAL);
	    pane.getChildren().addAll(userNamePane, passwordPane, buttonsPane);
	    pane.setPadding(new Insets(20)); 
	    pane.setVgap(10);
	    pane.setAlignment(Pos.CENTER);
	    
	    Scene scene = new Scene(pane, 300, 300); 

	    primaryStage.setTitle("My GUI");
	    primaryStage.setScene(scene);
	    primaryStage.show();
}
```

You can set the border of the pane to see the size of the pane. Below is an example of setting the border for the main pane.

```java
pane.setStyle("-fx-border-color: green; -fx-border-width: 2; -fx-border-style: solid;");

```

## GridPane
A  arranges nodes in a grid (matrix) formation. The nodes are placed in the specified column and row indices. By default, the grid pane will resize rows and columns to the preferred sizes of its contents, even if the grid pane is resized larger than its preferred size. You may purposely set a large value for the preferred width and height of its contents by invoking the `setPrefWidth` and `setPrefHeight` methods, so the contents will be automatically stretched to fill in the grid pane when the grid pane is enlarged.

`MyGUI.java`
```java
import javafx.application.Application;
import javafx.geometry.HPos;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.Label;
import javafx.scene.control.PasswordField;
import javafx.scene.control.TextField;

import javafx.scene.layout.GridPane;

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
		GridPane pane = new GridPane();
		pane.setPadding(new Insets(11, 12, 13, 14));
		pane.setHgap(15);
		pane.setVgap(5);
		pane.add(new Label("User Name"), 0, 0);
		pane.add(new TextField(), 1, 0);
		pane.add(new Label("Password"), 0, 1);
		pane.add(new PasswordField(), 1, 1);
		
		Button btnLogin = new Button("Login");
		Button btnCancel = new Button("Cancel");
		
		pane.add(btnLogin, 1, 3);
		pane.add(btnCancel, 1, 3);
		
		GridPane.setMargin(btnLogin, new Insets(0, 10, 0, 0));
		
		GridPane.setHalignment(btnLogin, HPos.CENTER);
		GridPane.setHalignment(btnCancel, HPos.RIGHT);
		
		pane.setAlignment(Pos.CENTER);

		Scene scene = new Scene(pane, 300,300);
		
		primaryStage.setTitle("My GUI");
		primaryStage.setScene(scene);
		primaryStage.show();
	}
}
```
