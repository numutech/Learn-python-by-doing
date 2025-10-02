# Python Object-Oriented Programming (OOP) - Complete Guide

Based on the comprehensive tutorial from Chai aur Code, this document provides a structured overview of Python OOP concepts with practical examples and clear explanations.

## Table of Contents

1. [Introduction to OOP](#introduction-to-oop)
2. [Classes and Objects](#classes-and-objects)
3. [The `__init__` Method (Constructor)](#the-__init__-method-constructor)
4. [The `self` Parameter](#the-self-parameter)
5. [Instance Methods](#instance-methods)
6. [Class Attributes vs Instance Attributes](#class-attributes-vs-instance-attributes)
7. [Inheritance](#inheritance)
8. [Encapsulation](#encapsulation)
9. [Polymorphism](#polymorphism)
10. [Static Methods](#static-methods)
11. [Property Decorator](#property-decorator)
12. [Multiple Inheritance](#multiple-inheritance)
13. [Built-in Functions](#built-in-functions)

---

## Introduction to OOP

Object-Oriented Programming (OOP) is a programming paradigm that uses objects and classes to structure code. The instructor uses the analogy of making "gujiya" (a traditional sweet) to explain OOP concepts:

- **Class**: Like a mold or template (the gujiya mold)
- **Object**: Like the actual items created from the template (individual gujiyas)

This approach emphasizes that OOP is not complex but rather a natural way of organizing code that mirrors real-world patterns.

---

## Classes and Objects

### Definition

A **class** is a blueprint or template for creating objects. An **object** is an instance of a class with specific data.

### Basic Syntax

```python
# Define a class
class Car:
    # Class attributes (shared by all instances)
    brand = None
    model = None

# Create objects (instances)
my_car = Car()
my_new_car = Car()
```

### Key Points

- Class names follow **PascalCase** convention (e.g., `Car`, `ElectricCar`)
- Objects are created by calling the class like a function
- Each object is independent and can have different attribute values

### Example: Basic Car Class

```python
class Car:
    """A simple car class to demonstrate basic OOP concepts"""
    
    # Class attributes
    brand = None
    model = None

# Creating objects
my_car = Car()
my_new_car = Car()

# This approach has limitations - we can't set values during creation
```

---

## The `__init__` Method (Constructor)

The `__init__` method is a special method (constructor) that automatically runs when an object is created. It's used to initialize object attributes.

### Syntax

```python
class Car:
    def __init__(self, brand, model):
        # Initialize instance attributes
        self.brand = brand
        self.model = model
```

### Complete Example

```python
class Car:
    """Car class with constructor for initialization"""
    
    def __init__(self, brand, model):
        """Initialize a new car with brand and model"""
        self.brand = brand
        self.model = model

# Creating objects with initialization
my_car = Car("Toyota", "Camry")
my_new_car = Car("Tata", "Safari")

# Accessing attributes
print(my_car.brand)    # Output: Toyota
print(my_new_car.model)  # Output: Safari
```

### Key Points

- `__init__` is called automatically when creating objects
- It must take `self` as the first parameter
- Additional parameters can be passed during object creation
- Often called a "constructor" in other programming languages

---

## The `self` Parameter

The `self` parameter is a reference to the current instance of the class. It's used to access variables and methods belonging to the class.

### Understanding `self`

Think of `self` as a "telephone line" or "internet connection" that allows the class to communicate with its own attributes and methods.

```python
class Car:
    def __init__(self, brand, model):
        # self.brand refers to THIS object's brand attribute
        self.brand = brand    # Left side: instance attribute
        # brand refers to the parameter passed to __init__
        self.model = model
    
    def display_info(self):
        # Using self to access instance attributes
        print(f"Car: {self.brand} {self.model}")

# When you call my_car.display_info()
# Python automatically passes my_car as 'self'
my_car = Car("Honda", "Civic")
my_car.display_info()  # Output: Car: Honda Civic
```

### Key Points

- `self` is **always** the first parameter in instance methods
- It's automatically passed by Python when calling methods
- Similar to `this` keyword in other programming languages
- Allows access to instance attributes and methods within the class

---

## Instance Methods

Methods are functions defined inside a class that operate on object data.

### Basic Method Definition

```python
class Car:
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model
    
    def full_name(self):
        """Return the full name of the car"""
        return f"{self.brand} {self.model}"
    
    def start_engine(self):
        """Method to start the car engine"""
        return f"{self.brand} {self.model} engine started!"

# Using methods
my_car = Car("BMW", "X5")
print(my_car.full_name())      # Output: BMW X5
print(my_car.start_engine())   # Output: BMW X5 engine started!
```

### Method vs Function

- **Function**: Defined outside classes, standalone
- **Method**: Defined inside classes, operates on object data
- **Instance Method**: Requires `self` parameter, can access instance attributes

---

## Class Attributes vs Instance Attributes

### Instance Attributes

- Unique to each object
- Defined in `__init__` or other methods using `self`
- Each object can have different values

### Class Attributes

- Shared by all instances of the class
- Defined directly in the class body
- Same value for all objects (unless explicitly changed)

### Comprehensive Example

```python
class Car:
    # Class attribute - shared by all instances
    total_cars = 0
    vehicle_type = "Automobile"
    
    def __init__(self, brand, model):
        # Instance attributes - unique to each object
        self.brand = brand
        self.model = model
        
        # Increment class attribute when new car is created
        Car.total_cars += 1
    
    def get_info(self):
        """Return car information"""
        return f"{self.brand} {self.model}"
    
    @classmethod
    def get_total_cars(cls):
        """Return total number of cars created"""
        return cls.total_cars

# Creating objects
car1 = Car("Toyota", "Prius")
car2 = Car("Honda", "Accord")
car3 = Car("Tesla", "Model 3")

# Instance attributes are different
print(car1.brand)  # Output: Toyota
print(car2.brand)  # Output: Honda

# Class attributes are shared
print(car1.vehicle_type)  # Output: Automobile
print(car2.vehicle_type)  # Output: Automobile

# Class attribute reflects total count
print(Car.total_cars)     # Output: 3
print(car1.total_cars)    # Output: 3 (accessible through instance too)
```

### Key Differences

| Aspect | Instance Attributes | Class Attributes |
|--------|-------------------|------------------|
| **Definition** | `self.attribute = value` | `attribute = value` (in class body) |
| **Access** | `object.attribute` | `ClassName.attribute` or `object.attribute` |
| **Scope** | Unique per object | Shared across all objects |
| **Memory** | Separate memory per object | Single memory location |
| **Use Case** | Object-specific data | Shared constants, counters |

---

## Inheritance

Inheritance allows a class to inherit attributes and methods from another class, promoting code reuse and establishing relationships between classes.

### Basic Inheritance

```python
# Parent class (Base class)
class Car:
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model
    
    def full_name(self):
        return f"{self.brand} {self.model}"
    
    def fuel_type(self):
        return "Petrol or Diesel"

# Child class (Derived class)
class ElectricCar(Car):  # Inherits from Car
    def __init__(self, brand, model, battery_size):
        # Call parent class constructor using super()
        super().__init__(brand, model)
        # Add new attribute specific to electric cars
        self.battery_size = battery_size
    
    # Override parent method with different behavior
    def fuel_type(self):
        return "Electric Charge"
    
    # Add new method specific to electric cars
    def charging_info(self):
        return f"Battery size: {self.battery_size} kWh"

# Using inheritance
regular_car = Car("Toyota", "Camry")
tesla = ElectricCar("Tesla", "Model S", 100)

# ElectricCar has access to parent methods
print(tesla.full_name())        # Output: Tesla Model S (inherited)
print(tesla.charging_info())    # Output: Battery size: 100 kWh (own method)

# Different behaviors for same method (polymorphism)
print(regular_car.fuel_type())  # Output: Petrol or Diesel
print(tesla.fuel_type())        # Output: Electric Charge
```

### The `super()` Function

The `super()` function provides access to parent class methods, particularly useful in constructors:

```python
class ElectricCar(Car):
    def __init__(self, brand, model, battery_size):
        # Instead of rewriting brand and model initialization:
        # self.brand = brand
        # self.model = model
        
        # Use super() to call parent constructor
        super().__init__(brand, model)
        self.battery_size = battery_size
```

### Key Benefits of Inheritance

1. **Code Reuse**: Don't repeat common functionality
2. **Hierarchical Organization**: Model real-world relationships
3. **Extensibility**: Add new features while maintaining existing ones
4. **Polymorphism**: Same interface, different behaviors

---

## Encapsulation

Encapsulation is the bundling of data and methods that work on that data within a single unit (class), and restricting access to some components.

### Private Attributes (Name Mangling)

In Python, prefix attributes with double underscores (`__`) to make them "private":

```python
class Car:
    def __init__(self, brand, model):
        # Public attributes
        self.model = model
        # Private attribute (name mangling)
        self.__brand = brand
    
    def get_brand(self):
        """Getter method for private brand attribute"""
        return self.__brand + "!"  # Can add processing logic
    
    def set_brand(self, new_brand):
        """Setter method for private brand attribute"""
        if new_brand:  # Validation logic
            self.__brand = new_brand
        else:
            print("Brand cannot be empty!")

# Using encapsulation
my_car = Car("Toyota", "Camry")

# This works - public attribute
print(my_car.model)  # Output: Camry

# This won't work - private attribute
# print(my_car.__brand)  # AttributeError

# Use getter method instead
print(my_car.get_brand())  # Output: Toyota!

# Use setter method
my_car.set_brand("Honda")
print(my_car.get_brand())  # Output: Honda!
```

### Complete Encapsulation Example

```python
class BankAccount:
    def __init__(self, account_holder, initial_balance):
        self.account_holder = account_holder  # Public
        self.__balance = initial_balance      # Private
    
    def get_balance(self):
        """Safe way to check balance"""
        return f"Balance: ${self.__balance:.2f}"
    
    def deposit(self, amount):
        """Deposit money with validation"""
        if amount > 0:
            self.__balance += amount
            return f"Deposited ${amount}. New balance: ${self.__balance}"
        else:
            return "Deposit amount must be positive!"
    
    def withdraw(self, amount):
        """Withdraw money with validation"""
        if amount > 0 and amount <= self.__balance:
            self.__balance -= amount
            return f"Withdrew ${amount}. New balance: ${self.__balance}"
        else:
            return "Invalid withdrawal amount!"

# Using the encapsulated class
account = BankAccount("John Doe", 1000)

print(account.get_balance())     # Output: Balance: $1000.00
print(account.deposit(500))      # Output: Deposited $500. New balance: $1500
print(account.withdraw(200))     # Output: Withdrew $200. New balance: $1300

# Direct access to balance is prevented
# print(account.__balance)       # AttributeError
```

### Benefits of Encapsulation

1. **Data Protection**: Prevent direct modification of sensitive data
2. **Validation**: Add checks when setting values
3. **Flexibility**: Change internal implementation without affecting external code
4. **Maintenance**: Easier to debug and modify

---

## Polymorphism

Polymorphism allows objects of different classes to be treated as objects of a common base class, with each class providing its own implementation of methods.

### Method Overriding Example

```python
class Vehicle:
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model
    
    def fuel_type(self):
        return "Generic fuel"
    
    def start_sound(self):
        return "Generic engine sound"

class Car(Vehicle):
    def fuel_type(self):
        return "Petrol or Diesel"
    
    def start_sound(self):
        return "Vroom vroom!"

class ElectricCar(Vehicle):
    def fuel_type(self):
        return "Electric Charge"
    
    def start_sound(self):
        return "Silent start"

class Motorcycle(Vehicle):
    def fuel_type(self):
        return "Petrol"
    
    def start_sound(self):
        return "Roar!"

# Polymorphism in action
vehicles = [
    Car("Toyota", "Camry"),
    ElectricCar("Tesla", "Model 3"),
    Motorcycle("Harley", "Davidson")
]

# Same method call, different behaviors
for vehicle in vehicles:
    print(f"{vehicle.brand} {vehicle.model}:")
    print(f"  Fuel: {vehicle.fuel_type()}")
    print(f"  Sound: {vehicle.start_sound()}")
    print()

# Output:
# Toyota Camry:
#   Fuel: Petrol or Diesel
#   Sound: Vroom vroom!
#
# Tesla Model 3:
#   Fuel: Electric Charge
#   Sound: Silent start
#
# Harley Davidson:
#   Fuel: Petrol
#   Sound: Roar!
```

### Duck Typing Example

Python supports "duck typing" - if it looks like a duck and quacks like a duck, it's a duck:

```python
class Dog:
    def speak(self):
        return "Woof!"

class Cat:
    def speak(self):
        return "Meow!"

class Robot:
    def speak(self):
        return "Beep boop!"

def make_sound(creature):
    """This function works with any object that has a speak() method"""
    return creature.speak()

# Polymorphism through duck typing
animals = [Dog(), Cat(), Robot()]

for animal in animals:
    print(make_sound(animal))

# Output:
# Woof!
# Meow!
# Beep boop!
```

---

## Static Methods

Static methods belong to the class rather than any specific instance. They don't access instance data and don't require `self`.

### Static Method Example

```python
class Car:
    total_cars = 0
    
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model
        Car.total_cars += 1
    
    @staticmethod
    def general_description():
        """Static method - doesn't need self or instance data"""
        return "Cars are vehicles with four wheels used for transportation."
    
    @staticmethod
    def is_valid_year(year):
        """Static utility method"""
        current_year = 2024
        return 1900 <= year <= current_year
    
    def instance_method(self):
        """Regular instance method for comparison"""
        return f"This is a {self.brand} {self.model}"

# Using static methods
# Can be called on the class directly
print(Car.general_description())
# Output: Cars are vehicles with four wheels used for transportation.

print(Car.is_valid_year(2020))  # Output: True
print(Car.is_valid_year(2030))  # Output: False

# Can also be called on instances (but not recommended)
my_car = Car("Honda", "Civic")
print(my_car.general_description())  # Works but not recommended

# Regular instance method
print(my_car.instance_method())  # Output: This is a Honda Civic
```

### When to Use Static Methods

1. **Utility Functions**: Related to the class but don't need instance data
2. **Validation**: Check if values are valid before creating instances
3. **Factory Methods**: Alternative constructors
4. **Helper Functions**: Common operations related to the class

---

## Property Decorator

The `@property` decorator allows methods to be accessed like attributes, providing controlled access to data.

### Basic Property Example

```python
class Car:
    def __init__(self, brand, model):
        self.__brand = brand    # Private attribute
        self.__model = model    # Private attribute
    
    @property
    def brand(self):
        """Read-only access to brand"""
        return self.__brand
    
    @property
    def model(self):
        """Read-only access to model"""
        return self.__model
    
    @property
    def full_name(self):
        """Computed property"""
        return f"{self.__brand} {self.__model}"

# Using properties
my_car = Car("BMW", "X5")

# Access like attributes (no parentheses)
print(my_car.brand)      # Output: BMW
print(my_car.model)      # Output: X5
print(my_car.full_name)  # Output: BMW X5

# These won't work - read-only properties
# my_car.brand = "Audi"  # AttributeError
```

### Property with Getter and Setter

```python
class Circle:
    def __init__(self, radius):
        self.__radius = radius
    
    @property
    def radius(self):
        """Getter for radius"""
        return self.__radius
    
    @radius.setter
    def radius(self, value):
        """Setter for radius with validation"""
        if value <= 0:
            raise ValueError("Radius must be positive!")
        self.__radius = value
    
    @property
    def area(self):
        """Computed property - read-only"""
        return 3.14159 * self.__radius ** 2
    
    @property
    def diameter(self):
        """Another computed property"""
        return 2 * self.__radius

# Using properties with getters and setters
circle = Circle(5)

print(circle.radius)    # Output: 5
print(circle.area)      # Output: 78.53975
print(circle.diameter)  # Output: 10

# Using setter
circle.radius = 10
print(circle.area)      # Output: 314.159

# Validation in action
try:
    circle.radius = -5  # ValueError: Radius must be positive!
except ValueError as e:
    print(e)
```

---

## Multiple Inheritance

Python supports multiple inheritance, where a class can inherit from multiple parent classes.

### Multiple Inheritance Example

```python
class Engine:
    def __init__(self, engine_type):
        self.engine_type = engine_type
    
    def engine_info(self):
        return f"Engine: {self.engine_type}"

class Battery:
    def __init__(self, battery_capacity):
        self.battery_capacity = battery_capacity
    
    def battery_info(self):
        return f"Battery: {self.battery_capacity} kWh"

class Car:
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model
    
    def car_info(self):
        return f"Car: {self.brand} {self.model}"

# Multiple inheritance
class HybridCar(Engine, Battery, Car):
    def __init__(self, brand, model, engine_type, battery_capacity):
        # Initialize all parent classes
        Engine.__init__(self, engine_type)
        Battery.__init__(self, battery_capacity)
        Car.__init__(self, brand, model)
    
    def hybrid_info(self):
        return f"Hybrid vehicle with {self.engine_type} engine and {self.battery_capacity} kWh battery"

# Using multiple inheritance
prius = HybridCar("Toyota", "Prius", "Hybrid", 1.8)

print(prius.car_info())      # Output: Car: Toyota Prius
print(prius.engine_info())   # Output: Engine: Hybrid
print(prius.battery_info())  # Output: Battery: 1.8 kWh
print(prius.hybrid_info())   # Output: Hybrid vehicle with Hybrid engine and 1.8 kWh battery
```

### Method Resolution Order (MRO)

Python uses Method Resolution Order to determine which method to call when multiple inheritance is involved:

```python
class A:
    def method(self):
        return "Method from A"

class B:
    def method(self):
        return "Method from B"

class C(A, B):  # Inherits from both A and B
    pass

# MRO: C -> A -> B -> object
obj = C()
print(obj.method())  # Output: Method from A (A comes first in MRO)

# Check MRO
print(C.__mro__)
# Output: (<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>, <class 'object'>)
```

---

## Built-in Functions

### `isinstance()` Function

Check if an object is an instance of a specific class or its subclasses:

```python
class Vehicle:
    pass

class Car(Vehicle):
    pass

class ElectricCar(Car):
    pass

# Create objects
my_vehicle = Vehicle()
my_car = Car()
my_tesla = ElectricCar()

# isinstance() checks
print(isinstance(my_tesla, ElectricCar))  # Output: True
print(isinstance(my_tesla, Car))          # Output: True (inheritance)
print(isinstance(my_tesla, Vehicle))      # Output: True (inheritance)
print(isinstance(my_car, ElectricCar))    # Output: False

# Check multiple types
print(isinstance(my_tesla, (Car, Vehicle)))  # Output: True
```

### `hasattr()` Function

Check if an object has a specific attribute:

```python
class Car:
    def __init__(self, brand):
        self.brand = brand
    
    def start(self):
        return "Car started"

my_car = Car("Toyota")

print(hasattr(my_car, 'brand'))  # Output: True
print(hasattr(my_car, 'model'))  # Output: False
print(hasattr(my_car, 'start'))  # Output: True
```

### `getattr()` Function

Get an attribute value with optional default:

```python
class Car:
    def __init__(self, brand):
        self.brand = brand

my_car = Car("Honda")

print(getattr(my_car, 'brand'))           # Output: Honda
print(getattr(my_car, 'model', 'Unknown'))  # Output: Unknown (default)
```

---

## Summary

This comprehensive guide covers all the essential Python OOP concepts:

1. **Classes and Objects**: Templates and instances
2. **Constructor (`__init__`)**: Object initialization
3. **`self` Parameter**: Instance reference
4. **Attributes**: Instance vs class variables  
5. **Methods**: Functions within classes
6. **Inheritance**: Code reuse and hierarchies
7. **Encapsulation**: Data protection and access control
8. **Polymorphism**: Same interface, different behaviors
9. **Static Methods**: Class-level utilities
10. **Properties**: Controlled attribute access
11. **Multiple Inheritance**: Inheriting from multiple classes

### Key Takeaways

- OOP in Python is straightforward when understood through practical examples
- Start with simple classes and gradually add complexity
- Use inheritance for code reuse and logical relationships
- Apply encapsulation to protect and validate data
- Leverage polymorphism for flexible, maintainable code
- Properties provide elegant ways to control attribute access

### Best Practices

1. Use clear, descriptive class and method names
2. Follow PEP 8 naming conventions
3. Keep classes focused on a single responsibility
4. Use inheritance judiciously - favor composition when appropriate
5. Document your classes and methods with docstrings
6. Validate input in setters and constructors
7. Use `@property` for computed attributes and data validation

This foundation in Python OOP will enable you to write more organized, maintainable, and scalable code.