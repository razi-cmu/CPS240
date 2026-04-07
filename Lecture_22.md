# FX Collections II

## TableView
A TableView is a UI component in JavaFX that displays data in a tabular format. The TableView control is designed to visualize an unlimited number of rows of data, broken out into columns. A TableView is therefore very similar to the ListView control, with the addition of support for columns.

The TableView control has a number of features, including:
- Powerful TableColumn API:
	- Support for cell factories to easily customize cell contents in both rendering and editing states.
	- Specification of minWidth/ prefWidth/ maxWidth, and also fixed width columns.
	- Width resizing by the user at runtime.
	- Column reordering by the user at runtime.
	- Built-in support for column nesting
- Different resizing policies to dictate what happens when the user resizes columns.
- Support for multiple column sorting by clicking the column header (hold down Shift keyboard key whilst clicking on a header to sort by multiple columns).

Note that TableView is intended to be used to visualize data. It is not intended to be used for laying out your user interface. If you want to lay your user interface out in a grid-like fashion, consider the GridPane layout instead.

`Fruit.java`
```java
package com.cmu;

public class Fruit
{
		private String name;
		private String color;
		private int quantity;
		private boolean selected;
		
		public Fruit(String name, String color, int quantity) 
		{
			this.name = name;
			this.color = color;
			this.quantity = quantity;
			this.selected = false;
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
		
		public boolean isSelected() {
			return selected;
		}

		public void setSelected(boolean selected) {
			this.selected = selected;
		}
}	
```

`MyGUI.java`
```java
package com.cmu;

import java.util.ArrayList;
import javafx.application.Application;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
import javafx.scene.Scene;
import javafx.scene.control.TableColumn;
import javafx.scene.control.TableView;
import javafx.scene.control.cell.PropertyValueFactory;
import javafx.scene.layout.BorderPane;
import javafx.stage.Stage;

public class MyGUI extends Application 
{
		@Override
		public void start(Stage primaryStage) throws Exception 
		{

			BorderPane pane = new BorderPane();
			
			TableView<Fruit> tableView = new TableView<>();
			
			TableColumn<Fruit, String> colName = new TableColumn<>("Name");
			colName.setCellValueFactory(new PropertyValueFactory<>("name"));
			
			TableColumn<Fruit, String> colColor = new TableColumn<>("Color");
			colColor.setCellValueFactory(new PropertyValueFactory<>("color"));
			
			TableColumn<Fruit, Integer> colQuantity = new TableColumn<>("Quantity");
			colQuantity.setCellValueFactory(new PropertyValueFactory<>("quantity"));
			
			colName.setPrefWidth(100);
			colColor.setPrefWidth(100);
			colQuantity.setPrefWidth(100);
			
			tableView.getColumns().addAll(colName, colColor, colQuantity);
			
			ObservableList<Fruit> fruits = FXCollections.observableArrayList(
					new Fruit("Apple", "Red", 2),
					new Fruit("Banana", "Yellow", 6),
					new Fruit("Grape", "Green", 10)
					);
			
			tableView.setItems(fruits);
			
			pane.setCenter(tableView);

			Scene scene = new Scene(pane, 300, 200);

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
Create `TableColumn` objects and set their cell value using the `setCellValueFactory()` method. A cell value factory is a function that tells the column how to extract the data from the data model class. 


`TableView` are very handy as they allow editing of its rows as well. We need to set the `setEditable` to `true` and then set the column for editing

```java
@Override
public void start(Stage primaryStage) throws Exception 
{
		BorderPane pane = new BorderPane();
		
		TableView<Fruit> tableView = new TableView<>();
		tableView.setEditable(true);
		
		TableColumn<Fruit, String> colName = new TableColumn<>("Name");
		colName.setCellValueFactory(new PropertyValueFactory<>("name"));
		colName.setCellFactory(TextFieldTableCell.forTableColumn());
		colName.setOnEditCommit(e -> {
			Fruit fruit = e.getRowValue();
			fruit.setName(e.getNewValue());
		});
		
		TableColumn<Fruit, String> colColor = new TableColumn<>("Color");
		colColor.setCellValueFactory(new PropertyValueFactory<>("color"));
		
		TableColumn<Fruit, Integer> colQuantity = new TableColumn<>("Quantity");
		colQuantity.setCellValueFactory(new PropertyValueFactory<>("quantity"));
		
		colName.setPrefWidth(100);
        colColor.setPrefWidth(100);
        colQuantity.setPrefWidth(100);
        	
		tableView.getColumns().addAll(colName, colColor, colQuantity);
		
		ObservableList<Fruit> fruits = FXCollections.observableArrayList(
				new Fruit("Apple", "Red", 2),
				new Fruit("Banana", "Yellow", 6),
				new Fruit("Grape", "Green", 10)
				);
		
		tableView.setItems(fruits);
		
		pane.setCenter(tableView);

		Scene scene = new Scene(pane, 305, 200);

		primaryStage.setTitle("My GUI");
		primaryStage.setScene(scene);
		primaryStage.show();
}
```
Let's perform some more operations on the button clicks on TableView. We'll add a couple of buttons that can help us adding and deleting records from the TableView.

```Java
@Override
public void start(Stage primaryStage) throws Exception 
{

		BorderPane pane = new BorderPane();
		
		TableView<Fruit> tableView = new TableView<>();
		tableView.setEditable(true);
		
		TableColumn<Fruit, String> colName = new TableColumn<>("Name");
		colName.setCellValueFactory(new PropertyValueFactory<>("name"));
		colName.setCellFactory(TextFieldTableCell.forTableColumn());
		colName.setOnEditCommit(e -> {
            Fruit fruit = e.getRowValue();
            String newName = e.getNewValue();
            fruit.setName(newName);
        });
		
		TableColumn<Fruit, String> colColor = new TableColumn<>("Color");
		colColor.setCellValueFactory(new PropertyValueFactory<>("color"));
		colColor.setCellFactory(TextFieldTableCell.forTableColumn());
		colColor.setOnEditCommit(e -> {
            Fruit fruit = e.getRowValue();
            String newColor = e.getNewValue();
            fruit.setColor(newColor);
        });
		
		TableColumn<Fruit, Integer> colQuantity = new TableColumn<>("Quantity");
		colQuantity.setCellValueFactory(new PropertyValueFactory<>("quantity"));
		colQuantity.setCellFactory(TextFieldTableCell.forTableColumn(new IntegerStringConverter()));
		colQuantity.setOnEditCommit(e -> {
            Fruit fruit = e.getRowValue();
            int newQuantity= e.getNewValue();
            fruit.setQuantity(newQuantity);
        });
		
		colName.setPrefWidth(100);
        colColor.setPrefWidth(100);
        colQuantity.setPrefWidth(100);
        	
		tableView.getColumns().addAll(colName, colColor, colQuantity);
		
		ObservableList<Fruit> fruits = FXCollections.observableArrayList(
				new Fruit("Apple", "Red", 2),
				new Fruit("Banana", "Yellow", 6),
				new Fruit("Grape", "Green", 10)
				);
		
		tableView.setItems(fruits);
		
		Button btnAdd = new Button("Add Record");
		Button btnDelete = new Button("Delete Record");
		
		btnAdd.setOnAction(e->{
			if (fruits.size() == 0 || !fruits.get(fruits.size() - 1).getName().isEmpty())
			{
				fruits.add(new Fruit("", "", 0));
			}
			
			tableView.scrollTo(fruits.size() - 1);
			tableView.edit(fruits.size() - 1, colName);
		});
		
		btnDelete.setOnAction(e -> {
			Fruit selectedFruit = tableView.getSelectionModel().getSelectedItem();
			if (selectedFruit != null)
			{
				fruits.remove(selectedFruit);
			}
		});

		HBox buttonPane = new HBox(10);
        buttonPane.getChildren().addAll(btnAdd, btnDelete);
        buttonPane.setAlignment(Pos.CENTER);
		
		pane.setCenter(tableView);
		pane.setBottom(buttonPane);

		Scene scene = new Scene(pane, 305, 300);

		primaryStage.setTitle("My GUI");
		primaryStage.setScene(scene);
		primaryStage.show();
}
```

Individual Cell properties can be changed by setting CellFactory. For now, let's go by the book and use a traditional way of styling. Later, we'll simplify it.

```java
colName.setCellFactory(new Callback<TableColumn<Fruit,String>, TableCell<Fruit,String>>() {
				
				@Override
				public TableCell<Fruit, String> call(TableColumn<Fruit, String> param) {
					// TODO Auto-generated method stub
					return new TableCell<Fruit, String>() {
						protected void updateItem(String item, boolean empty)
						{
							super.updateItem(item, empty);
							setText(item);
							setTextFill(Color.RED);
							setAlignment(Pos.CENTER);
						}
					};
				}
			});
```

`TableView` is pretty flexible when it comes to adding other UI controls to it. Let's add a checkbox to one of the columns of the TableView:

`Fruit.java`
```java
package com.cmu;

public class Fruit
{
		private String name;
		private String color;
		private int quantity;
		private boolean selected;
		
		public Fruit(String name, String color, int quantity) 
		{
			this.name = name;
			this.color = color;
			this.quantity = quantity;
			this.selected = false;
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
		
		public boolean isSelected() {
			return selected;
		}

		public void setSelected(boolean selected) {
			this.selected = selected;
		}
}
	
```
`MyGUI.java`
```java
package com.cmu;

import java.awt.Checkbox;

import javafx.application.Application;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.control.CheckBox;
import javafx.scene.control.TableCell;
import javafx.scene.control.TableColumn;
import javafx.scene.control.TableView;
import javafx.scene.control.cell.PropertyValueFactory;
import javafx.scene.control.cell.TextFieldTableCell;
import javafx.scene.layout.BorderPane;
import javafx.scene.layout.HBox;
import javafx.scene.paint.Color;
import javafx.stage.Stage;
import javafx.util.Callback;
import javafx.util.converter.IntegerStringConverter;

public class MyGUI extends Application 
{
		@Override
		public void start(Stage primaryStage) throws Exception 
		{

			BorderPane pane = new BorderPane();
			
			TableView<Fruit> tableView = new TableView<>();
			tableView.setEditable(true);
			
			TableColumn<Fruit, String> colName = new TableColumn<>("Name");
			colName.setCellValueFactory(new PropertyValueFactory<>("name"));
			colName.setCellFactory(TextFieldTableCell.forTableColumn());
			colName.setOnEditCommit(e -> {
				Fruit fruit = e.getRowValue();
				String newName = e.getNewValue();
				fruit.setName(newName);
			});
			colName.setCellFactory(new Callback<TableColumn<Fruit,String>, TableCell<Fruit,String>>() {
				
				@Override
				public TableCell<Fruit, String> call(TableColumn<Fruit, String> param) {
					// TODO Auto-generated method stub
					return new TableCell<Fruit, String>() {
						protected void updateItem(String item, boolean empty)
						{
							super.updateItem(item, empty);
							setText(item);
							setTextFill(Color.RED);
							setAlignment(Pos.CENTER);
						}
					};
				}
			});
			
			TableColumn<Fruit, String> colColor = new TableColumn<>("Color");
			colColor.setCellValueFactory(new PropertyValueFactory<>("color"));
			colColor.setCellFactory(TextFieldTableCell.forTableColumn());
			colColor.setOnEditCommit(e -> {
				Fruit fruit = e.getRowValue();
				String newColor = e.getNewValue();
				fruit.setColor(newColor);
			});
			
			TableColumn<Fruit, Integer> colQuantity = new TableColumn<>("Quantity");
			colQuantity.setCellValueFactory(new PropertyValueFactory<>("quantity"));
			colQuantity.setCellFactory(TextFieldTableCell.forTableColumn(new IntegerStringConverter()));
			colQuantity.setOnEditCommit(e -> {
				Fruit fruit = e.getRowValue();
				int newQuantity= e.getNewValue();
				fruit.setQuantity(newQuantity);
			});
			
			TableColumn<Fruit, Boolean> colSelect = new TableColumn<>("Select");
			colSelect.setCellValueFactory(new PropertyValueFactory<>("selected"));
			colSelect.setCellFactory(new Callback<TableColumn<Fruit,Boolean>, TableCell<Fruit,Boolean>>() {
				
				@Override
				public TableCell<Fruit, Boolean> call(TableColumn<Fruit, Boolean> param) {
					// TODO Auto-generated method stub
					return new TableCell<Fruit, Boolean>() {
						private CheckBox checkBox = new CheckBox();
						
						protected void updateItem(Boolean item, boolean empty)
						{
							if (empty || item == null) {
								setGraphic(null);
							} else {
								checkBox.setSelected(item);
								checkBox.setOnAction(event -> {
									Fruit fruit = getTableView().getItems().get(getIndex());
									fruit.setSelected(checkBox.isSelected());
								});
								setGraphic(checkBox);
							}
						}
					};
				}
			});
			
			colName.setPrefWidth(100);
			colColor.setPrefWidth(100);
			colQuantity.setPrefWidth(100);
				
			tableView.getColumns().addAll(colName, colColor, colQuantity, colSelect);
			
			ObservableList<Fruit> fruits = FXCollections.observableArrayList(
					new Fruit("Apple", "Red", 2),
					new Fruit("Banana", "Yellow", 6),
					new Fruit("Grape", "Green", 10)
					);
			
			tableView.setItems(fruits);
			
			Button btnAdd = new Button("Add Record");
			Button btnDelete = new Button("Delete Record");
			
			btnAdd.setOnAction(e->{
				if (fruits.size() == 0 || !fruits.get(fruits.size() - 1).getName().isEmpty())
				{
					fruits.add(new Fruit("", "", 0));
				}
				
				tableView.scrollTo(fruits.size() - 1);
				tableView.edit(fruits.size() - 1, colName);
			});
			
			btnDelete.setOnAction(e -> {
				Fruit selectedFruit = tableView.getSelectionModel().getSelectedItem();
				if (selectedFruit != null)
				{
					fruits.remove(selectedFruit);
				}
			});
				
			HBox buttonPane = new HBox(10);
			buttonPane.getChildren().addAll(btnAdd, btnDelete);
			buttonPane.setAlignment(Pos.CENTER);
			
			pane.setCenter(tableView);
			pane.setBottom(buttonPane);

			Scene scene = new Scene(pane, 305, 300);

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

There is a still issue in the above program. When we try to add a new record by clicking on Add Record button, it might not work as expected. The reason for this is we have two `setCellFactory` methods in place. We should use only one of them at a time. Since we are customizing our cells, we do not need `colName.setCellFactory(TextFieldTableCell.forTableColumn());`, so it is safe to remove that. Instead, we can use the one that customizes the cells. We can simplify it like below:

`Driver.java`
```java
package com.cmu.test;


import javafx.application.Application;
import javafx.collections.*;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.control.cell.PropertyValueFactory;
import javafx.scene.control.cell.TextFieldTableCell;
import javafx.scene.layout.*;
import javafx.stage.Stage;
import javafx.util.converter.DefaultStringConverter;
import javafx.util.Callback;
import java.util.*;

public class Driver extends Application {
    @Override
    public void start(Stage stage) 
    {
    	BorderPane pane = new BorderPane();
    	
    	TableView<Fruit> tableView = new TableView<>();
    	tableView.setEditable(true);
    	
    	TableColumn<Fruit, String> colName = new TableColumn<Fruit, String>("Name");
    	colName.setCellValueFactory(new PropertyValueFactory<>("name"));
    	colName.setOnEditCommit(e -> {
    		Fruit fruit = e.getRowValue();
    		String newName = e.getNewValue();
    		fruit.setName(newName);
    	});
    	colName.setCellFactory(column -> {
    	    TextFieldTableCell<Fruit, String> cell = new TextFieldTableCell<>(new DefaultStringConverter());

    	    cell.setStyle("-fx-text-fill: red;");

    	    return cell;
    	});
    	
    	TableColumn<Fruit, String> colColor = new TableColumn<>("Color");
    	colColor.setCellValueFactory(new PropertyValueFactory<>("color"));
    	colColor.setCellFactory(column -> {
    	    TextFieldTableCell<Fruit, String> cell = new TextFieldTableCell<>(new DefaultStringConverter());

    	    cell.setStyle("-fx-text-fill: red;");

    	    return cell;
    	});
    	colColor.setOnEditCommit(e -> {
    		Fruit fruit = e.getRowValue();
    		String newColor = e.getNewValue();
    		fruit.setColor(newColor);
    	});
    	
    	TableColumn<Fruit, Integer> colQuantity = new TableColumn<>("Quantity");
    	colQuantity.setCellValueFactory(new PropertyValueFactory<>("quantity"));
    	colQuantity.setOnEditCommit(e -> {
    		Fruit fruit = e.getRowValue();
    		int newQuantity = e.getNewValue();
    		fruit.setQuantity(newQuantity);
    	});
    	
    	TableColumn<Fruit, Boolean> colSelect = new TableColumn<Fruit, Boolean>("Select");
    	colSelect.setCellValueFactory(new PropertyValueFactory<>("selected"));
    	colSelect.setCellFactory(new Callback<TableColumn<Fruit, Boolean>, TableCell<Fruit, Boolean>>(){

			@Override
			public TableCell<Fruit, Boolean> call(TableColumn<Fruit, Boolean> param) {
				// TODO Auto-generated method stub
				return new TableCell<Fruit, Boolean>(){
					protected void updateItem(Boolean item, boolean empty)
					{
						if (empty || item == null)
						{
							setGraphic(null);
						}
						else
						{
							CheckBox checkBox = new CheckBox();
							checkBox.setSelected(item);
							checkBox.setOnAction(event -> {
								Fruit fruit = getTableView().getItems().get(getIndex());
								fruit.setSelected(checkBox.isSelected());
							});
							setGraphic(checkBox);
						}
					}
				};
			}
    		
    	});
    	
    	colName.setPrefWidth(200);
    	colColor.setPrefWidth(150);
    	colQuantity.setPrefWidth(140);
    
    	tableView.getColumns().addAll(Arrays.asList(colName, colColor, colQuantity, colSelect));
    	
    	ObservableList<Fruit> fruits = FXCollections.observableArrayList(
    			new Fruit("Apple", "Red", 2),
    			new Fruit("Banana", "Yellow", 6),
    			new Fruit("Orange", "Orange", 5),
    			new Fruit("Melon", "Green", 1)
    			);
    	
    	tableView.setItems(fruits);
    	
    	Button btnAdd = new Button("Add Record");
    	Button btnDelete = new Button("Delete Record");
    	
    	btnAdd.setOnAction(e -> {
    		if (fruits.size() == 0 || !fruits.get(fruits.size() - 1).getName().isEmpty())
    		{
    			fruits.add(new Fruit("", "", 0));
    		}
    		tableView.scrollTo(fruits.size() - 1);
    		tableView.edit(fruits.size() - 1, colName);

    	});
    	
    	btnDelete.setOnAction(e -> {
    		Fruit fruit = tableView.getSelectionModel().getSelectedItem();
    		if (fruit != null)
    		{
    			fruits.remove(fruit);
    		}
    	});
    	
    	HBox hBox = new HBox(10);
    	hBox.getChildren().addAll(btnAdd, btnDelete);
    	hBox.setAlignment(Pos.CENTER);
    	hBox.setPadding(new Insets(10));
    	
    	pane.setCenter(tableView);
    	pane.setBottom(hBox);
      
    	Scene scene = new Scene(pane, 550, 550);

        stage.setScene(scene);
        stage.setTitle("Fruits");
        stage.show();
    }

    public static void main(String[] args) {
        launch();
    }
}
```

In the above code, quantity column is not editable, which you should try on your own.

## References
- Oracle Documentation
	- https://docs.oracle.com/javase/8/javafx/api/javafx/scene/control/TableView.html

- https://www.tutorialspoint.com/javafx/javafx_tableview.htm

