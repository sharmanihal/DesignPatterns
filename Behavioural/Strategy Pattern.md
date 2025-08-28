# **Strategy Pattern**

## **Definition**

The **Strategy Pattern** is a behavioral design pattern that enables selecting an algorithm’s behavior at runtime. Instead of implementing a single behavior directly in a class (leading to duplication and rigidity), the behavior is encapsulated in separate **strategy classes** and injected into the context class using **composition**.

It follows the principle:

👉 **Favor composition over inheritance**

👉 **Encapsulate what varies**

---

## **Problem Without Strategy Pattern**

Example: **Duck Simulation**

* Base class: `Duck` with methods `fly()` and `quack()`.
* Subclasses: `CityDuck`, `MountainDuck`, `RubberDuck`, `QuailDuck`, `WhistlingDuck`.

### Issues:

1. **Code duplication**

   * `QuailDuck` and `WhistlingDuck` share the same flying behavior but duplicate the code.
2. **Violation of DRY principle**

   * Common behavior repeated across subclasses.
3. **Rigid design**

   * Adding a new flying style requires modifying or overriding multiple classes.
4. **Incorrect inheritance**

   * `RubberDuck` inherits `fly()` but has to override it to "do nothing" since it cannot fly.

---

## **Solution With Strategy Pattern**

### Idea:

* **Extract varying behaviors** (fly, quack) into separate interfaces.
* Provide multiple **implementations** for those interfaces.
* Use **composition** to assign a behavior to a duck dynamically.

---

### **Interfaces**

```java
// Flying behavior
public interface FlyBehavior {
    void fly();
}

// Quacking behavior
public interface QuackBehavior {
    void quack();
}
```

### **Concrete Implementations**

```java
// Fly behaviors
public class FlyWithWings implements FlyBehavior {
    public void fly() { System.out.println("Flying with wings"); }
}

public class FlyNoWay implements FlyBehavior {
    public void fly() { System.out.println("I can't fly"); }
}

public class FlyWhistlingStyle implements FlyBehavior {
    public void fly() { System.out.println("Flying in whistling style"); }
}

// Quack behaviors
public class NormalQuack implements QuackBehavior {
    public void quack() { System.out.println("Quack! Quack!"); }
}

public class Squeak implements QuackBehavior {
    public void quack() { System.out.println("Squeak!"); }
}
```

### **Duck Class (Context)**

```java
public class Duck {
    private FlyBehavior flyBehavior;
    private QuackBehavior quackBehavior;

    // Constructor Injection
    public Duck(FlyBehavior flyBehavior, QuackBehavior quackBehavior) {
        this.flyBehavior = flyBehavior;
        this.quackBehavior = quackBehavior;
    }

    public void performFly() {
        flyBehavior.fly();
    }

    public void performQuack() {
        quackBehavior.quack();
    }

    // Behavior can also be changed at runtime
    public void setFlyBehavior(FlyBehavior fb) {
        this.flyBehavior = fb;
    }

    public void setQuackBehavior(QuackBehavior qb) {
        this.quackBehavior = qb;
    }
}
```

### **Client Code**

```java
public class StrategyPatternDemo {
    public static void main(String[] args) {
        Duck cityDuck = new Duck(new FlyWithWings(), new NormalQuack());
        cityDuck.performFly();   // Flying with wings
        cityDuck.performQuack(); // Quack! Quack!

        Duck rubberDuck = new Duck(new FlyNoWay(), new Squeak());
        rubberDuck.performFly();   // I can't fly
        rubberDuck.performQuack(); // Squeak!

        // Dynamically change behavior
        rubberDuck.setFlyBehavior(new FlyWhistlingStyle());
        rubberDuck.performFly();   // Flying in whistling style
    }
}
```

---

## **Class Diagram**

```
        +------------------+
        |     FlyBehavior  |<-----+
        |------------------|      |
        | + fly(): void    |      |
        +------------------+      |
               ^                  |
               |                  |
  +--------------------+   +-------------------+
  |   FlyWithWings     |   |   FlyNoWay        |
  |--------------------|   |-------------------|
  | + fly(): void      |   | + fly(): void     |
  +--------------------+   +-------------------+

        +------------------+
        |  QuackBehavior   |<-----+
        |------------------|      |
        | + quack(): void  |      |
        +------------------+      |
               ^                  |
               |                  |
  +-------------------+    +-------------------+
  |   NormalQuack     |    |     Squeak        |
  |-------------------|    |-------------------|
  | + quack(): void   |    | + quack(): void   |
  +-------------------+    +-------------------+

                +---------------------------------+
                |              Duck               |
                |---------------------------------|
                | - flyBehavior: FlyBehavior      |
                | - quackBehavior: QuackBehavior  |
                |---------------------------------|
                | + performFly(): void            |
                | + performQuack(): void          |
                | + setFlyBehavior(fb)            |
                | + setQuackBehavior(qb)          |
                +---------------------------------+
```

---

## **Key Points**

* Removes code duplication by **encapsulating behavior** in separate classes.
* Allows **runtime behavior change** (flexibility).
* Makes code follow **Open/Closed Principle (OCP)**.
* Eliminates misuse of inheritance → **Composition > Inheritance**.

