# Overview of Java (Review CPS 180)

## The Java Language
Java is a powerful and versatile programming language for developing software running on mobile devices, desktop computers, and servers. Java is a full-featured, general-purpose programming language that can be used to develop robust mission-critical applications.  Most major companies use Java in some applications. Most server-side applications were developed using Java. Java was used to develop the code to communicate with and control the robotic rover on Mars. The software for Android cell phones is developed using Java. <br>
Java is a full-fledged and powerful language that can be used in many ways. It comes in three editions:  
- Java Standard Edition (Java SE) to develop client-side applications. The applications can run on desktop.
- Java Enterprise Edition (Java EE) to develop server-side applications, such as Java servlets, JavaServer Pages (JSP), and JavaServer Faces (JSF).
- Java Micro Edition (Java ME) to develop applications for mobile devices, such as cell phones.

Java SE is the foundation upon which all other Java technology is based. Oracle releases each version with a Java Development Toolkit (JDK). The JDK consists of a set of separate programs, each invoked from a command line, for compiling, running, and testing Java programs. The program for running Java programs is known as Java Runtime Environment (JRE). Instead of using the JDK, you can use a Java development tool (e.g., NetBeans, Eclipse, and TextPad), software that provides an integrated development environment (IDE) for developing Java programs quickly

## A Simple Java Program
Java uses classes to create a program. The program is executed from the `main` method. A class may contain several methods. The `main` method is the entry point where the program begins execution.
```java
public class HelloWorld
{
    public static void main(String[] args)
    {
        System.out.println("Hello World");
    }
}
```
In order to show something on the console, a static object `System.out` is used with functions like `print()` and `println()`
```java
public class HelloWorld
{
    public static void main(String[] args)
    {
        System.out.print("Hello World");
        System.out.print("Central Michigan University");
    }
}
```
Keywords have a specific meaning to the compiler and cannot be used for other purposes in the program. For example, when the compiler sees the word `class`, it understands that the word after `class` is the name for the class. Other keywords in this program are `public`, `static`, and `void`. <br>
The most common errors you will make as you learn to program will be syntax errors. Like any programming language, Java has its own syntax, and you need to write code that conforms to the syntax rules. If your program violates a rule, for example, if a semicolon is missing, a brace is missing, a quotation mark is missing, or a word is misspelled, the Java compiler will report syntax errors.

## Compiling and Executing a Java Program
You have to create your program and compile it before it can be executed. If your program has compile errors, you have to modify the program to fix them, then recompile it. If your program has runtime errors or does not produce the correct result, you have to modify the program, recompile it, and execute it again. <br>
A Java compiler translates a Java source file into a Java bytecode file. The following command compiles `HelloWorld.java`. <br>
```java
javac HelloWorld.java
```

If there aren’t any syntax errors, the compiler generates a bytecode file with a `.class` extension. The Java language is a high-level language, but Java bytecode is a low-level language. The bytecode is similar to machine instructions but is architecture neutral and can run on any platform that has a Java Virtual Machine (JVM). You can execute the bytecode on any platform with a JVM, which is an interpreter. It translates the individual instructions in the bytecode into the target machine language code one at a time, rather than the whole program as a single unit. Each step is executed immediately after it is translated. You can execute the bytcode file using `java` command:
```java
java HelloWorld

```

## Programming Style and Documentation
Good programming style and appropriate documentation reduce the chance of errors and make programs easy to read. In a long program, you should also include comments that introduce each major step and explain anything that is difficult to read. <br>
In addition to line comments (beginning with `//`) and block comments (beginning with `/*`), Java supports comments of a special type, referred to as javadoc comments. javadoc comments begin with `/**` and end with `*/`. They can be extracted into an HTML file using the JDK’s javadoc command.

```java
public class HelloWorld
{
    // this is a comment.
    public static void main(String[] args)
    {
        System.out.println("Hello World");
    }
}
```

## Variables
Variables are for representing data of a certain type. To use a variable, you declare it by telling the compiler its name as well as what type of data it can store. The variable declaration tells the compiler to allocate appropriate memory space for the variable based on its data type.

## Java Data Types
Java uses various data types. Java uses four types for integers: `byte`, `short`, `int` and `long`. Java uses two types for floating-point numbers: `float` and `double`. A `String` which is a class in Java is also used as a data type but technically it is a class rather than a data type. The `boolean` data type declares a variable with the value either `true` or `false`. 
```java
public class Student {
    
    public static void main(String[] args) {
        String name = "Steve Jobs";
        int age = 55;
        float cgpa = 2.95f;
        boolean active = true;
        double height = 5.11;

        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("CGPA: " + cgpa);
        System.out.println("Active: " + active);
        System.out.println("Height: " + height);
    }
}
```
A `char` data type is used to hold a single character.

## Console Printing
`printf` is another way of printing in Java. It is convenient way of providing all the variables in a single print statement.
```java
public class Student {
    
    public static void main(String[] args) {
        String name = "Steve Jobs";
        int age = 55;
        float cgpa = 2.95f;
        boolean active = true;
        double height = 5.11;

        System.out.printf("Name: %s \nAge: %d \nCGPA: %.2f \nActive: %s \nHeight: %.2f", name, age, cgpa, active, height);
    }
}
```
## Instance Variables
Instance members (variables) are used in Java much like member functions. More than 1 can be created. An instance of the class is required to access these variables inside a static method.
```java
public class Student {
    String name = "Steve Jobs";
    int age = 55;
    float cgpa = 2.95f;
    boolean active = true;
    double height = 5.11;

    public static void main(String[] args) {

        Student student = new Student();
        System.out.printf("Name: %s \nAge: %d \nCGPA: %.2f \nActive: %s \nHeight: %.2f", student.name, student.age, student.cgpa, student.active, student.height);
    }
}
```
Member functions can access instance variables without creating an instance of a class. However, to call a member function in a static function, an instance is required for that class.
```java
public class Student {
    String name = "Steve Jobs";
    int age = 55;
    float cgpa = 2.95f;
    boolean active = true;
    double height = 5.11;

    public void display()
    {
        System.out.printf("Name: %s \nAge: %d \nCGPA: %.2f \nActive: %s \nHeight: %.2f", name, age, cgpa, active, height);
    }

    public static void main(String[] args) {
        Student student = new Student();
        student.display();
        
    }
}
```

## Class Level Functions
Word `static` means it is a class level function and not an object/instance level function. So you can access this function without creating an object of a class. You can create more than one static function in a class. <br>

```java
public class HelloWorld
{
    public static void main(String[] args)
    {
        System.out.println("Hello World");
        test();
    }

    public static void test()
    {
        System.out.println("This is another static function");
    }
}
```
## Member Functions
Member functions can also be used in Java that would require you to create an object/instance of a class to access them. These functions cannot be accessed without an object. So the following code will produce an error.
```java
public class Testing {
    public void test()
    {
        System.out.println("This is a member function");
    }
    public static void main(String[] args) {
        test();
    }
}
```
In order to fix the above code, an instance of the class is required. Below is the correct code.
```java
public class Testing {


    public void test()
    {
        System.out.println("This is a member function");
    }
    public static void main(String[] args) {
        Testing testing = new Testing();
        testing.test();
    }
    
}
```
More than 1 member function can be created inside a class
```java
public class Testing {


    public void test()
    {
        System.out.println("This is a member function");
    }

    public void test_2()
    {
        System.out.println("This is another member function");
    }
    public static void main(String[] args) {
        Testing testing = new Testing();
        testing.test();
        testing.test_2();
    }
    
}
```

## Conditional Statements
Java has several types of selection statements: one-way `if` statements, two-way `if-else` statements, nested `if` statements, multi-way `if-else` statements, `switch` statements, and conditional operators. <br>

```java
public class Student {
    String name = "Steve Jobs";
    int age = 55;
    float cgpa = 3.95f;
    boolean active = true;
    double height = 5.11;

    void calculateGrade()
    {
        if (cgpa > 3.5f && cgpa <=4.0f)
        {
            System.out.println("A");
        }
        else if (cgpa > 3.0f && cgpa <=3.5f)
        {
            System.out.println("B");
        }
        else if (cgpa > 2.5f && cgpa <=3.0f)
        {
            System.out.println("C");
        }
        else if (cgpa > 2.0f && cgpa <=2.5f)
        {
            System.out.println("D");
        }
        else
        {
            System.out.println("F");
        }
    }

    public void display()
    {
        System.out.printf("Name: %s \nAge: %d \nCGPA: %.2f \nActive: %s \nHeight: %.2f \n", name, age, cgpa, active, height);
    }

    public static void main(String[] args) {
        Student student = new Student();
        student.display();
        student.calculateGrade();
        
    }
}
```

## switch Statement
A `switch` statement executes statements based on the value of a variable or an expression.
```java
switch(status)
{
    case 0: compute tax for single filer;
            break;
    case 1: compute tax for married filing separately;
            break;
    case 2: compute tax for head of household;
            break;
    default:
            System.out.println("Error: invalid status");
            System.exit(1);
}

```

## Loops
Java provides a powerful construct called a loop that controls how many times an operation or a sequence of operations is performed in succession. Using a loop statement, you can simply tell the computer to display a string a hundred times without having to code the print statement a hundred times. <br>

A `while` loop executes statements repeatedly while the condition is true.
```java
    void progress()
    {
        int i = 1;
        while (i<=8)
        {
            System.out.printf("Semester: %d , CGPA: %.2f ", i, cgpa );
            cgpa = cgpa - 0.25f;
            i++;
        }
    }
```

A `do-while` loop is the same as a `while` loop except that it executes the loop body first then checks the loop continuation condition.
```java
int i = 0;
do 
{
    System.out.println(i);
    i++;
}
while (i < 5);

```

A `for` loop has a concise syntax for writing loops. This loop is intuitive and easy for beginners to grasp.
```java
for (int i = 0; i < 5; i++) 
{
    System.out.println(i);
}

```

## Methods / Functions
Methods can be used to define reusable code and organize and simplify coding, and make code easy to maintain.
```java
void progress()
    {
        int i = 1;
        while (i<=8)
        {
            System.out.printf("Semester: %d , CGPA: %.2f ", i, cgpa );
            cgpa = cgpa - 0.25f;
            i++;
        }
    }
```
## Functions with Return Types
A `void` method does not return a value. Java functions can return results based on various data types.
```java
public class Numbers {

    public static double power(int base, int exponent)
    {
        double result = 1;

        for (int i=0; i<exponent; i++)
        {
            result *= base;
        }

        return result;
    }
    public static void main(String[] args) {
        
        double result = power(5, 2);
        System.out.println("Result: " + result);
    }
}
```
When calling a method, you need to provide arguments, which must be given in the same order as their respective parameters in the method signature. This is known as parameter order association.

## Scope of Variables
The scope of a variable is the part of the program where the variable can be referenced. A `local` variable defined inside a method is referred to as a local variable. The scope of a local variable starts from its declaration and continues to the end of the block that contains the variable. A local variable must be declared and assigned a value before it can be used. A parameter is actually a local variable. The scope of a method parameter covers the entire method.
```java
public class Main {
  public static void main(String[] args) {

    for (int i = 0; i < 5; i++) {
      System.out.println(i); // i is accessible here
    }

    // i is NOT accessible here
  }
}

```

## Casting
Java allows casting from one data type to another. Java allows implicit casting as well as explicit casting.
```java
// Implicit Casting

    public static void casting(int number)
    {
        int i = 10;
        double d = i;
        float f = i;

        System.out.println("Integer : " + i);
        System.out.println("Double : " + d);
        System.out.println("Float : " + f);

    }
```
Narrow casting would result in an error as we lose precision. Below will produce an error
```java
    public static void casting(int number)
    {
        double d = 10.5;
        int i = d;

        System.out.println("Integer : " + i);
        System.out.println("Double : " + d);

    }

// Explicit Casting

public static void casting(int number)
{
        int i = 10;
        double d = (double) i;
        System.out.println("Integer : " + i);
        System.out.println("Double : " + d);

}
```
Narrow casting is allowed using explicit casting
```java
public static void casting(int number)
    {
        double d = 10.5;
        int i = (int) d;
        System.out.println("Integer : " + i);
        System.out.println("Double : " + d);

    }
```
