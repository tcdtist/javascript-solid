# SOLID Principles in JavaScript Guide

The SOLID principles are foundational to object-oriented programming, designed to enhance the durability, maintainability, and scalability of code. In this guide, we will explore how to apply the SOLID principles within a JavaScript programming environment.

## Introduction

SOLID stands for:

- **S** - Single Responsibility Principle (SRP)
- **O** - Open/Closed Principle (OCP)
- **L** - Liskov Substitution Principle (LSP)
- **I** - Interface Segregation Principle (ISP)
- **D** - Dependency Inversion Principle (DIP)

Each principle in SOLID helps develop software systems that are easier to understand, maintain, and scale. Let’s go through each principle one by one.

## 1. Single Responsibility Principle (SRP)

### Explanation

A class should have only one reason to change, meaning it should have only one job. Segmenting functions helps reduce the complexity of the class and makes it easier to manage and understand.

### Example

```jsx
// Not good: A class performing multiple functions
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeUsername(username) {
    this.user.username = username;
  }

  render() {
    return `<div>${this.user.username}</div>`;
  }
}

// Better: Each class has a single responsibility
class User {
  constructor(user) {
    this.user = user;
  }

  changeUsername(username) {
    this.user.username = username;
  }
}

class UserSettingsRenderer {
  constructor(user) {
    this.user = user;
  }

  render() {
    return `<div>${this.user.username}</div>`;
  }
}
```

## 2. Open/Closed Principle (OCP)

### Explanation

Classes or objects should be open for extension but closed for modification. This principle encourages us to extend a class without modifying it directly.

### Example

```jsx
// Use inheritance and override to extend functionality without modifying original class
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  area() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  constructor(size) {
    super(size, size);
  }
}
```

## 3. Liskov Substitution Principle (LSP)

### Explanation

Objects of a subclass should be able to replace objects of the superclass without altering the correctness of the program. This emphasizes the necessity to design substitutable subclasses.

### Example

```jsx
class Bird {
  fly() {
    console.log("Can fly");
  }
}

class Duck extends Bird {}

class Ostrich extends Bird {
  fly() {
    throw new Error("Can't fly");
  }
}

function makeBirdFly(bird) {
  bird.fly();
}

// Duck can fly, no issues here
makeBirdFly(new Duck());

// Ostrich can't fly, calling fly() will cause an error
makeBirdFly(new Ostrich()); // Error! Violates LSP
```

## 4. Interface Segregation Principle (ISP)

### Explanation

Classes should not be forced to implement interfaces they do not use. ISP encourages splitting a large interface into smaller, more specific ones, allowing classes to only care about the interfaces that pertain to them.

### Example

```jsx
// Not good: An "overweight" interface not necessary
interface Worker {
  work();
  eat();
}

class Human implements Worker {
  work() {
    // working
  }

  eat() {
    // eating
  }
}

class Robot implements Worker {
  work() {
    // working
  }

  eat() {
    // Robots do not eat, but have to implement the method
  }
}

// Better: Clearly segregated interfaces
interface Workable {
  work();
}

interface Eatable {
  eat();
}

class Human implements Workable, Eatable {
  work() {
    // working
  }

  eat() {
    // eating
  }
}

class Robot implements Workable {
  work() {
    // working
  }
}
```

## 5. Dependency Inversion Principle (DIP)

### Explanation

High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details; details should depend on abstractions. This principle encourages reliance on interfaces or abstract classes rather than concrete classes.

### Example

```jsx
// Not good: Direct dependency on concrete classes
class User {
  constructor() {
    this.dbConnection = new MySQLConnection();
  }
}

// Better: Depend on abstraction
interface DBConnectionInterface {
  connect();
}

class MySQLConnection implements DBConnectionInterface {
  connect() {
    // MySQL connection logic
  }
}

class User {
  constructor(dbConnection: DBConnectionInterface) {
    this.dbConnection = dbConnection;
  }
}

const dbConnection = new MySQLConnection();
const user = new User(dbConnection);
```

## Conclusion

Applying SOLID principles in JavaScript not only improves the quality of the code but also makes it more maintainable, extensible, and manageable. Whether starting a new project or during the maintenance and upgrade of existing projects, consider how you can integrate these principles into your application's architecture and design to create robust and sustainable software solutions.

---

If you find this guide helpful, please do not hesitate to give us a star on [GitHub](https://github.com/tcdtist/public-javascript-solid)!
