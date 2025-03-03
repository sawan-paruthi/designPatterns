# Adapter Design Pattern 

## Overview
The **Adapter Design Pattern** is a **structural design pattern** that allows incompatible interfaces to work together. It acts as a bridge between two interfaces so that a client can use a service it otherwise wouldn't be able to use directly.

## Key Components
1. **Target Interface** – The expected interface by the client.
2. **Adaptee** – The existing class that needs adaptation.
3. **Adapter** – The class that implements the **Target Interface** and internally calls the **Adaptee** to bridge the gap.

## When to Use?
- When integrating a new system with legacy code.
- When two incompatible interfaces need to work together.
- When adapting an external library to match your system's interface.

## Example: Adapter Pattern in Java
### Step 1: Define the Target Interface
```java
// Target Interface - The modern USB-C charger
interface USBTypeC {
    void chargeWithUSBC();
}
```

### Step 2: Define the Adaptee
```java
// Adaptee - The legacy Lightning charger
class LightningCharger {
    void chargeWithLightning() {
        System.out.println("Charging using Lightning charger...");
    }
}
```

### Step 3: Create the Adapter
```java
// Adapter - Converts LightningCharger to USBTypeC
class LightningToUSBCAdapter implements USBTypeC {
    private LightningCharger lightningCharger;

    public LightningToUSBCAdapter(LightningCharger lightningCharger) {
        this.lightningCharger = lightningCharger;
    }

    @Override
    public void chargeWithUSBC() {
        System.out.println("Adapter converts USB-C request to Lightning...");
        lightningCharger.chargeWithLightning();
    }
}
```

### Step 4: Client Code
```java
public class AdapterPatternExample {
    public static void main(String[] args) {
        LightningCharger legacyCharger = new LightningCharger();
        USBTypeC adapter = new LightningToUSBCAdapter(legacyCharger);

        // Charging a modern device using the adapter
        adapter.chargeWithUSBC();
    }
}
```

### Output
```
Adapter converts USB-C request to Lightning...
Charging using Lightning charger...
```

## Types of Adapter Pattern
1. **Class Adapter** (Using Inheritance) → Uses multiple inheritance (not possible in Java).
2. **Object Adapter** (Using Composition) → Uses an instance of Adaptee inside Adapter. (Preferred in Java)

## Real-World Examples
- **Java IO Streams** (e.g., `InputStreamReader` converts byte streams to character streams).
- **Database Driver Wrappers** (e.g., JDBC drivers adapt SQL queries to database-specific implementations).
- **Legacy System Integration** (Bridging old APIs with modern applications).


