# Factory Design Pattern

The **Factory Design Pattern** is a **creational design pattern** that provides an interface for creating objects in a superclass but allows subclasses to alter the type of objects that will be created. It helps in **encapsulating object creation logic** and **promotes loose coupling** in code.

## 🔹 When to Use the Factory Pattern?
- When object creation logic is **complex** and involves multiple steps.
- When a class **cannot anticipate** the type of objects it needs to create.
- When you want to **decouple** the client code from the specific concrete classes.
- When you want to introduce **abstraction** in object creation.

## 🔹 Types of Factory Patterns
1. **Simple Factory (Not a design pattern, but a common practice)**
2. **Factory Method Pattern**
3. **Abstract Factory Pattern**

## 🔹 Factory Pattern - Example in Java
A **factory class** creates objects of different types based on input.

### 🎯 Example: Shape Factory
```java
// Step 1: Create an interface
interface Shape {
    void draw();
}

// Step 2: Concrete Implementations of Shape
class Circle implements Shape {
    public void draw() {
        System.out.println("Drawing a Circle");
    }
}

class Square implements Shape {
    public void draw() {
        System.out.println("Drawing a Square");
    }
}

// Step 3: Create a Factory Class
class ShapeFactory {
    public Shape getShape(String shapeType) {
        if (shapeType == null) {
            return null;
        }
        if (shapeType.equalsIgnoreCase("CIRCLE")) {
            return new Circle();
        } else if (shapeType.equalsIgnoreCase("SQUARE")) {
            return new Square();
        }
        return null;
    }
}

// Step 4: Using the Factory
public class FactoryPatternExample {
    public static void main(String[] args) {
        ShapeFactory shapeFactory = new ShapeFactory();

        // Get an object of Circle and call its draw method
        Shape shape1 = shapeFactory.getShape("CIRCLE");
        shape1.draw();

        // Get an object of Square and call its draw method
        Shape shape2 = shapeFactory.getShape("SQUARE");
        shape2.draw();
    }
}
```

## 🔹 Benefits of Factory Pattern
✅ **Encapsulation:** Hides the object creation logic from the client.  
✅ **Loose Coupling:** Client code depends on abstraction rather than concrete classes.  
✅ **Code Reusability:** Reduces code duplication by centralizing object creation.  
✅ **Scalability:** Easily add new subclasses without modifying client code.  

---

# Abstract Factory Pattern
The **Abstract Factory Pattern** is an **extension of the Factory Method Pattern**. It provides an interface for creating **families of related or dependent objects** without specifying their concrete classes.

## 🔹 When to Use Abstract Factory Pattern?
- When multiple related objects need to be created together.
- When enforcing consistency among objects is required.
- When object creation needs to be abstracted for different environments (e.g., Windows, Mac UI components).

## 🔹 Abstract Factory Pattern - Example in Java
### 🎯 Example: GUI Factory (Windows & Mac UI Elements)
```java
// Step 1: Create Abstract Product Interfaces
interface Button {
    void paint();
}

interface Checkbox {
    void render();
}

// Step 2: Concrete Implementations for Windows
class WindowsButton implements Button {
    public void paint() {
        System.out.println("Rendering Windows Button");
    }
}

class WindowsCheckbox implements Checkbox {
    public void render() {
        System.out.println("Rendering Windows Checkbox");
    }
}

// Step 3: Concrete Implementations for Mac
class MacButton implements Button {
    public void paint() {
        System.out.println("Rendering Mac Button");
    }
}

class MacCheckbox implements Checkbox {
    public void render() {
        System.out.println("Rendering Mac Checkbox");
    }
}

// Step 4: Create Abstract Factory Interface
interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

// Step 5: Concrete Factories
class WindowsFactory implements GUIFactory {
    public Button createButton() {
        return new WindowsButton();
    }
    public Checkbox createCheckbox() {
        return new WindowsCheckbox();
    }
}

class MacFactory implements GUIFactory {
    public Button createButton() {
        return new MacButton();
    }
    public Checkbox createCheckbox() {
        return new MacCheckbox();
    }
}

// Step 6: Using the Abstract Factory
public class AbstractFactoryExample {
    public static void main(String[] args) {
        GUIFactory factory;
        
        // Assume OS is Windows
        factory = new WindowsFactory();
        
        Button button = factory.createButton();
        Checkbox checkbox = factory.createCheckbox();
        
        button.paint();
        checkbox.render();
    }
}
```

## 🔹 Benefits of Abstract Factory Pattern
✅ **Encapsulation:** Hides object creation logic for multiple related objects.  
✅ **Loose Coupling:** Factory ensures client code doesn't directly depend on concrete implementations.  
✅ **Flexibility:** Easily switch between families of objects (e.g., Windows UI ↔ Mac UI).  
✅ **Scalability:** Adding new families is easy without modifying existing code.  

---

## 🔹 Factory Method vs. Abstract Factory
| Feature | Factory Method | Abstract Factory |
|---------|---------------|------------------|
| **Definition** | Defines an interface for creating an object but lets subclasses decide which class to instantiate | Provides an interface for creating families of related objects without specifying concrete classes |
| **Usage** | Used when you need to create objects derived from a common interface | Used when you need to create related objects that belong to the same family |
| **Example** | `ShapeFactory` creating different `Shape` types | `GUIFactory` creating `Button` and `Checkbox` objects for different platforms (Windows, Mac) |



