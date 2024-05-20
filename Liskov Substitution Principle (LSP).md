# Liskov Substitution Principle (LSP) in Java

The Liskov Substitution Principle (LSP) is one of the five SOLID principles of object-oriented design. It states that objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program. In other words, if class `S` is a subclass of class `T`, then objects of type `T` should be replaceable with objects of type `S` without altering the desirable properties of the program (e.g., correctness).

## Key Points

1. **Subtypes must be substitutable for their base types.**
2. **A subclass should override the parent class methods in a way that does not break functionality from a client’s point of view.**
3. **Subclasses should enhance, rather than weaken, the behavior of the parent class.**

# Pros and Cons of the Liskov Substitution Principle (LSP)

## Pros

1. **Improved Code Reusability:**
   - Adhering to LSP ensures that subclasses can be used interchangeably with their parent classes, promoting code reuse across different parts of an application.

2. **Enhanced Maintainability:**
   - Following LSP leads to a well-structured codebase where changes in subclass implementations do not affect the correctness of the parent class, making the code easier to maintain.

3. **Increased Reliability:**
   - By ensuring that subclasses extend the functionality of parent classes without altering expected behavior, LSP improves the reliability of the system.

4. **Simplified Testing:**
   - Tests written for parent class behavior are automatically valid for subclasses, reducing the need for duplicate tests and ensuring consistency in behavior.

5. **Clearer Abstractions:**
   - Adhering to LSP helps in creating clearer and more meaningful abstractions, which is fundamental for designing robust object-oriented systems.

## Cons

1. **Increased Design Complexity:**
   - Ensuring that all subclasses adhere to LSP can increase the complexity of the design process, requiring careful planning and consideration of class hierarchies.

2. **Potential for Over-Engineering:**
   - Strictly following LSP might lead to over-engineering, where developers create unnecessary abstractions or interfaces to ensure substitutability, even when simpler solutions could suffice.

3. **Inheritance Constraints:**
   - LSP can impose constraints on inheritance, making it challenging to create subclasses that need to diverge significantly in behavior from their parent classes without violating the principle.

4. **Possible Performance Overheads:**
   - Ensuring LSP compliance may introduce additional layers of abstraction or indirection, potentially leading to performance overheads in some scenarios.

5. **Difficulty in Refactoring:**
   - Refactoring existing code to comply with LSP can be challenging, especially in large codebases where initial designs did not consider this principle.

## Liskov Substitution Principle (LSP) Examples :

## Bad Example

Here’s a bad example that violates the Liskov Substitution Principle:

```java
class Rectangle {
    protected int width;
    protected int height;

    public void setWidth(int width) {
        this.width = width;
    }

    public void setHeight(int height) {
        this.height = height;
    }

    public int getArea() {
        return width * height;
    }
}

class Square extends Rectangle {
    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width;
    }

    @Override
    public void setHeight(int height) {
        this.height = height;
        this.width = height;
    }
}

public class Main {
    public static void main(String[] args) {
        Rectangle rectangle = new Square();
        rectangle.setWidth(5);
        rectangle.setHeight(10);
        System.out.println(rectangle.getArea()); // Output should be 50 but will be 100
    }
}
```

## Why It's Bad ?
### Description

1. In this bad example, we have a `Rectangle` class with methods to set its width and height, and a `getArea` method to calculate the area.
2. We also have a `Square` class that extends `Rectangle`. However, the `Square` class overrides the `setWidth` and `setHeight` methods to maintain the invariant that all sides of a square are equal.
3. **Violation of LSP:** The `Square` class changes the expected behavior of the `Rectangle` class. When an instance of `Square` is used in place of a `Rectangle`, the behavior of `setWidth` and `setHeight` methods lead to incorrect area calculations.
4. **Unexpected Behavior:** When we set the width and height of a `Square` object through a `Rectangle` reference, it results in an unexpected area value, violating the principle that a subclass should be able to replace its superclass without altering the correctness of the program.

## Good Example : 

```java
abstract class Shape {
    public abstract int getArea();
}

class Rectangle extends Shape {
    protected int width;
    protected int height;

    public void setWidth(int width) {
        this.width = width;
    }

    public void setHeight(int height) {
        this.height = height;
    }

    @Override
    public int getArea() {
        return width * height;
    }
}

class Square extends Shape {
    private int side;

    public void setSide(int side) {
        this.side = side;
    }

    @Override
    public int getArea() {
        return side * side;
    }
}

public class Main {
    public static void main(String[] args) {
        Shape rectangle = new Rectangle();
        ((Rectangle) rectangle).setWidth(5);
        ((Rectangle) rectangle).setHeight(10);
        System.out.println(rectangle.getArea()); // Output: 50

        Shape square = new Square();
        ((Square) square).setSide(5);
        System.out.println(square.getArea()); // Output: 25
    }
}
```
### Why It's Good
### Description

1. In this good example, we create an abstract `Shape` class with an abstract `getArea` method.
2. We then create `Rectangle` and `Square` classes that extend `Shape`.
   - **Rectangle:** Has methods to set its width and height, and calculates the area as width * height.
   - **Square:** Has a method to set the length of its side, and calculates the area as side * side.
3. **Adherence to LSP:** Both `Rectangle` and `Square` can be used interchangeably as `Shape` objects without causing unexpected behavior or incorrect results. Each subclass correctly implements the `getArea` method based on its own properties.
4. **Consistent Behavior:** When substituting `Rectangle` and `Square` for `Shape`, the behavior remains consistent with the expectations for each specific shape, adhering to the Liskov Substitution Principle.


## Conclusion

The Liskov Substitution Principle ensures that a subclass can stand in for its superclass without causing errors or unexpected behavior. By designing subclasses to adhere to this principle, you create more robust and maintainable code.

