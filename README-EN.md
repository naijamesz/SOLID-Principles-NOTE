# S.O.L.I.D. Principles: Notes

**[Back to README.md](/README.md)**


## What are SOLID Design Principles?

The S.O.L.I.D. principles are a set of best practices in software development aimed at making code more maintainable, flexible, and scalable. By adhering to these principles, developers have been able to reduce redundancy, avoid side effects, and make systems easier to modify and extend without introducing bugs.

---

## SRP - Single Responsibility Principle

- A class should have had only one reason to change, meaning it should have been responsible for only one functionality.

##### Bad Code

```typescript
class UserService {
  createUser(name: string) {
    console.log(`User ${name} created`);
  }

  sendWelcomeEmail(name: string) {
    console.log(`Sending welcome email to ${name}`);
  }
}
```

**Problem**: The `UserService` class has been doing too much by both managing user creation and sending emails. This would have made it harder to maintain and modify since responsibilities have been mixed.

##### Best Practices

```typescript
class UserService {
  createUser(name: string) {
    console.log(`User ${name} created`);
  }
}

class EmailService {
  sendWelcomeEmail(name: string) {
    console.log(`Sending welcome email to ${name}`);
  }
}
```

**Improvement**: The concerns have been separated by splitting user management into `UserService` and email handling into `EmailService`.

**Use case**: This would have allowed future changes to either user creation or email sending without affecting the other part of the system.

---

## OCP - Open-Closed Principle

- Classes should have been open for extension but closed for modification.

##### Bad Code

```typescript
class Shape {
  type: string;

  constructor(type: string) {
    this.type = type;
  }

  getArea() {
    if (this.type === 'circle') {
      return Math.PI * 10 * 10;
    } else if (this.type === 'square') {
      return 20 * 20;
    }
  }
}
```

**Problem**: Adding a new shape would have required modifying the `Shape` class, increasing complexity and making the code harder to maintain.

##### Best Practices

```typescript
abstract class Shape {
  abstract getArea(): number;
}

class Circle extends Shape {
  getArea() {
    return Math.PI * 10 * 10;
  }
}

class Square extends Shape {
  getArea() {
    return 20 * 20;
  }
}

class Triangle extends Shape {
  getArea() {
    return (15 * 10) / 2;
  }
}
```

**Improvement**: `Polymorphism` has been applied, which would have allowed the `Shape` class to be extended without modifying it. Each shape implements its own `getArea()` method.

**Use case**: This principle would have been useful when adding new features or behaviors without changing existing code, such as adding new payment methods to a platform.

---

## LSP - Liskov Substitution Principle

- Subclasses should have been substitutable for their base classes without affecting the correctness of the program.

##### Bad Code

```typescript
class Bird {
  fly() {
    console.log('Flying');
  }
}

class Penguin extends Bird {
  fly() {
    throw new Error("Penguins can't fly");
  }
}
```

**Problem**: `Penguin` would not have been able to substitute `Bird` as expected since it cannot fly, causing errors when calling `fly()`.

##### Best Practices

```typescript
class Bird {
  move() {
    console.log('Moving');
  }
}

class Penguin extends Bird {
  move() {
    console.log("Penguins can't fly, but they swim.");
  }
}

class Eagle extends Bird {
  move() {
    console.log('Eagle is flying');
  }
}
```

**Improvement**: The `fly()` method has been replaced with a more general `move()` method, allowing different subclasses to implement their own behaviors, such as swimming for penguins and flying for eagles.

**Use case**: This principle would have applied when creating subclasses that need to maintain the behavior of their parent class while allowing for specialized behaviors.

---

## ISP - Interface Segregation Principle

- No client should have been forced to depend on methods it does not use. Interfaces should have been specific to the needs of each client.

##### Bad Code

```typescript
interface Worker {
  work(): void;
  eat(): void;
}

class Robot implements Worker {
  work() {
    console.log('Robot is working');
  }

  eat() {
    throw new Error("Robot can't eat");
  }
}
```

**Problem**: `Robot` would have been forced to implement the `eat()` method even though it cannot and does not need to.

##### Best Practices

```typescript
interface Workable {
  work(): void;
}

interface Eatable {
  eat(): void;
}

class Human implements Workable, Eatable {
  work() {
    console.log('Human is working');
  }

  eat() {
    console.log('Human is eating');
  }
}

class Robot implements Workable {
  work() {
    console.log('Robot is working');
  }
}
```

**Improvement**: The interfaces have been split into `Workable` and `Eatable`, so `Robot` doesn’t need to implement `eat()`.

**Use case**: ISP would have been used in systems where different objects or entities have required distinct behaviors, such as managing both humans and robots with specific interfaces.

---

## DIP - Dependency Inversion Principle

- High-level modules should not have depended on low-level modules. Both should have depended on abstractions.

##### Bad Code

```typescript
class Database {
  connect() {
    console.log('Connecting to database');
  }
}

class UserService {
  private db: Database = new Database();

  getUser() {
    this.db.connect();
    console.log('Getting user data');
  }
}
```

**Problem**: `UserService` would have been tightly coupled with `Database`, making it difficult to swap out or scale in the future.

##### Best Practices

```typescript
interface IDatabase {
  connect(): void;
}

class Database implements IDatabase {
  connect() {
    console.log('Connecting to database');
  }
}

class UserService {
  private db: IDatabase;

  constructor(db: IDatabase) {
    this.db = db;
  }

  getUser() {
    this.db.connect();
    console.log('Getting user data');
  }
}
```

**Improvement**: `UserService` now depends on the abstraction (`IDatabase`), allowing for different database implementations without modifying the `UserService` class.

**Use case**: DIP would have been applied in systems that required dependency injection, such as when using different databases in various environments.

