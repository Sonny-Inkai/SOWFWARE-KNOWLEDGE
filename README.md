# SOWFWARE-KNOWLEDGE

### SOLID Principles

The SOLID principles are a set of five design principles intended to make software designs more understandable, flexible, and maintainable. These principles were introduced by Robert C. Martin (also known as Uncle Bob). Here's a detailed look at each principle along with examples to help you understand them better.

### 1. Single Responsibility Principle (SRP)
**Definition**: A class should have only one reason to change, meaning it should have only one responsibility.

**Example**:
Imagine a class that handles both user authentication and user profile management.

```python
class User:
    def __init__(self, username, password, email):
        self.username = username
        self.password = password
        self.email = email

    def authenticate(self, password):
        return self.password == password

    def update_email(self, new_email):
        self.email = new_email
```

Here, the `User` class has two responsibilities: authentication and profile management. According to SRP, these should be separated.

```python
class UserAuthentication:
    def __init__(self, user):
        self.user = user

    def authenticate(self, password):
        return self.user.password == password

class UserProfile:
    def __init__(self, user):
        self.user = user

    def update_email(self, new_email):
        self.user.email = new_email
```

Now, each class has a single responsibility.

### 2. Open/Closed Principle (OCP)
**Definition**: Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.

**Example**:
Consider a simple payment system that supports only PayPal payments.

```python
class Payment:
    def pay_with_paypal(self, amount):
        print(f"Paid {amount} using PayPal.")
```

Now, if we need to add support for credit card payments, we would need to modify the existing class, which violates OCP.

A better approach is to use inheritance or interfaces to extend functionality without modifying the existing code.

```python
class Payment:
    def pay(self, amount):
        raise NotImplementedError("Subclasses should implement this method.")

class PayPalPayment(Payment):
    def pay(self, amount):
        print(f"Paid {amount} using PayPal.")

class CreditCardPayment(Payment):
    def pay(self, amount):
        print(f"Paid {amount} using Credit Card.")
```

Now, adding new payment methods doesn't require modifying the existing classes.

### 3. Liskov Substitution Principle (LSP)
**Definition**: Objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program.

**Example**:
Consider a base class `Bird` with a method `fly`.

```python
class Bird:
    def fly(self):
        print("Flying")
```

Now, if we create a subclass `Penguin` which cannot fly, substituting `Penguin` for `Bird` would break the program.

```python
class Penguin(Bird):
    def fly(self):
        raise NotImplementedError("Penguins cannot fly")
```

A better design is to segregate flying behavior into a separate class.

```python
class Bird:
    pass

class FlyingBird(Bird):
    def fly(self):
        print("Flying")

class Penguin(Bird):
    def swim(self):
        print("Swimming")
```

### 4. Interface Segregation Principle (ISP)
**Definition**: Clients should not be forced to depend on interfaces they do not use.

**Example**:
Consider an interface `Worker` with methods `work` and `eat`.

```python
class Worker:
    def work(self):
        pass

    def eat(self):
        pass
```

Now, if we have a `Robot` class that only works but doesn't eat, it shouldn't be forced to implement `eat`.

```python
class Robot(Worker):
    def work(self):
        print("Robot is working")

    def eat(self):
        pass  # Robots don't eat
```

A better approach is to create more specific interfaces.

```python
class Workable:
    def work(self):
        pass

class Eatable:
    def eat(self):
        pass

class Worker(Workable, Eatable):
    def work(self):
        print("Worker is working")

    def eat(self):
        print("Worker is eating")

class Robot(Workable):
    def work(self):
        print("Robot is working")
```

### 5. Dependency Inversion Principle (DIP)
**Definition**: High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions.

**Example**:
Consider a class `Button` that directly depends on a class `Lamp`.

```python
class Lamp:
    def turn_on(self):
        print("Lamp is on")

class Button:
    def __init__(self, lamp):
        self.lamp = lamp

    def press(self):
        self.lamp.turn_on()
```

This design tightly couples `Button` with `Lamp`. A better approach is to depend on an abstraction.

```python
class Switchable:
    def turn_on(self):
        pass

class Lamp(Switchable):
    def turn_on(self):
        print("Lamp is on")

class Button:
    def __init__(self, device: Switchable):
        self.device = device

    def press(self):
        self.device.turn_on()
```

Now, `Button` can work with any `Switchable` device, making the code more flexible and easier to extend.

### Summary
- **SRP**: One class, one responsibility.
- **OCP**: Open for extension, closed for modification.
- **LSP**: Subtypes must be substitutable for their base types.
- **ISP**: Prefer small, specific interfaces.
- **DIP**: Depend on abstractions, not concretions.

By adhering to these principles, you can create software that is modular, easy to maintain, and scalable.
