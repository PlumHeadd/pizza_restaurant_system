# Design Patterns in Pizza Restaurant System

This document explains the design patterns used in the Pizza Restaurant System and their implementation.

---

## **1. Singleton Pattern: Inventory Manager**

### **Description**
The Singleton Pattern ensures a class has only one instance and provides a global point of access to it. In this system, it is used to manage inventory, ensuring that the same inventory is shared across all operations.

### **Implementation**
The `InventoryManager` class uses the Singleton Pattern. This ensures that all updates to the inventory (like decrementing ingredient stock) affect the same instance.

```python
class InventoryManager:
    _instance = None
    _inventory = { ... }  # Ingredient stock levels

    def __new__(cls, *args, **kwargs):
        if not cls._instance:
            cls._instance = super().__new__(cls)
        return cls._instance
```

### **Benefits**
- Ensures consistency in inventory data.
- Centralizes inventory management.

---

## **2. Factory Pattern: Pizza Factory**

### **Description**
The Factory Pattern provides a way to create objects without specifying their concrete classes. This simplifies object creation and adds flexibility.

### **Implementation**
The `PizzaFactory` class is responsible for creating `Margherita` and `Pepperoni` pizzas.

```python
class PizzaFactory:
    @staticmethod
    def create_pizza(pizza_type):
        if pizza_type == "Margherita":
            return Margherita()
        elif pizza_type == "Pepperoni":
            return Pepperoni()
```

### **Benefits**
- Simplifies object creation.
- Makes it easy to add new pizza types in the future.

---

## **3. Decorator Pattern: Toppings**

### **Description**
The Decorator Pattern allows behavior to be added to individual objects dynamically. It is used here to add toppings (Cheese, Olives, Mushrooms) to pizzas.

### **Implementation**
Each topping is a decorator class inheriting from the `ToppingDecorator` class, which wraps a `Pizza` object.

```python
class Cheese(ToppingDecorator):
    def get_description(self):
        return f"{self.pizza.get_description()}, Cheese"

    def get_cost(self):
        return self.pizza.get_cost() + 30.0
```

### **Benefits**
- Flexible way to add toppings.
- Each topping is independent and reusable.

---

## **4. Strategy Pattern: Payment Methods**

### **Description**
The Strategy Pattern defines a family of algorithms (payment methods) and allows them to be used interchangeably. This is used to support multiple payment methods (Cash, Orange Cash).

### **Implementation**
The `PaymentMethod` interface defines a common `pay` method. Classes `Cash` and `OrangeCash` implement this interface.

```python
class Cash(PaymentMethod):
    def pay(self, amount):
        print(f"Paid {amount:.2f} EGP in cash.")
```

### **Benefits**
- Easy to add new payment methods.
- Each payment method is isolated, improving maintainability.

---

## **Overengineering**

### **Concept**
Overengineering occurs when a system is designed with unnecessary complexity. In this project, introducing advanced patterns without justification could lead to overengineering.

### **Example**
If we added separate factories for each topping, it would increase code complexity without significant benefit:

```python
class CheeseFactory:
    @staticmethod
    def create_topping(pizza):
        return Cheese(pizza)
```

This adds extra layers without improving functionality.

