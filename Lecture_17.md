# GUI (Basic Components and Event Driven Programming)

## JavaFX Basics
JavaFX is a new framework for developing Java GUI programs. The JavaFX API is an excellent example of how the object-oriented principles are applied. JavaFX is the recommended framework for developing modern Java applications, providing a rich set of features and tools to create dynamic, user-friendly interfaces.

When Java was introduced, the GUI classes were bundled in a library known as the Abstract Windows Toolkit (AWT). AWT is fine for developing simple graphical user interfaces, but not for developing comprehensive GUI projects. In addition, AWT is prone to platform-specific bugs. The AWT user-interface components were replaced by a more robust, versatile, and flexible library known as Swing. Swing components depend less on the target platform, and use less of the native GUI resources. Swing is designed for developing desktop GUI applications. It is now replaced by a completely new GUI platform known as JavaFX. JavaFX incorporates modern GUI technologies to enable you to develop rich GUI applications. In addition, JavaFX provides a multitouch support for touch-enabled devices such as tablets and smart phones. JavaFX has a built-in 2D, 3D, animation support, and video and audio playback. Using third-party software, you can develop JavaFX programs to be deployed on devices running iOS or Android.

### Zulu SDK with JavaFX
If you have Zulu SDK with JavaFX installed on your system, then JavaFX framework should work fine without any further configurations. It is recommended to install JavaFX plugin from Eclipse Marketplace (see details in the next section).

### Setting up JavaFX in Eclipse (Non-Zulu SDK)
If you have installed any Non-Zulu SDKs, you might need to install JavaFX manually by following the below steps:
* Download JavaFX SDK from https://openjfx.io/index.html
* Once downloaded, extract to some location on your computer
* In Eclipse go to Preferences > Build Path > User Libraries and add a new library "JavaFX" and from Add External JARs, add all the JAR files in the downloaded folder's lib folder
* Apply and Close.
* In Eclipse > Help > Eclipse Marketplace, add JavaFX plugin
* You can now create a new Java Project
  
In some cases, you might have to add the above created User Library manually to your Java projects. Follow steps below:
* Right Click your Java Project in Eclipse and Click Build Path > Configure Build Path
* Select the Libraries Tab and click Classpath.
* Click Add Library > User Library, select JavaFX and click Finish.
  
In most of the cases, the above would solve the problem of JavaFX classes not available to use. However, in extreme cases you might have to configure the runtime environment for JavaFX
* In Eclipse in the Menu Bar select Run > Run ConfigurationS. A window will pop up; click Arguments and paste the following in the VM Arguments:
```
--module-path "D:\Utils\openjfx-23.0.1_windows-x64_bin-sdk\javafx-sdk-23.0.1/lib" --add-modules javafx.controls,javafx.fxml
```
Make sure you update the above highlighted part to the path of JavaFX SDK you downloaded and extracted.
	
### First JavaFX Application
Once JavaFX is setup, you are now ready to create a GUI application using JavaFX in Java. Let's write our first JavaFX program.

`MyGUI.java`
```java
package com.cmu;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.stage.Stage;

public class MyGUI extends Application 
{

		@Override
		public void start(Stage primaryStage) throws Exception 
		{
			Button btn = new Button("Click me");
			Scene scene = new Scene(btn, 300, 300);
			
			primaryStage.setTitle("My GUI");
			primaryStage.setScene(scene);
			primaryStage.show();

			
		}
		
		public static void main(String[] args) {
			launch(args);
		}

}
```	

The `launch` method is a static method defined in the `Application` class for launching a stand-alone JavaFX application. The `main` method is not needed if you run the program from the command line. It may be needed to launch a JavaFX program from an IDE with a limited JavaFX support. When you run a JavaFX application without a main method, JVM automatically invokes the  method to run the application.

The main class overrides the `start` method defined in `javafx.application.Application`. After a JavaFX application is launched, the JVM constructs an instance of the class using its `no-args` constructor and invokes its `start` method. The `start` method normally places UI controls in a scene and displays the scene in a stage.

`Scene` object can be created using the constructor `Scene(node, width, height)`. This constructor specifies the width and height of the scene and places the node in the scene.

A `Stage` object is a window. A `Stage` object called primary stage is automatically created by the JVM when the application is launched. We set the scene to the primary stage and display the primary stage. JavaFX names the `Stage` and `Scene` classes using the analogy from the theater. You may think of stage as the platform to support scenes, and nodes as actors to perform in the scenes.

By default, the user can resize the stage. To prevent the user from resizing the stage, invoke `stage.setResizable(false)`.

	
### Multistage JavaFX Application
Multiple stages can be added to a JavaFX Application. Let's write code for a multistage JavaFX application.

`MyGUI.java`
```java
package com.cmu;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.stage.Stage;

public class MyGUI extends Application 
{

		@Override
		public void start(Stage primaryStage) throws Exception 
		{
			Button btn = new Button("Click me");
			Scene scene = new Scene(btn, 300, 300);
			
			primaryStage.setTitle("My GUI");
			primaryStage.setScene(scene);
			primaryStage.show();
			
			Stage stage = new Stage();
			stage.setTitle("Another Scene");
			Scene scene2 = new Scene(new Button("Hello"), 200, 200);
			stage.setScene(scene2);
			stage.show();
			
		}
		
		public static void main(String[] args) {
			launch(args);
		}

}
```

### Grouping multiple nodes
A Group helps in holding multiple nodes. The code below creates a Group, adds two buttons to it and then adds the group to the scene. A Group will take on the collective bounds of its children and is not directly resizable. `Group` class inherits from the `Parent` class.

`MyGUI.java`
```java
package com.cmu;
import javafx.application.Application;
import javafx.scene.Group;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.stage.Stage;

public class MyGUI extends Application 
{

		@Override
		public void start(Stage primaryStage) throws Exception 
		{
			Button btn1 = new Button("Click me");
			Button btn2 = new Button("Click me 2");
			
			btn1.setLayoutX(50);
			btn1.setLayoutY(100);
			
			btn2.setLayoutX(150);
			btn2.setLayoutY(100);
			
			Group group = new Group(btn1, btn2);
			Scene scene = new Scene(group, 300, 300);
			
			primaryStage.setTitle("My GUI");
			primaryStage.setScene(scene);
			primaryStage.show();
	
		}
		
		public static void main(String[] args) {
			launch(args);
		}

}
```
### Working with Text
A `Text` can also be added to a Group. A `Text` supports fine control over font, size, and styling. It also allows precise positioning (x, y coordinates) along with providing effects like rotation, scaling, transforms. It does not support built-in features like tooltips or accessibility roles. If you are interested in using these features, use `Label` instead.

```java
@Override
public void start(Stage primaryStage) throws Exception 
{
		Button btn1 = new Button("Click me");

		Text txtHello = new Text("Hello I am Text");

		btn1.setLayoutX(110);
		btn1.setLayoutY(100);
		
		txtHello.setLayoutX(110);
		txtHello.setLayoutY(160);
		
		Group group = new Group(btn1, txtHello);
		Scene scene = new Scene(group, 300, 300);
		
		primaryStage.setTitle("My GUI");
		primaryStage.setScene(scene);
		primaryStage.show();
		
}
```

### Look and Feel of Nodes
Several properties of nodes can be changed, e.g., width, font and color etc. These properties can be changed using appropriate methods provided to the node class.

`Main.java`
```java
package com.cmu;

import javafx.application.Application;
import javafx.scene.Group;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.paint.Color;
import javafx.scene.text.Font;
import javafx.scene.text.Text;
import javafx.stage.Stage;

public class MyGUI extends Application 
{

		@Override
		public void start(Stage primaryStage) throws Exception 
		{
			Button btn1 = new Button("Click me");
			btn1.setMinWidth(80);
			btn1.setFont(new Font("Arial", 18));
			Text txtHello = new Text("Hello I am Text");
			txtHello.setFont(new Font("Arial", 18));
			txtHello.setFill(Color.RED);
			
			
			btn1.setLayoutX(110);
			btn1.setLayoutY(100);
			
			txtHello.setLayoutX(110);
			txtHello.setLayoutY(160);
			
			Group group = new Group(btn1, txtHello);
			Scene scene = new Scene(group, 300, 300);
			
			primaryStage.setTitle("My GUI");
			primaryStage.setScene(scene);
			primaryStage.show();
			
		}
		
		public static void main(String[] args) {
			launch(args);
		}

}
```

### Working with Shapes
Shapes can also be drawn to a scene in JavaFX Application. Several built-in shapes are available like Circle, Square and Rectangle etc.

```java
@Override
public void start(Stage primaryStage) throws Exception 
{
		Circle circle = new Circle(150,110,50);
		
		Pane pane = new Pane(circle);
		
		Scene scene = new Scene(pane, 300, 300);
		
		primaryStage.setTitle("My GUI");
		primaryStage.setScene(scene);
		primaryStage.show();
		
}
```

Although adding circle to a group and then adding that group to a scene would still work but it does not make sense to add a circle in a group because its just a single component. It is a good idea to use a `Pane` to add a single item that needs to be added to a scene.
