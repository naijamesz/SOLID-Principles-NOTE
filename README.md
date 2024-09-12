# S.O.L.I.D. Principles: 5 Notes

## What are SOLID Design Principles?

คือองค์ประกอบหนึ่งของ Clean code ที่เป็น Best Practices ในการทำการเขียนโปรแกรมเพื่อลดความซ้ำซ้อนและเพิ่มความปลอดภัยของโปรแกรมสามารถทำการใช้งานหรือแก้ไขส่วนต่างๆได้อย่างง่ายโดยเพื่อลดผลกระทบต่อโมดูลอื่นๆให้เหลือน้อยที่สุด

## SRP - Single Responsibility Principle

- หลักการที่ว่าแต่ละคลาสควรมีความรับผิดชอบเดียวของตนเอง

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

`ปัญหา`: UserService มีหน้าที่หลายอย่าง ที่อยู่ในความรับผิดชอบของคลาสนี้ทั้งการสร้างผู้ใช้และส่งอีเมล ทำให้มีความรับผิดชอบมากเกินไป ซึ่งจะก่อให้เกิดผลกระทบหากมีการเพิ่ม แก้ไข หรือมีความเปลี่ยนแปลงที่อื่นๆที่ส่งผลกระทบต่อโมดูลอื่นๆ

##### ฺBest Practices

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

`ปรับปรุง`:แยกหน้าที่ออกจากกันอย่างชัดเจน `UserService` ทำหน้าที่จัดการผู้ใช้ส่วนคลาส `EmailService` จะมีหน้าที่รับผิดชอบในส่วนของจัดการอีเมล

Use case : สามารถใช้แยกการทำงานของการจัดการผู้ใช้กับบริการแจ้งเตือนได้โดยไม่ส่งผลกระทบต่อกันเนื่องจากมีส่วนรับผิดชอบที่ไม่ได้ผูกติดกัน

---

## OCP - Open-Closed Principle

หลักการที่ว่าคลาสควรมีการเปิดให้ขยาย(extend) แต่ปิดไม่ให้ทำการเปลี่ยนแปลงตัวคลาสได้ หรือ คลาสควรมีการเปิดให้ขยาย(extend) แต่ปิดไม่ให้ทำการเปลี่ยนแปลงตัวคลาส

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

ปัญหา : การเพิ่มประเภทของ `Shape` จะต้องมาทำการแก้ไขในคลาสโดยใส่เคงื่อนไขซับซ้อนกันไปมาหากเราต้องการใช้ method `getArea` ในคลาส `Shape` ซึ่งเป็นคลาสหลักใน method `getArea` ซึ่งอาจจะสร้างปัญหาเมื่อทำการแก้ไข method ภายในคลาส `Shape`

##### ฺBest Practices

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

class Rectangle extends Shape {
  getArea() {
    return 10 * 20;
  }
}
```

`ปรับปรุง`: ปัญหาข้างต้นสามารถแก้ไขได้โดยนำหลักการของ `polymorphism` มาปรับใช้ทำให้ไม่ต้องแก้ไขคลาส `Shape` หากมีการเพิ่มรูปทรงใหม่เพียงแค่สร้างคลาสใหม่ แต่เพียงแก้ไข method `getArea` ในคลาสที่สร้างขึ้นมาโดยทำการนำ `Shape` ที่มีการเปิดให้ขยาย(extends) แต่ปิดไม่ให้ทำการเปลี่ยนแปลงตัวคลาสเพื่อทำการใช้ method `getArea` แม้จะเป็นรูปทรงที่แตกต่างกัน

`Use case`: ใช้เมื่อต้องการเพิ่มฟีเจอร์ใหม่โดยไม่ต้องแก้ไขโค้ดเดิม เช่น การเพิ่มรูปแบบการชำระเงินใหม่ในระบบที่มีการเปิดให้ขยาย(extend) แต่ปิดไม่ให้ทำการเปลี่ยนแปลงตัวคลาส เป็นต้น

---

## LSP - Liskov Substitution Principle

คือหลักการที่ว่า"คลาสย่อยหรือ(subclass)สามารถทำหน้าที่แทนคลาสหลักได้โดยไม่ต้องแก้ไขคลาสหลัก" ที่อาจส่งผลกระทุบต่อคลาสย่อยอื่นๆที่รับคุณสมบัติมาจากคลาสหลักดังกลาวให้เกิดผลกระทบที่ต้องแก้ไขในหลายๆตำแหน่งที่ให้เกิดความซ้ำซ้อนโดยไม่จำเป็นได้

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

`ปัญหา`: `Penguin`จะไม่สามารถแทนที่คลาส `Bird` ได้ตามที่คาดหวัง เพราะมันไม่สามารถบินได้จึงใช้ method `fly` จากคลาส `Bird` ไม่ได้

##### ฺBest Practices

```typescript
class Bird {
  move() {
    console.log('Moving');
  }
}

class Penguin extends Bird {
  move() {
    console.log(`Penguin is Swimming. Penguins can't fly`);
  }
}

class Eagle extends Penguin {
  move() {
    console.log('Eagle is flying');
  }
}

const penguin = new Penguin();
penguin.move(); // Penguin is Swimming. Penguins can't fly

const eagle = new Eagle().move(); // Eagle is flying
```

`ปรับปรุง`: ทำการเปลี่ยน method `fly` เป็น `move` จากคลาส `Bird` เพื่อนำมาใช้เป็น method `move` ให้คลาส `Penguin`ที่มีความสามารถแตกต่างจากสัวต์ปีชนิดอื่นอย่างเช่น `Eagle` ที่สามารถบินได้

`Use case`:ใช้เมื่อมีการสร้างคลาสที่สืบทอดจากคลาสหลักและต้องการคงพฤติกรรมเดิม(method)ของคลาสหลัก

---

## ISP - Interface Segregation Principle

หลักการนี้ว่าด้วยการไม่ควรบังคับให้คลาสจะต้องใช้ interface ที่มี method ที่ไม่จำเป็นต้องใช้ได้

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

`ปัญหา`: จะเห็นได้ว่า `Robot` ไม่สามารถเอา method `eat` ที่เป็นพฤติกรรมที่ `Robot`ไม่จำเป็นต้องมีหรือไม่ได้มีพฤติกรรมแบบนั้นตั้งแต่แรก

##### ฺBest Practices

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

`ปรับปรุง`: แยก interface ออกเป็น `Workable` และ `Eatable` ออกจากกัน ทำให้ `Robot` ไม่ต้องมีเมธอด `eat()` ก็ได้เนื่องจากไม่จำเป็นต้องมีพฤติกรรมแบบนั้นตั้งแต่แรก

`Use case`:
ใช้ในระบบที่มีการทำงานแตกต่างกันตามประเภทของออบเจกต์ เช่น ระบบจัดการพนักงาน,และระบบจัดการหุ่นยนต์

---

## DIP - Dependency Inversion Principle

หลักการที่ว่าด้วยเรื่องการที่คลาสพึ่่งพาการ abstractions (interfaces) มากกว่าคลาสพึ่งพาการ concretions (implementation) เนื่องจากการทำงานของคลาสพึ่งพาการอยู่ในระดับล่างของคลาสพึ่งพาการ

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

`ปัญหา`: `UserService` พึ่งพาโดยผูกโยงกันโดยตรงกับ `Database` ซึ่งทำให้ยากต่อการปรับปรุงหรือทำการขยายสเกลในอนาคต

##### ฺBest Practices

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

`ปรับปรุง`: `UserService` พึ่งพา interface `IDatabase` ทำให้สามารถเปลี่ยนฐานข้อมูลหรือเพิ่มฐานข้อมูลได้ในอนาคต

`Use case`:ใช้ในระบบที่ต้องการทำ Dependency Injection (DI) เช่น ระบบที่ใช้ฐานข้อมูลต่างชนิดในสภาพแวดล้อมที่แตกต่างกัน
