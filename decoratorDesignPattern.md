# Decorator Design Pattern

The Decorator Design Pattern is a structural design pattern used to dynamically add new functionalities or behaviors to an object at runtime without altering its structure or modifying its original code. It achieves this by wrapping the object in a "decorator" class.

---

## Key Features

1. **Dynamic Behavior Addition**: Add functionality to objects without changing their class.
2. **Open/Closed Principle**: The pattern supports the principle of being open for extension but closed for modification.
3. **Object Composition**: Instead of inheritance, it uses composition to achieve flexibility, as it wraps the base class inside a decorator class rather using inheritance for creating seperate class for creating multiple combination of features.

---

## Components of the Decorator Pattern

1. **Component Interface**:
   - Defines the interface for objects that can have responsibilities added dynamically.

2. **Concrete Component**:
   - The base implementation of the component interface.

3. **Decorator Interface**:
   - Implements the component interface and acts as a wrapper for the base component.
   - Defines methods to add additional behavior.

4. **Concrete Decorators**:
   - Extend the decorator interface and add new behaviors or responsibilities.

---

## How It Works

The decorator object wraps the original object (concrete component) and provides additional functionality by implementing the same interface. Multiple decorators can be applied, allowing for stacking or chaining additional behaviors.

---

## Example in Java

### Scenario: Add functionality to a Shape object (e.g., add colors and borders).

```java
// Component Interface
interface Shape {
    void draw();
}

// Concrete Component
class Circle implements Shape {
    @Override
    public void draw() {
        System.out.println("Drawing a Circle");
    }
}

// Decorator Interface
abstract class ShapeDecorator implements Shape {
    protected Shape decoratedShape;

    public ShapeDecorator(Shape decoratedShape) {
        this.decoratedShape = decoratedShape;
    }

    public void draw() {
        decoratedShape.draw(); // Delegates call to the wrapped object
    }
}

// Concrete Decorators
class RedBorderDecorator extends ShapeDecorator {
    public RedBorderDecorator(Shape decoratedShape) {
        super(decoratedShape);
    }

    @Override
    public void draw() {
        decoratedShape.draw();
        addRedBorder();
    }

    private void addRedBorder() {
        System.out.println("Adding a red border");
    }
}

class DashedBorderDecorator extends ShapeDecorator {
    public DashedBorderDecorator(Shape decoratedShape) {
        super(decoratedShape);
    }

    @Override
    public void draw() {
        decoratedShape.draw();
        addDashedBorder();
    }

    private void addDashedBorder() {
        System.out.println("Adding a dashed border");
    }
}

// Main Class to Demonstrate
public class DecoratorPatternExample {
    public static void main(String[] args) {
        Shape circle = new Circle();

        // Add a red border
        Shape redCircle = new RedBorderDecorator(circle);
        System.out.println("Red Border Circle:");
        redCircle.draw();

        // Add a red and dashed border
        Shape redDashedCircle = new DashedBorderDecorator(redCircle);
        System.out.println("\nRed Dashed Border Circle:");
        redDashedCircle.draw();
    }
}
```

### Output:

```
Red Border Circle:
Drawing a Circle
Adding a red border

Red Dashed Border Circle:
Drawing a Circle
Adding a red border
Adding a dashed border
```

---

## When to Use the Decorator Pattern

1. **Dynamic Behavior Addition**: When you want to add responsibilities dynamically to individual objects, not all objects of a class.
2. **Avoiding Subclass Explosion**: When subclassing to add functionality would result in many subclasses.
3. **Reusable Behaviors**: When you want to combine behaviors flexibly.

---

## Advantages

1. Promotes flexibility by avoiding subclassing.
2. Behaviors can be added or removed at runtime.
3. Supports single responsibility principle by dividing functionality into multiple classes.

---

## Disadvantages

1. Can result in a large number of small classes.
2. Complexity can increase due to the layered structure of decorators.

