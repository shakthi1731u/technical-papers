# Python OOPs

## 1. What is OOP?

Object-Oriented Programming (OOP) is a way of organising code using classes and objects. It groups related data and functions together. OOP can make larger programs easier to organise, reuse, and maintain.

## 2. Class and Object

A **class** is a blueprint. An **object** is an instance created from that class.

```python
class Student:
    def introduce(self):
        print("I am a student")


student_one = Student()
student_one.introduce()
```

- `Student` is the class.
- `student_one` is an object.
- `introduce()` is a method.

## 3. Constructor: `__init__()`

The `__init__()` method runs when an object is created. It is commonly used to initialise object data.

```python
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age


student = Student("Arun", 22)
print(student.name)
```

## 4. What is `self`?

`self` refers to the current object. It lets us access that object's variables and methods.

```python
class Counter:
    def __init__(self):
        self.value = 0

    def increment(self):
        self.value += 1
```

## 5. Instance and Class Variables

- **Instance variable:** Belongs to an individual object.
- **Class variable:** Belongs to the class and is shared by instances unless overridden.

```python
class Employee:
    company = "ABC Technologies"

    def __init__(self, name):
        self.name = name


first = Employee("Arun")
second = Employee("Bala")
```

Here, `name` is an instance variable and `company` is a class variable.

## 6. Types of Methods

### Instance method

Receives `self` and works with object data.

```python
def show_name(self):
    print(self.name)
```

### Class method

Receives `cls` and works with class-level data. Use `@classmethod`.

```python
@classmethod
def show_company(cls):
    print(cls.company)
```

### Static method

Does not automatically receive `self` or `cls`. Use it for a related operation that does not need object or class data.

```python
@staticmethod
def add(first, second):
    return first + second
```

## 7. Four Main OOP Concepts

### Encapsulation

Groups data and methods inside a class and controls how data is accessed.

Python naming conventions include:
- `name`: public.
- `_name`: internal-use convention.
- `__name`: name-mangled attribute.

A leading underscore is a convention, not strict access protection.

### Inheritance

Allows a child class to reuse or extend a parent class.

```python
class Animal:
    def eat(self):
        print("Eating")


class Dog(Animal):
    def bark(self):
        print("Barking")


dog = Dog()
dog.eat()
dog.bark()
```

### Polymorphism

Different objects can use the same method name but provide different behaviour.

```python
class Dog:
    def sound(self):
        print("Bark")


class Cat:
    def sound(self):
        print("Meow")


for animal in [Dog(), Cat()]:
    animal.sound()
```

### Abstraction

Shows essential operations while hiding implementation details. Python supports abstract classes through the `abc` module.

```python
from abc import ABC, abstractmethod


class Shape(ABC):
    @abstractmethod
    def area(self):
        pass
```

A concrete child class must implement the abstract method before it can be instantiated.

## 8. Method Overriding

A child class can provide its own implementation of an inherited method.

```python
class Animal:
    def sound(self):
        print("Animal sound")


class Dog(Animal):
    def sound(self):
        print("Bark")
```

## 9. The `super()` Function

`super()` lets a child class call methods from its parent class. It is commonly used to initialise parent-class data.

```python
class Person:
    def __init__(self, name):
        self.name = name


class Student(Person):
    def __init__(self, name, course):
        super().__init__(name)
        self.course = course
```

## 10. Special Methods

Special methods have double underscores at the beginning and end. Python calls them for specific operations.

| Method | Purpose |
|---|---|
| `__init__()` | Initialise an object |
| `__str__()` | Readable string representation |
| `__repr__()` | Representation useful for debugging |
| `__len__()` | Define behaviour for `len()` |

## Quick Revision

| Concept | Meaning |
|---|---|
| Class | Blueprint for objects |
| Object | Instance of a class |
| `self` | Current object reference |
| `__init__()` | Initialises object data |
| Encapsulation | Groups data and methods |
| Inheritance | Reuses or extends a class |
| Polymorphism | Same interface, different behaviour |
| Abstraction | Hides implementation details |
| Overriding | Replaces inherited method behaviour |
| `super()` | Calls parent-class behaviour |

