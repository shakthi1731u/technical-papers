# Python SOLID Principles

## 1. What is SOLID?

SOLID is a group of five object-oriented design principles. They help us write code that is easier to maintain, test, and extend.

1. **S** - Single Responsibility Principle
2. **O** - Open/Closed Principle
3. **L** - Liskov Substitution Principle
4. **I** - Interface Segregation Principle
5. **D** - Dependency Inversion Principle

## 2. Single Responsibility Principle (SRP)

A class should have one main responsibility and one clear reason to change.

For example, invoice calculation, file saving, and email sending are separate responsibilities.

```python
class Invoice:
    def calculate_total(self):
        pass


class InvoiceFile:
    def save(self, invoice):
        pass


class EmailSender:
    def send(self, invoice):
        pass
```

Each class has a focused job.

## 3. Open/Closed Principle (OCP)

Software entities should be **open for extension but closed for modification**.

We should be able to add new behaviour without repeatedly changing existing working code.

```python
class EmailNotification:
    def send(self, message):
        print(f"Email: {message}")


class SMSNotification:
    def send(self, message):
        print(f"SMS: {message}")


def notify(notification, message):
    notification.send(message)
```

We can add another notification class with a `send()` method without changing `notify()`.

## 4. Liskov Substitution Principle (LSP)

A child class should be usable wherever its parent class is expected, without breaking the expected behaviour.

For example, if a function expects an object with an `area()` method, every supported shape should return a valid area.

When using inheritance, child classes should honour the behaviour promised by the parent class.

## 5. Interface Segregation Principle (ISP)

A class should not be forced to implement methods it does not need. Prefer small, focused interfaces.

```python
from abc import ABC, abstractmethod


class Printer(ABC):
    @abstractmethod
    def print_document(self):
        pass


class Scanner(ABC):
    @abstractmethod
    def scan_document(self):
        pass
```

A basic printer can implement only `Printer`. A multifunction device can implement both interfaces.

## 6. Dependency Inversion Principle (DIP)

High-level code should not depend directly on a specific low-level implementation. Both should depend on abstractions.

This makes implementations easier to replace.

```python
class EmailSender:
    def send(self, message):
        print(f"Email: {message}")


class Notification:
    def __init__(self, sender):
        self.sender = sender

    def notify(self, message):
        self.sender.send(message)


sender = EmailSender()
notification = Notification(sender)
notification.notify("Hello")
```

`Notification` receives its sender from outside. This is a simple example of dependency injection.

## 7. Quick Revision

| Principle | Main idea |
|---|---|
| SRP | One main responsibility per class |
| OCP | Extend behaviour without repeatedly modifying existing code |
| LSP | Child classes honour parent-class behaviour |
| ISP | Prefer small, focused interfaces |
| DIP | Depend on abstractions, not concrete implementations |

## 8. Why SOLID is useful

- Makes code easier to maintain.
- Reduces unnecessary dependencies.
- Makes testing easier.
- Helps developers add new features.
- Reduces the risk of breaking existing code during changes.
