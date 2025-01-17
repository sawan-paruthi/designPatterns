# Observer Design Pattern

The Observer design pattern is a behavioral design pattern where an object (called the **subject**) maintains a list of its dependents (called **observers**) and notifies them of any state changes, usually by calling one of their methods. This pattern is widely used in implementing distributed event-handling systems, in which one object (the subject) changes state and all registered observers are automatically notified and updated.

## Key Components

- **Subject**: The object being observed. It maintains a list of observers and provides methods to add, remove, and notify them.
- **Observer**: The interface or abstract class that defines a method to update itself when the subject changes its state.
- **ConcreteSubject**: A subclass of the subject that stores its state and sends notifications to observers when the state changes.
- **ConcreteObserver**: A class that implements the observer interface and updates itself when notified of changes in the subject's state.

---

## Example

Imagine a weather monitoring system where the **WeatherStation** is the subject, and different display devices (like **CurrentConditionsDisplay**, **ForecastDisplay**, etc.) are observers.

### 1. Observer Interface
```java
public interface Observer {
    void update(float temperature, float humidity, float pressure);
}
```

### 2. ConcreteObserver
```java
public class CurrentConditionsDisplay implements Observer {
    private float temperature;
    private float humidity;

    @Override
    public void update(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        display();
    }

    public void display() {
        System.out.println("Current conditions: " + temperature + "F degrees and " + humidity + "% humidity");
    }
}
```

### 3. Subject Interface
```java
public interface Subject {
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObservers();
}
```

### 4. ConcreteSubject
```java
import java.util.ArrayList;
import java.util.List;

public class WeatherStation implements Subject {
    private List<Observer> observers;
    private float temperature;
    private float humidity;
    private float pressure;

    public WeatherStation() {
        observers = new ArrayList<>();
    }

    @Override
    public void registerObserver(Observer o) {
        observers.add(o);
    }

    @Override
    public void removeObserver(Observer o) {
        observers.remove(o);
    }

    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(temperature, humidity, pressure);
        }
    }

    public void setMeasurements(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        notifyObservers();
    }
}
```

### 5. Client Code
```java
public class Main {
    public static void main(String[] args) {
        WeatherStation weatherStation = new WeatherStation();
        CurrentConditionsDisplay currentDisplay = new CurrentConditionsDisplay();

        weatherStation.registerObserver(currentDisplay);
        weatherStation.setMeasurements(80, 65, 30.4f);
    }
}
```

---

## Benefits

1. **Loose coupling**: The subject and observer are loosely coupled, meaning that the subject doesn't need to know any details about the observers. It only knows that it has a list of observers to notify.
2. **Flexibility**: Observers can be added or removed at any time without affecting the subject.

## Drawbacks

1. **Memory leak**: If observers are not removed from the list when they are no longer needed, they can cause memory leaks.
2. **Performance**: In some cases, notifying a large number of observers can be inefficient, particularly if many observers need to be updated frequently.

---

This pattern is often used in event-driven systems, such as GUI frameworks, and in real-time monitoring systems.
