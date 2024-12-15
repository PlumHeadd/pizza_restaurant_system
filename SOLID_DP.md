# SOLID Principles and Design Patterns in Pizza Restaurant System

This document explains how the Pizza Restaurant System adheres to the SOLID principles using various design patterns.

---

## **1. Single Responsibility Principle (SRP)**

### **Description**
The Single Responsibility Principle states that a class should have only one reason to change. Each class should handle a single, well-defined functionality.

### **Implementation**
- **InventoryManager (Singleton Pattern)**: Manages inventory updates and ensures consistent stock tracking.
- **PizzaFactory (Factory Pattern)**: Focuses solely on pizza creation logic.
- **ToppingDecorator (Decorator Pattern)**: Handles the dynamic addition of toppings to pizzas.
- **PaymentMethod (Strategy Pattern)**: Manages payment logic independently.

### **Example**
The `PizzaFactory` class is responsible only for creating pizza objects:

```python
class PizzaFactory:
    @staticmethod
    def create_pizza(pizza_type):
        if pizza_type == "Margherita":
            return Margherita()
        elif pizza_type == "Pepperoni":
            return Pepperoni()
```

**Benefit:** Each class has a clear purpose, making the code easier to maintain and extend.

---

## **2. Open/Closed Principle (OCP)**

### **Description**
The Open/Closed Principle states that classes should be open for extension but closed for modification. New functionality should be added without altering existing code.

### **Implementation**
- **Decorator Pattern**: Allows the addition of new toppings dynamically without modifying the existing pizza classes.

### **Example**
Adding a new topping (e.g., Bacon) can be achieved by creating a new decorator class:

```python
class Bacon(ToppingDecorator):
    def get_description(self):
        return f"{self.pizza.get_description()}, Bacon"

    def get_cost(self):
        return self.pizza.get_cost() + 40.0
```

**Benefit:** Existing pizza and topping classes remain unchanged.

---

## **3. Liskov Substitution Principle (LSP)**

### **Description**
The Liskov Substitution Principle states that objects of a superclass should be replaceable with objects of a subclass without affecting the functionality.

### **Implementation**
- All toppings inherit from the `ToppingDecorator` class, which itself inherits from the base `Pizza` class. This ensures that toppings behave like pizzas.

### **Example**
A `Cheese` topping can be used wherever a `Pizza` object is expected:

```python
pizza = Margherita()
pizza = Cheese(pizza)
print(pizza.get_description())  # "Margherita Pizza, Cheese"
```

**Benefit:** Ensures consistent behavior across all subclasses.

---

## **4. Interface Segregation Principle (ISP)**

### **Description**
The Interface Segregation Principle states that no client should be forced to depend on methods it does not use. Interfaces should be small and specific.

### **Implementation**
- The `PaymentMethod` interface is simple and specific, containing only the `pay` method.

### **Example**
Each payment method implements the `PaymentMethod` interface:

```python
class PaymentMethod(ABC):
    @abstractmethod
    def pay(self, amount):
        pass

class Cash(PaymentMethod):
    def pay(self, amount):
        print(f"Paid {amount:.2f} EGP in cash.")
```

**Benefit:** Each payment method handles only its own logic without being burdened by unnecessary methods.

---

## **5. Dependency Inversion Principle (DIP)**

### **Description**
The Dependency Inversion Principle states that high-level modules should not depend on low-level modules. Both should depend on abstractions.

### **Implementation**
- The `main` function depends on the `PaymentMethod` abstraction rather than concrete payment classes like `Cash` or `OrangeCash`.

### **Example**
The payment logic uses the abstract `PaymentMethod` interface:

```python
payment_method = Cash()
payment_method.pay(total_cost)
```

**Benefit:** New payment methods can be added without changing the `main` function.

---

By adhering to the SOLID principles, the Pizza Restaurant System is modular, maintainable, and extensible, ensuring it meets current requirements while being adaptable to future changes.

