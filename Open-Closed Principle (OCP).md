# Open/Closed Principle in SOLID with Java

The Open/Closed Principle is one of the SOLID principles of object-oriented design, which provides guidelines for creating software that is easy to maintain and extend. The principle states that:

**Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification.**

This means that the behavior of a module can be extended without modifying its source code. Let's see how this principle can be applied in Java with an example.

# Pros of the Open/Closed Principle

## Enhanced Maintainability
- By ensuring that existing code does not need to be modified when new features are added, maintenance becomes easier and less error-prone.
- Reduces the risk of introducing bugs into existing, working code.

## Improved Extensibility
- The design is inherently more flexible, allowing for easy addition of new functionality.
- Encourages modular design where new components can be added without impacting existing components.

## Code Reusability
- Promotes the creation of reusable components that can be easily integrated into different parts of the application or even across different projects.
- Facilitates a more modular approach, enabling the reuse of common behaviors.

## Separation of Concerns
- Encourages developers to separate different behaviors and logic into distinct, independent classes.
- Leads to a clearer, more understandable code structure.

## Scalability
- Makes the application more scalable by allowing new features to be added with minimal changes to the codebase.
- Supports a growing codebase with less effort and lower complexity.

# Helps Avoid

## Modification-Related Bugs
- Prevents the introduction of new bugs when modifying existing code to add new features.
- Reduces the chances of breaking existing functionality.

## Code Entanglement
- Avoids tightly coupled code, where changes in one part of the system necessitate changes in another.
- Ensures that new functionality does not interfere with or degrade the performance of existing code.

## Technical Debt
- By encouraging proper extension methods, it reduces the accumulation of technical debt over time.
- Helps maintain clean and manageable code.

## Difficulty in Testing
- Makes unit testing easier since each class has a single responsibility and can be tested independently.
- Facilitates mocking and testing of individual components without needing to understand the entire system.

## Complex Refactoring
- Reduces the need for extensive refactoring by providing a clear path for extending functionality.
- Minimizes the need for large-scale changes when adding new features.

# Summary

The Open/Closed Principle is a key concept in object-oriented design that greatly enhances the flexibility, maintainability, and scalability of software systems. By following this principle, developers can create systems that are easier to extend with new functionality while minimizing the risk of introducing new bugs and technical debt. This leads to cleaner, more modular, and more understandable code, which in turn supports more efficient and effective development and maintenance practices.


<br>  

In the bad example, the main class has all the implementations for different shapes. This means every time we add a new shape, we have to modify the main class to include the new shape's area calculation.

In the good example, we use an abstract class (or interface) to define a common method, and each shape class implements this method with its own logic. This way, the main class just calls the method without knowing the details, and it does not need to be modified when a new shape is added.

## Example: Shape Area Calculation

Suppose we have a basic application that calculates the area of different shapes. Initially, we might start with a simple design where we have a class `Shape` and subclasses for each specific shape.

### Initial Design (Violating OCP)

```java
// Base class for shapes
abstract class Shape {
    // No implementation needed here for the bad example
}

// Circle subclass
class Circle extends Shape {
    double radius;

    Circle(double radius) {
        this.radius = radius;
    }
}

// Rectangle subclass
class Rectangle extends Shape {
    double width;
    double height;

    Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
}

// Triangle subclass
class Triangle extends Shape {
    double base;
    double height;

    Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }
}

/**
 * AreaCalculator class that calculates the area of shapes.
 * 
 * In the current design, whenever we add a new shape,
 * we need to modify this class to include the new shape's area calculation.
 * 
 * The best practice is to make the calculateArea method abstract in the Shape class.
 * Each subclass of Shape should implement this method with its own logic.
 * 
 * This approach allows us to add new shapes without modifying the AreaCalculator class,
 * adhering to the Open/Closed Principle.
 */

//here as bad example whenever I add shape I should come to this class to add it in the if else 
class AreaCalculator {
    public double calculateTotalArea(List<Shape> shapes) {
        double totalArea = 0;
        for (Shape shape : shapes) {
            if (shape instanceof Circle) {
                Circle circle = (Circle) shape;
                totalArea += Math.PI * circle.radius * circle.radius;
            } else if (shape instanceof Rectangle) {
                Rectangle rectangle = (Rectangle) shape;
                totalArea += rectangle.width * rectangle.height;
            } else if (shape instanceof Triangle) {
                Triangle triangle = (Triangle) shape;
                totalArea += 0.5 * triangle.base * triangle.height;
            }
        }
        return totalArea;
    }
}

```
In this design, the AreaCalculator class uses the Shape class and its subclasses to calculate the total area of a list of shapes. However, if we want to add a new shape, say a Triangle, we need to modify the AreaCalculator class to handle this new shape, thus violating the Open/Closed Principle.
## Improved Design (Adhering to OCP)
To adhere to the Open/Closed Principle, we can make use of polymorphism and ensure that new shapes can be added without modifying the existing code.

```java
// Base class for shapes (same as before)
abstract class Shape {
    abstract double calculateArea();
}

// Circle subclass (same as before)
class Circle extends Shape {
    private double radius;

    Circle(double radius) {
        this.radius = radius;
    }

    @Override
    double calculateArea() {
        return Math.PI * radius * radius;
    }
}

// Rectangle subclass (same as before)
class Rectangle extends Shape {
    private double width;
    private double height;

    Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    double calculateArea() {
        return width * height;
    }
}

// New Triangle subclass
class Triangle extends Shape {
    private double base;
    private double height;

    Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }

    @Override
    double calculateArea() {
        return 0.5 * base * height;
    }
}

// AreaCalculator class that calculates the area of shapes (same as before)
class AreaCalculator {
    public double calculateTotalArea(List<Shape> shapes) {
        double totalArea = 0;
        for (Shape shape : shapes) {
            totalArea += shape.calculateArea();
        }
        return totalArea;
    }
}
```

In this improved design:

Base Class: The Shape class remains unchanged.
Subclasses: Each shape subclass (like Circle, Rectangle, and Triangle) implements the calculateArea method.
AreaCalculator Class: The AreaCalculator class does not need any modification to accommodate new shapes. New shapes can be added by simply creating new subclasses of Shape.
This way, the system is open for extension (we can add new shapes) but closed for modification (we do not need to change existing code to add new shapes), adhering to the Open/Closed Principle.

