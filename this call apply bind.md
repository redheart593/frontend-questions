# this/call/apply/bind

## 1. 对this对象的理解

this 是执行上下文中的一个属性，它指向最后一次调用这个方法的对象。在实际开发中，this 的指向可以通过四种调用模式来判断。

- 第一种是**函数调用模式**，当一个函数不是一个对象的属性时，直接作为函数来调用时，this 指向全局对象。
- 第二种是**方法调用模式**，如果一个函数作为一个对象的方法来调用时，this 指向这个对象。
- 第三种是**构造器调用模式**，如果一个函数用 new 调用时，函数执行前会新创建一个对象，this 指向这个新创建的对象。
- 第四种是 **apply 、 call 和 bind 调用模式**，这三个方法都可以显示的指定调用函数的 this 指向。

这四种方式，使用构造器调用模式的优先级最高，然后是 apply、call 和 bind 调用模式，然后是方法调用模式，然后是函数调用模式。

## 2、bind、apply、call的作用与比较

### **三者的关键区别**

| **方法** | 调用时机         | 参数形式             | 返回结果       | 用途场景                   |
| :------- | :--------------- | :------------------- | :------------- | :------------------------- |
| `call`   | 立即执行         | 逗号分隔参数         | 原函数的返回值 | 需要动态指定 `this` 和参数 |
| `apply`  | 立即执行         | 数组形式参数         | 原函数的返回值 | 参数数量未知或需数组传递   |
| `bind`   | 返回绑定后的函数 | 逗号分隔参数（预置） | 新函数         | 延迟执行或固定上下文       |

### **1. `call()` 方法**

#### **功能**

- 立即调用函数，并显式指定函数内部的 `this` 指向。
- 参数以 **逗号分隔** 的形式逐个传递。

#### **语法**

```
func.call(thisArg, arg1, arg2, ...)
```

#### **示例**

```
const person = { name: "Alice" };

function greet(greeting, punctuation) {
  console.log(`${greeting}, ${this.name}${punctuation}`);
}

greet.call(person, "Hello", "!"); // 输出: "Hello, Alice!"
```

#### **核心用途**

- 借用其他对象的方法（如借用数组方法操作伪数组）。
- 实现继承（如构造函数中调用父类构造函数）。

### **2. `apply()` 方法**

#### **功能**

- 立即调用函数，显式指定 `this` 指向，参数以 **数组或伪数组** 形式传递。
- 与 `call()` 的唯一区别是参数传递方式。

#### **语法**

```
func.apply(thisArg, [argsArray])
```

#### **示例**

```
const numbers = [5, 2, 9, 1];
const max = Math.max.apply(null, numbers); // 等同于 Math.max(5, 2, 9, 1)
console.log(max); // 输出: 9
```

#### **核心用途**

- 传递动态参数（如未知数量的参数）。
- 合并数组（如 `Array.prototype.push.apply(arr1, arr2)`）。

------

### **3. `bind()` 方法**

#### **功能**

- 返回一个 **新函数**，新函数的 `this` 被永久绑定到 `thisArg`，并可预先设置部分参数。
- 不会立即执行原函数，需要手动调用新函数。

#### **语法**

```
const boundFunc = func.bind(thisArg, arg1, arg2, ...)
```

#### **示例**

```
const person = { name: "Bob" };

function greet(greeting) {
  console.log(`${greeting}, ${this.name}`);
}

const boundGreet = greet.bind(person, "Hi");
boundGreet(); // 输出: "Hi, Bob"
```

#### **核心用途**

- 固定函数的 `this`（如事件处理函数绑定上下文）。
- 柯里化函数（预先设置部分参数）。

## 3、实际应用场景

#### **1. 方法借用**（apply）

```
// 伪数组对象借用数组方法
const arrayLike = { 0: "a", 1: "b", length: 2 };
Array.prototype.push.call(arrayLike, "c"); // arrayLike 变为 {0: "a", 1: "b", 2: "c", length: 3}
```

#### **2. 构造函数继承**（apply）

```
function Parent(name) {
  this.name = name;
}

function Child(name, age) {
  Parent.call(this, name); // 调用父类构造函数
  this.age = age;
}
```

#### **3. 事件处理函数绑定 `this`**（bind）

```
class Button {
  constructor() {
    this.text = "Click me";
    this.handleClick = this.handleClick.bind(this); // 绑定 this
  }

  handleClick() {
    console.log(this.text);
  }
}

const btn = new Button();
document.addEventListener("click", btn.handleClick);
```

## 4、实际应用

#### **1. `call()` 的实际应用**

##### **场景1：方法借用**

- **类数组对象使用数组方法**
  类数组对象（如`arguments`、DOM元素集合）不具备数组方法，可通过`call`借用数组方法：

  ```
  function logArguments() {
    const args = Array.prototype.slice.call(arguments); // 将arguments转为数组
    console.log(args);
  }
  logArguments(1, 'a', true); // 输出: [1, 'a', true]
  ```

##### **场景2：继承中的构造函数调用**

- **子类调用父类构造函数**
  在继承中，使用`call`确保父类构造函数中的`this`指向子类实例：

  ```
  function Animal(name) {
    this.name = name;
  }
  function Dog(name, breed) {
    Animal.call(this, name); // 调用父类构造函数
    this.breed = breed;
  }
  const dog = new Dog('Buddy', 'Golden');
  console.log(dog.name); // 输出: Buddy
  ```

##### **场景3：动态上下文调用**

- **通用工具函数操作不同对象**
  编写通用函数处理不同对象的属性：

  ```
  function greet() {
    console.log(`Hello, ${this.name}!`);
  }
  const user = { name: 'Alice' };
  greet.call(user); // 输出: Hello, Alice!
  ```

------

#### **2. `apply()` 的实际应用**

##### **场景1：展开数组作为函数参数**

- **计算数组最大值**
  使用`apply`将数组展开为参数传递给`Math.max`：

  ```
  const numbers = [5, 2, 9, 1];
  const max = Math.max.apply(null, numbers); // 输出: 9
  ```

##### **场景2：合并数组**

- **合并两个数组**
  使用`apply`合并数组，避免循环：

  ```
  const arr1 = [1, 2];
  const arr2 = [3, 4];
  Array.prototype.push.apply(arr1, arr2); // arr1变为[1, 2, 3, 4]
  ```

##### **场景3：动态参数传递**

- **处理未知数量的参数**
  在不确定参数数量时，动态传递参数：

  ```
  function dynamicSum() {
    return Array.prototype.reduce.apply(arguments, [(a, b) => a + b]);
  }
  console.log(dynamicSum(1, 2, 3)); // 输出: 6
  ```

------

#### **3. `bind()` 的实际应用**

##### **场景1：事件处理函数绑定上下文**

- **确保回调函数中的`this`指向**
  在React类组件中绑定事件处理函数：

  ```
  class Button extends React.Component {
    constructor(props) {
      super(props);
      this.handleClick = this.handleClick.bind(this); // 绑定this到组件实例
    }
    handleClick() {
      console.log(this.props.message);
    }
    render() {
      return <button onClick={this.handleClick}>Click</button>;
    }
  }
  ```

##### **场景2：函数柯里化（预设参数）**

- **生成部分参数固定的新函数**
  通过`bind`预设参数，实现函数复用：

  ```
  function multiply(a, b) {
    return a * b;
  }
  const double = multiply.bind(null, 2); // 预设a=2
  console.log(double(5)); // 输出: 10
  ```

##### **场景3：延迟执行函数**

- **定时器中的上下文绑定**
  确保`setTimeout`回调中的`this`指向正确对象：

  ```
  const user = {
    name: 'Bob',
    sayHi() {
      console.log(`Hi, ${this.name}!`);
    },
  };
  setTimeout(user.sayHi.bind(user), 1000); // 1秒后输出: Hi, Bob!
  ```

------

#### **4. 对比与替代方案（ES6+）**

##### **箭头函数替代`bind`**

- **自动绑定外层`this`**
  箭头函数没有自己的`this`，简化上下文绑定：

  ```
  class Timer {
    constructor() {
      this.seconds = 0;
      setInterval(() => {
        this.seconds++; // this自动指向Timer实例
      }, 1000);
    }
  }
  ```

  ```
  class Button extends React.Component {
    // 使用箭头函数自动绑定 this
    handleClick = () => {
      console.log(this.props.message); // this 正确指向实例
    };
  
    render() {
      return <button onClick={this.handleClick}>Click</button>;
    }
  }
  ```

##### **展开运算符替代`apply`**

- **传递数组参数更简洁**
  使用`...`替代`apply`展开数组：

  ```
  const numbers = [1, 2, 3];
  Math.max(...numbers); // 输出: 3
  ```