# Python Object-Oriented Programming and File Operations

## Overview
This document covers essential concepts of Object-Oriented Programming (OOP) in Python, along with a step-by-step guide on handling file operations. It provides insights into inheritance, polymorphism, encapsulation, method overriding, and file handling techniques, making it a comprehensive guide for Python developers.

---

## Object-Oriented Programming (OOP) Concepts
### 1. Inheritance
Inheritance allows a class to derive properties and methods from another class, promoting code reuse.

```python
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        return f"{self.name} makes a sound"

class Dog(Animal):
    def speak(self):  # Overriding the method
        return f"{self.name} barks"
```

### 2. Polymorphism
Polymorphism allows different classes to have methods with the same name but different implementations.

```python
def animal_sound(animal):
    print(animal.speak())

dog = Dog("Buddy")
animal_sound(dog)  # Output: Buddy barks
```

### 3. Encapsulation
Encapsulation restricts access to certain data within an object.

```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance  # Private attribute
    
    def get_balance(self):
        return self.__balance
```

### 4. Shallow Copy vs. Deep Copy

```python
import copy

class Address:
    def __init__(self, city):
        self.city = city

class Person:
    def __init__(self, name, address):
        self.name = name
        self.address = address

addr = Address("New York")
p1 = Person("John", addr)
p2 = copy.deepcopy(p1)
```

---

## File Handling in Python
### 1. Creating a File
```python
def create_file():
    with open("test.txt", "w") as file:
        file.write("Hello, World!")
```

### 2. Writing to a File
```python
def write_to_file():
    with open("test.txt", "w") as file:
        file.write("Overwriting content")
```

### 3. Reading from a File
```python
def read_from_file():
    with open("test.txt", "r") as file:
        print(file.read())
```

### 4. Appending to a File
```python
def append_to_file():
    with open("test.txt", "a") as file:
        file.write("\nAppending new content")
```

### 5. Checking if a File Exists
```python
import os

def check_if_file_exists():
    if os.path.exists("test.txt"):
        print("File exists")
    else:
        print("File does not exist")
```

### 6. Deleting a File
```python
def delete_file():
    os.remove("test.txt")
```

### 7. Getting File Metadata
```python
def get_file_info():
    print("File Size:", os.path.getsize("test.txt"))
    print("Last Modified:", os.path.getmtime("test.txt"))
```

---

## Conclusion
This guide introduces core OOP principles and provides a structured approach to file handling in Python. By understanding inheritance, polymorphism, encapsulation, and various file operations, developers can build robust, scalable applications efficiently.
