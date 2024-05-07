# Hướng dẫn Nguyên tắc SOLID trong JavaScript

Nguyên tắc SOLID là một trong những nền tảng cơ bản của lập trình hướng đối tượng, được thiết kế để cải thiện độ bền vững, khả năng bảo trì và mở rộng của code. Trong hướng dẫn này, chúng ta sẽ khám phá cách áp dụng các nguyên tắc SOLID trong môi trường lập trình JavaScript.

## Giới thiệu

SOLID là từ viết tắt của:

- **S** - Single Responsibility Principle (SRP)
- **O** - Open/Closed Principle (OCP)
- **L** - Liskov Substitution Principle (LSP)
- **I** - Interface Segregation Principle (ISP)
- **D** - Dependency Inversion Principle (DIP)

Mỗi nguyên tắc trong SOLID đều giúp phát triển các hệ thống phần mềm dễ đọc, dễ hiểu và dễ bảo trì hơn. Hãy cùng đi qua từng nguyên tắc một.

## 1. Single Responsibility Principle (SRP) - Nguyên tắc Trách nhiệm Đơn lẻ

### Giải thích

Một lớp (class) chỉ nên có một lý do để thay đổi, có nghĩa là nó chỉ nên có một trách nhiệm duy nhất. Việc phân tách các chức năng giúp làm giảm độ phức tạp của lớp và làm cho lớp đó dễ dàng hơn để hiểu và bảo trì.

### Ví dụ

```jsx
// Không tốt: Một class thực hiện nhiều chức năng
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

// Tốt: Mỗi class chỉ có một trách nhiệm
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

## 2. Open/Closed Principle (OCP) - Nguyên tắc Mở/Đóng

### Giải thích

Các lớp hoặc đối tượng nên mở cho việc mở rộng nhưng đóng cho việc sửa đổi. Nguyên tắc này khuyến khích chúng ta mở rộng một lớp mà không sửa đổi trực tiếp lớp đó.

### Ví dụ

```jsx
// Sử dụng kế thừa và ghi đè để mở rộng chức năng mà không sửa đổi lớp gốc
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

## 3. Liskov Substitution Principle (LSP) - Nguyên tắc Thay thế Liskov

### Giải thích

Các đối tượng của một lớp con phải có thể thay thế cho các đối tượng của lớp cha mà không làm thay đổi tính đúng đắn của chương trình. Điều này nhấn mạnh sự cần thiết của việc thiết kế các lớp con có thể hoán đổi được với các lớp cha của chúng.

### Ví dụ

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

// Duck có thể bay, không vấn đề gì xảy ra
makeBirdFly(new Duck());

// Ostrich không thể bay, và việc gọi hàm fly() sẽ gây ra lỗi
makeBirdFly(new Ostrich()); // Lỗi! Vi phạm LSP
```

## 4. Interface Segregation Principle (ISP) - Nguyên tắc Phân tách Giao diện

### Giải thích

Các lớp không nên bị buộc phải triển khai các giao diện mà chúng không sử dụng. ISP khuyến khích chia nhỏ các giao diện lớn thành những cái nhỏ hơn, cụ thể hơn, giúp lớp triển khai chỉ cần quan tâm đến các giao diện liên quan đến chức năng của chúng.

### Ví dụ

```jsx
// Không tốt: Giao diện "thừa" không cần thiết
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

// Tốt: Giao diện được phân tách rõ ràng
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

## 5. Dependency Inversion Principle (DIP) - Nguyên tắc Đảo ngược Phụ thuộc

### Giải thích

Các module cấp cao không nên phụ thuộc vào các module cấp thấp. Cả hai nên phụ thuộc vào trừu tượng. Trừu tượng không nên phụ thuộc vào chi tiết; chi tiết nên phụ thuộc vào trừu tượng. Nguyên tắc này khuyến khích sự phụ thuộc vào các giao diện hoặc lớp trừu tượng thay vì các lớp cụ thể.

### Ví dụ

```jsx
// Không tốt: Phụ thuộc trực tiếp vào các lớp cụ thể
class User {
  constructor() {
    this.dbConnection = new MySQLConnection();
  }
}

// Tốt: Phụ thuộc vào trừu tượng
interface DBConnectionInterface {
  connect();
}

class MySQLConnection implements DBConnectionInterface {
  connect() {
    // logic kết nối MySQL
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

## Kết luận

Áp dụng các nguyên tắc SOLID trong JavaScript không chỉ cải thiện chất lượng code mà còn làm cho nó dễ bảo trì, mở rộng và quản lý hơn. Khi bắt đầu một dự án mới hoặc trong quá trình bảo trì và nâng cấp các dự án hiện có, hãy xem xét cách bạn có thể tích hợp các nguyên tắc này vào kiến trúc và thiết kế của ứng dụng của bạn để tạo ra những giải pháp phần mềm vững chắc và bền vững.

---

Nếu bạn thấy hướng dẫn này hữu ích, xin đừng ngần ngại để lại một sao cho chúng tôi trên [GitHub](https://github.com/tcdtist/public-javascript-solid)!
