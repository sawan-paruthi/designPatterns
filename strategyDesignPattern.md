One of the major benefits of the **Strategy Design Pattern** is that it helps address situations where **not all child classes in a hierarchy need the same behavior** or where some child classes need to use entirely different implementations of a behavior. Instead of forcing all child classes to inherit behavior from the parent (which might not apply to some of them), the Strategy Pattern decouples that behavior and makes it optional and flexible.

---

### The Problem with Forcing Behavior in Parent Classes

In a traditional inheritance model, if you put the behavior in the parent class, **all child classes are forced to either use it or override it**, even if it doesn't make sense for some of them.

#### Example: Animals and Movement (Without Strategy Pattern)

```java
class Animal {
    void move() {
        System.out.println("This animal moves.");
    }
}

class Dog extends Animal {
    @Override
    void move() {
        System.out.println("The dog walks on four legs.");
    }
}

class Bird extends Animal {
    @Override
    void move() {
        System.out.println("The bird flies in the sky.");
    }
}

// What if a Fish doesn't need the default "move" behavior?
class Fish extends Animal {
    @Override
    void move() {
        System.out.println("The fish swims in water.");
    }
}
```

In this case:

- Every child class **must override the `move` method** even if the default implementation doesn't make sense (e.g., a Fish doesn't "walk" or "fly").
- This introduces unnecessary coupling between the behavior (`move`) and the class hierarchy.

---

### How the Strategy Pattern Solves This Problem

The Strategy Pattern allows you to **completely remove the `move` behavior from the parent class** and instead delegate it to a strategy. This means:

1. Only the relevant classes need to use or assign a `MovementStrategy`.
2. Child classes that don’t need the behavior at all can simply ignore it.

#### Example: Animals with Strategy Pattern

##### Step 1: Define the Strategy Interface

```java
interface MovementStrategy {
    void move();
}
```

##### Step 2: Create Concrete Strategies

```java
class WalkMovement implements MovementStrategy {
    @Override
    public void move() {
        System.out.println("Walking on four legs.");
    }
}

class FlyMovement implements MovementStrategy {
    @Override
    public void move() {
        System.out.println("Flying in the sky.");
    }
}

class SwimMovement implements MovementStrategy {
    @Override
    public void move() {
        System.out.println("Swimming in water.");
    }
}
```

##### Step 3: Context Class (Optional Behavior)

```java
class Animal {
    private MovementStrategy movementStrategy;

    public Animal(MovementStrategy movementStrategy) {
        this.movementStrategy = movementStrategy;
    }

    public Animal() {
        // Some animals may not need a movement strategy.
        this.movementStrategy = null;
    }

    public void setMovementStrategy(MovementStrategy movementStrategy) {
        this.movementStrategy = movementStrategy;
    }

    public void move() {
        if (movementStrategy != null) {
            movementStrategy.move();
        } else {
            System.out.println("This animal doesn't move in the usual sense.");
        }
    }
}
```

##### Step 4: Test the Code

```java
public class Main {
    public static void main(String[] args) {
        Animal dog = new Animal(new WalkMovement());
        dog.move(); // Output: Walking on four legs.

        Animal bird = new Animal(new FlyMovement());
        bird.move(); // Output: Flying in the sky.

        Animal fish = new Animal(new SwimMovement());
        fish.move(); // Output: Swimming in water.

        // Animal with no movement strategy
        Animal coral = new Animal();
        coral.move(); // Output: This animal doesn't move in the usual sense.
    }
}
```

---

### Key Advantages in This Scenario:

1. **Flexibility**: Child classes that don't need the behavior (like `coral` in the example) can simply skip assigning a `MovementStrategy`, avoiding unnecessary coupling.
2. **Reusability**: The same strategy (e.g., `SwimMovement`) can be used by multiple unrelated classes (e.g., `Fish` and `Frog`).
3. **Open/Closed Principle**: You can add new movement strategies (e.g., "crawl") without modifying existing code.
4. **Eliminates Code Duplication**: Common behavior (e.g., walking) is encapsulated in a strategy class and reused.

---

### When a Child Doesn't Use a Behavior:

With the **Strategy Pattern**, this is handled elegantly:

- If a child doesn't need a behavior, it simply doesn't use or assign the strategy.
- The parent class (or the context class) ensures that the behavior is optional and doesn't enforce it.

#### Why This Works:

By decoupling the behavior into strategy classes, the parent class and its children no longer need to "know" or "care" about the behavior directly. This keeps the class hierarchy **clean** and **focused on what it should represent** rather than forcing every child to deal with irrelevant functionality.

---

### When to Use Strategy Pattern for Such Cases:

1. When **some children in a hierarchy don’t need the behavior**.
2. When behavior needs to be dynamically assigned or modified at runtime.
3. When you want to avoid tightly coupling behavior to a class hierarchy.
