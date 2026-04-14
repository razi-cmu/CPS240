# Progress Bars in JavaFX

## ProgressBar
The progress bar usually shows the amount of completion of a task. A ProgressBar is a visual component that provides feedback to the user on the progress of a task. It is used to show the progress of ongoing processes, such as file uploads, downloads, or calculations. It helps to inform the user about the status of operations that may take time. Represents progress from 0.0 (start) to 1.0 (completion). It can be updated programmatically using `.setProgress()`.

`MyGUI.java`
```java
package com.cmu;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.ProgressBar;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.VBox;
import javafx.geometry.Pos;
import javafx.stage.Stage;

public class MyGUI extends Application {

		@Override
		public void start(Stage primaryStage) {

			BorderPane pane = new BorderPane();
			VBox vBox = new VBox(10);
			vBox.setAlignment(Pos.CENTER);

			Button btnStart = new Button("Start");
			ProgressBar progressBar = new ProgressBar(0);
			progressBar.setPrefWidth(200);
			
			btnStart.setOnAction(e -> {

					for (double i = 0; i <= 1; i += 0.01)
					{
						progressBar.setProgress(i);
						
						try 
						{
							Thread.sleep(100);
						} 
						catch (InterruptedException e1) 
						{
					
							e1.printStackTrace();
						}
					}

				
			});
			
			vBox.getChildren().addAll(progressBar, btnStart);
			 
			pane.setCenter(vBox);

			Scene scene = new Scene(pane, 350, 300);
			primaryStage.setScene(scene);
			primaryStage.setTitle("My GUI");
			primaryStage.show();
		}

		public static void main(String[] args) {
			launch(args);
		}
}
```
In the above program that UI is blocked because the progress of the progress bar is implemented on the main thread which blocks the main thread.

In order to make sure the main thread is not blocked, expensive work should always be done in a non-main thread. So, let's put progress bar's progress in a new thread.

```java
package com.cmu;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.ProgressBar;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.VBox;
import javafx.geometry.Pos;
import javafx.stage.Stage;

public class MyGUI extends Application {

		@Override
		public void start(Stage primaryStage) {

			BorderPane pane = new BorderPane();
			VBox vBox = new VBox(10);
			vBox.setAlignment(Pos.CENTER);

			Button btnStart = new Button("Start");
			ProgressBar progressBar = new ProgressBar(0);
			progressBar.setPrefWidth(200);
			
			btnStart.setOnAction(e -> {
				
				new Thread(() -> {
					for (double i = 0; i <= 1; i += 0.01)
					{
						progressBar.setProgress(i);
						
						try 
						{
							Thread.sleep(100);
						} 
						catch (InterruptedException e1) 
						{
					
							e1.printStackTrace();
						}
					}
				}).start();
				
			});
			
			vBox.getChildren().addAll(progressBar, btnStart);
			 
			pane.setCenter(vBox);

			Scene scene = new Scene(pane, 350, 300);
			primaryStage.setScene(scene);
			primaryStage.setTitle("My GUI");
			primaryStage.show();
		}

		public static void main(String[] args) {
			launch(args);
		}
}
```

Although the above program works but it is a good idea to explicity take care of the new thread so that it can talk to the main method and proper synchronization of threads is achieved. 

**JavaFX UI can only be updated from the JavaFX Application Thread. All JavaFX UI updates must happen on the JavaFX Application Thread.**

Below is a better way of achieving the same results:

```java
package com.cmu;

import javafx.application.Application;
import javafx.application.Platform;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.ProgressBar;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.VBox;
import javafx.geometry.Pos;
import javafx.stage.Stage;

public class MyGUI extends Application {

			@Override
			public void start(Stage primaryStage) {

				BorderPane pane = new BorderPane();
				VBox vBox = new VBox(10);
				vBox.setAlignment(Pos.CENTER);

				Button btnStart = new Button("Start");
				ProgressBar progressBar = new ProgressBar(0);
				progressBar.setPrefWidth(200);
				
				btnStart.setOnAction(e -> {
					new Thread(() -> {
						for (double i = 0; i <= 1; i += 0.01)
						{
							final double progress = i;
							Platform.runLater(() -> progressBar.setProgress(progress));
							
							try 
							{
								Thread.sleep(100);
							} 
							catch (InterruptedException e1) 
							{
						
								e1.printStackTrace();
							}
						}
					}).start();
					
				});
				
				vBox.getChildren().addAll(progressBar, btnStart);
				 
				pane.setCenter(vBox);

				Scene scene = new Scene(pane, 350, 300);
				primaryStage.setScene(scene);
				primaryStage.setTitle("My GUI");
				primaryStage.show();
			}

			public static void main(String[] args) {
				launch(args);
			}
		}
```

`Platform.runLater()` method is used to schedule the code that updates the ProgressBar (`progressBar.setProgress(progress)`) to run on the JavaFX Application Thread. Without this, the UI update will not happen because it was being executed on a background thread.

We can change the properties of the ProgressBar as well

```java
progressBar.setStyle("-fx-accent:green;");
```

We can add a label to show the progress of the progress bar as below:

```java
@Override
public void start(Stage primaryStage) {

			BorderPane pane = new BorderPane();
			VBox vBox = new VBox(10);
			vBox.setAlignment(Pos.CENTER);

			Label lblProgress = new Label("0%");
			Button btnStart = new Button("Start");
			ProgressBar progressBar = new ProgressBar(0);
			progressBar.setPrefWidth(200);
			progressBar.setStyle("-fx-accent: orange; ");
			
			btnStart.setOnAction(e -> {
				
				new Thread(() -> {
					for (double i = 0; i <= 1; i += 0.01)
					{
						final double progress = i;
						Platform.runLater(() -> {
							progressBar.setProgress(progress);
							lblProgress.setText(String.format("%.0f %%", progress * 100));
							});
						
						
						try 
						{
							Thread.sleep(100);
						} 
						catch (InterruptedException e1) 
						{
					
							e1.printStackTrace();
						}

					}
				}).start();	
			});
			
			vBox.getChildren().addAll(progressBar, lblProgress, btnStart);
			 
			pane.setCenter(vBox);

			Scene scene = new Scene(pane, 350, 300);
			primaryStage.setScene(scene);
			primaryStage.setTitle("My GUI");
			primaryStage.show();
}
```

Let's now try to put the label inside the progressBar:
```java
@Override
public void start(Stage primaryStage) {

			BorderPane pane = new BorderPane();
			VBox vBox = new VBox(10);
			vBox.setAlignment(Pos.CENTER);

			Label lblProgress = new Label("0 %");
			lblProgress.setStyle("-fx-font-size: 11px;");
			
			Button btnStart = new Button("Start");
			ProgressBar progressBar = new ProgressBar(0);
			progressBar.setPrefWidth(200);
			progressBar.setStyle("-fx-accent: orange; ");
			
		    StackPane stackPane = new StackPane();
		    stackPane.getChildren().addAll(progressBar, lblProgress);
			
			btnStart.setOnAction(e -> {
				
				new Thread(() -> {
					for (double i = 0; i <= 1; i += 0.01)
					{
						final double progress = i;
						Platform.runLater(() -> {
							progressBar.setProgress(progress);
							lblProgress.setText(String.format("%.0f %%", progress * 100));
							});
						
						
						try 
						{
							Thread.sleep(100);
						} 
						catch (InterruptedException e1) 
						{
					
							e1.printStackTrace();
						}
						
					
					}
				}).start();
				
			});
			
			vBox.getChildren().addAll(stackPane, btnStart);
			 
			pane.setCenter(vBox);

			Scene scene = new Scene(pane, 350, 300);
			primaryStage.setScene(scene);
			primaryStage.setTitle("My GUI");
			primaryStage.show();
}
```

## Progress Indicator (Circular Progress Bar)
JavaFX allows using circular progress bars through ProgressIndicator:

```java
package com.cmu;

import javafx.application.Application;
import javafx.application.Platform;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.ProgressIndicator;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class MyGUI extends Application {

		@Override
		public void start(Stage primaryStage) {

			BorderPane pane = new BorderPane();
			VBox vBox = new VBox(20);
			vBox.setAlignment(Pos.CENTER);

			Button btnStart = new Button("Start");
			ProgressIndicator progressBar = new ProgressIndicator();
			progressBar.setPrefSize(100, 100);
			progressBar.setMaxSize(100, 100);
			progressBar.setMinSize(100, 100);
			progressBar.setStyle("-fx-accent: orange;");
			progressBar.setProgress(0);
			

			btnStart.setOnAction(e -> {

				new Thread(() -> {
					for (double i = 0; i <= 1; i += 0.1) {
						final double progress = i;
						Platform.runLater(() -> {
							progressBar.setProgress(progress);

						});

						try {
							Thread.sleep(100);
						} catch (InterruptedException e1) {

							e1.printStackTrace();
						}

					}
				}).start();

			});

			vBox.getChildren().addAll(progressBar, btnStart);

			pane.setCenter(vBox);

			Scene scene = new Scene(pane, 350, 300);
			primaryStage.setScene(scene);
			primaryStage.setTitle("My GUI");
			primaryStage.show();
		}

		public static void main(String[] args) {
			launch(args);
		}
}
```
External CSS files can be used to style the controls in FX. Add the following style.css file to the package where MyGUI exists:

`style.css`
```css
.progress-indicator .percentage 
{
		-fx-fill: red;
		-fx-font-size: 14px;
		-fx-translate-y: 5px; 
}

.progress-indicator 
{
		-fx-progress-color: red;
		-fx-background-color: transparent;
}
```

`MyGUI.java`
```java
package com.cmu;

import javafx.application.Application;
import javafx.application.Platform;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.ProgressIndicator;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class MyGUI extends Application {

		@Override
		public void start(Stage primaryStage) {

			BorderPane pane = new BorderPane();
			VBox vBox = new VBox(20);
			vBox.setAlignment(Pos.CENTER);

			Button btnStart = new Button("Start");
			ProgressIndicator progressBar = new ProgressIndicator();
			progressBar.setPrefSize(100, 100);
			progressBar.setMaxSize(100, 100);
			progressBar.setMinSize(100, 100);
			progressBar.setProgress(0);
			
			vBox.getChildren().addAll(progressBar, btnStart);

			pane.setCenter(vBox);

			Scene scene = new Scene(pane, 350, 300);
			scene.getStylesheets().add(getClass().getResource("style.css").toExternalForm());

			btnStart.setOnAction(e -> {

				new Thread(() -> {
					for (double i = 0; i <= 1; i += 0.1) {
						final double progress = i;
						Platform.runLater(() -> {
							progressBar.setProgress(progress);

						});

						try {
							Thread.sleep(100);
						} catch (InterruptedException e1) {

							e1.printStackTrace();
						}

					}
				}).start();

			});


			primaryStage.setScene(scene);
			primaryStage.setTitle("My GUI");
			primaryStage.show();
		}

		public static void main(String[] args) {
			launch(args);
		}
}
```
