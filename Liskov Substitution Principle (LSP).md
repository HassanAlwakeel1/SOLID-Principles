# Liskov Substitution Principle (LSP) in Java

The Liskov Substitution Principle (LSP) is one of the five SOLID principles of object-oriented design. It states that objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program. In other words, if class `S` is a subclass of class `T`, then objects of type `T` should be replaceable with objects of type `S` without altering the desirable properties of the program (e.g., correctness).

## Key Points

1. **Subtypes must be substitutable for their base types.**
2. **A subclass should override the parent class methods in a way that does not break functionality from a client’s point of view.**
3. **Subclasses should enhance, rather than weaken, the behavior of the parent class.**

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
- **Violation of Substitution:** The `Square` class violates LSP because it changes the behavior of `Rectangle`'s `setWidth` and `setHeight` methods.
- **Unexpected Behavior:** Setting the width and height of a `Square` as if it were a `Rectangle` leads to unexpected behavior, as the area calculation will not be as intended for a `Rectangle`.

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

- **Adheres to Substitution:** Both `Rectangle` and `Square` are subclasses of `Shape` and can be used interchangeably without breaking the program.
- **No Unexpected Behavior:** Each subclass implements the `getArea` method correctly according to its own properties, ensuring substitutability without unexpected behavior.

## Conclusion

The Liskov Substitution Principle ensures that a subclass can stand in for its superclass without causing errors or unexpected behavior. By designing subclasses to adhere to this principle, you create more robust and maintainable code.

