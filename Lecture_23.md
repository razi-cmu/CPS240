# Dialogs in JavaFX

## Alert
AlertDialog is typically represented by the Alert class, which provides a simple way to display dialog windows with various types of messages (such as information, warnings, errors, or confirmations).

`MyGUI.java`
```java
package com.cmu;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Alert;
import javafx.scene.control.Alert.AlertType;
import javafx.scene.control.Button;
import javafx.scene.layout.BorderPane;
import javafx.stage.Stage;

public class MyGUI extends Application {

		public static void main(String[] args) {
			launch(args);
		}

		@Override
		public void start(Stage primaryStage) 
		{
			BorderPane pane = new BorderPane();
			
			Button btnShow  = new Button("Show Dialog");
			
			pane.setCenter(btnShow);
			
			Alert alert = new Alert(AlertType.INFORMATION);
			alert.setTitle("Information Dialog");
			alert.setHeaderText("This is the head text");
			alert.setContentText("This is the content text to provide information about this dialog");
			
			btnShow.setOnAction(e -> {
				alert.showAndWait();
			});

			Scene scene = new Scene(pane, 350, 300);
			primaryStage.setScene(scene);
			primaryStage.setTitle("Student Data");
			primaryStage.show();
		}
}
```

## Confirmation
A confirmation dialog lets the user respond with either "Yes" or "No." You can check the user's response and take action accordingly.

```java
@Override
public void start(Stage primaryStage) 
{
        BorderPane pane = new BorderPane();
        
        Button btnShow  = new Button("Show Dialog");
        
        pane.setCenter(btnShow);
        
        Alert alert = new Alert(AlertType.CONFIRMATION);
        alert.setTitle("Confirmation Dialog");
        alert.setHeaderText("Receipt confirmation");
        alert.setContentText("Do you want receipt?");
        
        btnShow.setOnAction(e -> {
        	alert.showAndWait().ifPresent(response -> {
        		if (response == ButtonType.OK)
        		{
        			System.out.println("OK button pressed");
        		}
        		else
        		{
        			System.out.println("Cancel button pressed");
        		}
        	});
        });
   
        Scene scene = new Scene(pane, 350, 300);
        primaryStage.setScene(scene);
        primaryStage.setTitle("Student Data");
        primaryStage.show();
}
```
The caption of the OK and CANCEL buttons can be changed using `getButtonTypes`. Note that alert buttons are not regular buttons from `Button` class, instead they are `ButtonType` class objects.

```java
@Override
public void start(Stage primaryStage) 
{
        BorderPane pane = new BorderPane();
        
        Button btnShow  = new Button("Show Dialog");
        
        pane.setCenter(btnShow);
        
        Alert alert = new Alert(AlertType.CONFIRMATION);
        alert.setTitle("Confirmation Dialog");
        alert.setHeaderText("Receipt confirmation");
        alert.setContentText("Do you want receipt?");
        
        alert.getButtonTypes().clear();
        
        ButtonType yesButton = new ButtonType("Yes");
        ButtonType noButton = new ButtonType("No");
        
        alert.getButtonTypes().addAll(yesButton, noButton);
        
        btnShow.setOnAction(e -> {
        	alert.showAndWait().ifPresent(response -> {
        		if (response == yesButton)
        		{
        			System.out.println("Yes button pressed");
        		}
        		else
        		{
        			System.out.println("No button pressed");
        		}
        	});
        });

        Scene scene = new Scene(pane, 350, 300);
        primaryStage.setScene(scene);
        primaryStage.setTitle("Student Data");
        primaryStage.show();
}
```

## Custom Dialog
JavaFX allows creating custom dialog using Dialog class that provide more flexibility and freedom to design your own custom alert dialogs with your custom theme and style.

```java
package com.cmu;

import javafx.application.Application;
import javafx.scene.Node;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.ButtonBar;
import javafx.scene.control.ButtonType;
import javafx.scene.control.Dialog;
import javafx.scene.control.Label;
import javafx.scene.control.PasswordField;
import javafx.scene.control.TextField;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.GridPane;
import javafx.stage.Stage;

public class MyGUI extends Application {
		@Override
		public void start(Stage primaryStage) 
		{
			BorderPane pane = new BorderPane();

			Button btnShow = new Button("Show Dialog");
			pane.setCenter(btnShow);
			
			btnShow.setOnAction(e -> {
				
				Dialog<ButtonType> dialog = new Dialog<>();
				dialog.setTitle("Login Dialog");
				
				TextField txtUserName = new TextField();
				PasswordField txtPassword = new PasswordField();
				
				ButtonType btnLogin = new ButtonType("Login");
				ButtonType btnCancel = new ButtonType("Cancel");
				
				GridPane grid = new GridPane();
				grid.add(new Label("Username:"), 0, 0);
				grid.add(txtUserName, 1, 0);
				grid.add(new Label("Password:"), 0, 1);
				grid.add(txtPassword, 1, 1);
				 
				dialog.getDialogPane().setContent(grid); 
				dialog.getDialogPane().getButtonTypes().addAll(btnLogin, btnCancel);
				
				dialog.showAndWait().ifPresent(response -> {
					if (response == btnLogin)
					{
						if (txtUserName.getText().equals("admin") && txtPassword.getText().equals("1234"))
						{
							System.out.println("Login Successful");
						}
						else
						{
							System.out.println("Invalid Credentials");
						}
					}
					else if (response == btnCancel)
					{
						System.out.println("Cancel Button Clicked");
					}
				});
			   
			});

			Scene scene = new Scene(pane, 350, 300);
			primaryStage.setScene(scene);
			primaryStage.setTitle("Student Data");
			primaryStage.show();
		}
		public static void main(String[] args) {
			launch(args);
		}
}
```

We can update the above program to show an image 

```java
package com.cmu;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.ButtonType;
import javafx.scene.control.Dialog;
import javafx.scene.control.Label;
import javafx.scene.control.PasswordField;
import javafx.scene.control.TextField;
import javafx.scene.image.Image;
import javafx.scene.image.ImageView;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.GridPane;
import javafx.scene.layout.StackPane;
import javafx.scene.paint.ImagePattern;
import javafx.scene.shape.Circle;
import javafx.geometry.Insets;
import javafx.stage.Stage;

public class MyGUI extends Application {
		@Override
		public void start(Stage primaryStage) {

			BorderPane pane = new BorderPane();

			Button btnShow = new Button("Show Dialog");
			pane.setCenter(btnShow);

			btnShow.setOnAction(e -> {

				Dialog<ButtonType> dialog = new Dialog<>();
				dialog.setTitle("Login Dialog");

				TextField txtUserName = new TextField();
				PasswordField txtPassword = new PasswordField();

				ButtonType btnLogin = new ButtonType("Login");
				ButtonType btnCancel = new ButtonType("Cancel");

				BorderPane loginPane = new BorderPane();

				Circle circle = new Circle(50, 50, 50);
				Image image = new Image("CMU_Yellow.jpg");
				circle.setFill(new ImagePattern(image));

				StackPane imageContainer = new StackPane(circle);

				StackPane.setMargin(circle, new Insets(20, 0, 20, 0)); 

				loginPane.setTop(imageContainer);

				GridPane grid = new GridPane();
				grid.add(new Label("Username"), 0, 0);
				grid.add(txtUserName, 1, 0);
				grid.add(new Label("Password"), 0, 1);
				grid.add(txtPassword, 1, 1);
				grid.setHgap(10);

				loginPane.setCenter(grid);
				loginPane.setPadding(new Insets(10));

				dialog.getDialogPane().setContent(loginPane);
				dialog.getDialogPane().getButtonTypes().addAll(btnLogin, btnCancel);

				dialog.showAndWait().ifPresent(response -> 
				{
					if (response == btnLogin) 
					{
						if (txtUserName.getText().equals("admin") && txtPassword.getText().equals("1234")) 
						{
							System.out.println("Login Successful");
						} else {
							System.out.println("Invalid Credentials");
						}
					} 
					else if (response == btnCancel) 
					{
						System.out.println("Cancel Button Clicked");
					}
				});
			});

			Scene scene = new Scene(pane, 350, 300);
			primaryStage.setScene(scene);
			primaryStage.setTitle("Student Data");
			primaryStage.show();
		}
		public static void main(String[] args) {
			launch(args);
		}
}
```

You might have noticed that even if login is not successful, the dialog closes. We can control this behaviour

```java
package com.cmu;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.ButtonType;
import javafx.scene.control.Dialog;
import javafx.scene.control.Label;
import javafx.scene.control.PasswordField;
import javafx.scene.control.TextField;
import javafx.scene.image.Image;
import javafx.scene.image.ImageView;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.GridPane;
import javafx.scene.layout.StackPane;
import javafx.scene.paint.ImagePattern;
import javafx.scene.shape.Circle;
import javafx.geometry.Insets;
import javafx.stage.Stage;
import javafx.stage.Window;

public class MyGUI extends Application {

		@Override
		public void start(Stage primaryStage) {

			BorderPane pane = new BorderPane();

			Button btnShow = new Button("Show Dialog");
			pane.setCenter(btnShow);

			btnShow.setOnAction(e -> {

				Dialog<ButtonType> dialog = new Dialog<>();
				dialog.setTitle("Login Dialog");

				TextField txtUserName = new TextField();
				PasswordField txtPassword = new PasswordField();

				ButtonType btnLogin = new ButtonType("Login");
				ButtonType btnCancel = new ButtonType("Cancel");

				BorderPane loginPane = new BorderPane();

				Circle circle = new Circle(50, 50, 50);
				Image image = new Image("CMU_Yellow.jpg");
				circle.setFill(new ImagePattern(image));

				StackPane imageContainer = new StackPane(circle);

				StackPane.setMargin(circle, new Insets(20, 0, 20, 0)); 

				loginPane.setTop(imageContainer);

				GridPane grid = new GridPane();
				grid.add(new Label("Username"), 0, 0);
				grid.add(txtUserName, 1, 0);
				grid.add(new Label("Password"), 0, 1);
				grid.add(txtPassword, 1, 1);
				grid.setHgap(10);

				loginPane.setCenter(grid);
				loginPane.setPadding(new Insets(10));

				dialog.getDialogPane().setContent(loginPane);
				dialog.getDialogPane().getButtonTypes().addAll(btnLogin, btnCancel);
				
			   dialog.setResultConverter(button -> {
				   if (button == btnLogin)
				   {
					   if (txtUserName.getText().equals("admin") && txtPassword.getText().equals("1234")) 
					   {
						   System.out.println("Login Successful");
						   return btnLogin; 
					   }
					   else
					   {
						   System.out.println("Invalid Credentials");
						   return null;
					   }
				   }
				   else if (button == btnCancel)
				   {
					   System.out.println("Cancel Button Clicked");
					   return btnCancel;
				   }
				   return null;
			   }); 
			   
			   Window window = dialog.getDialogPane().getScene().getWindow();
			   window.setOnCloseRequest(event -> window.hide());
			   
			   dialog.showAndWait();
			});

			Scene scene = new Scene(pane, 350, 300);
			primaryStage.setScene(scene);
			primaryStage.setTitle("Student Data");
			primaryStage.show();
		}
		public static void main(String[] args) {
			launch(args);
		}
}
```