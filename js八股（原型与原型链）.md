# js八股（原型与原型链）

### 1. 对原型、原型链的理解

#### 原型

在JavaScript中是使用**构造函数**来新建一个对象的，每一个构造函数的内部都有一个 **prototype(原型对象)** 属性，它的属性值是一个对象，这个对象包含了可以**由该构造函数的所有实例共享的属性和方法**。当使用构造函数新建一个对象后，在这个对象的内部将包含一个指针，被称为**对象的原型**，这个指针指向构造函数的 prototype 属性对应的值。一般来说不应该能够获取到这个值的，但是现在浏览器中都实现了**`__proto__`**属性来访问这个属性，但是最好不要使用这个属性，因为它不是规范中规定的。ES5 中新增了一个 Object.getPrototypeOf() 方法，可以通过这个方法来获取对象的原型。

```js
function Person(name) {
  this.name = name;
}
Person.prototype.sayHello = function() {
  console.log("Hello, " + this.name);
};

const alice = new Person("Alice");
console.log(alice.__proto__ === Person.prototype); // true
console.log(Object.getPrototypeOf(alice) === Person.prototype); // true
```

#### 原型链

当访问一个对象的属性时，如果这个对象内部不存在这个属性，那么它就会去它的prototype原型对象里找这个属性，这个原型对象又会有自己的原型，于是就这样一直找下去，也就是原型链的概念。原型链的尽头一般来说都是 **Object.prototype** 再往上找Object.prototype.`__proto__`是null。新建的对象能够使用 toString() 等Object方法，就是因为它们的原型最终都是Object.prototype。

```js
function Animal(name) {
  this.name = name;
}
Animal.prototype.eat = function() {
  console.log(this.name + " is eating.");
};

function Dog(name) {
  Animal.call(this, name); // 继承属性
}
Dog.prototype = Object.create(Animal.prototype); // 继承方法
Dog.prototype.bark = function() {
  console.log("Woof!");
};

const dog = new Dog("Buddy");

// 原型链的层级：
// dog → Dog.prototype → Animal.prototype → Object.prototype → null
```

**特点：** JavaScript 对象是通过引用来传递的，创建的每个新对象实体中并没有一份属于自己的原型副本。当修改原型时，与之相关的对象也会继承这一改变。

#### constructor属性属性的基本作用

1. **默认行为**：

   - 每个函数的 `prototype` 对象都有一个 `constructor` 属性，默认指向该函数本身。
   - 通过 `new` 创建的实例对象，其 `constructor` 属性通过原型链继承自构造函数的 `prototype.constructor`。

   ```js
   function Person(name) {
     this.name = name;
   }
   
   // 默认情况下，Person.prototype.constructor === Person
   console.log(Person.prototype.constructor === Person); // true
   
   const alice = new Person("Alice");
   console.log(alice.constructor === Person); // true（通过原型链继承）
   ```

2. **作用**：

   - **标识对象的构造函数**：可以通过 `obj.constructor` 判断对象是由哪个构造函数创建的。
   - **动态创建同类对象**：例如通过 `new obj.constructor()` 创建新实例（需谨慎使用，可能有意外行为）。

### 2、原型修改、重写，属性覆盖

```javascript
function Person(name) {
    this.name = name
}
// 修改原型
Person.prototype.getName = function() {}
var p = new Person('hello')
console.log(p.__proto__ === Person.prototype) // true
console.log(p.__proto__ === p.constructor.prototype) // true
// 重写原型
Person.prototype = {
    getName: function() {}
}
var p = new Person('hello')
console.log(p.__proto__ === Person.prototype)        // true
console.log(p.__proto__ === p.constructor.prototype) // false
```

可以看到修改原型的时候p的构造函数依然指向Person，而重写原型时因为直接给Person的原型对象直接用对象赋值时，它的构造函数指向的了根构造函数Object，所以这时候`p.constructor === Object` ，而不是`p.constructor === Person`。要想成立，就要用constructor指回来：

```javascript
Person.prototype = {
    getName: function() {}
}
var p = new Person('hello')
p.constructor = Person
console.log(p.__proto__ === Person.prototype)        // true
console.log(p.__proto__ === p.constructor.prototype) // true
```

### 3、如何获得对象非原型链上的属性？

使用后`hasOwnProperty()`方法来判断属性是否属于原型链的属性：

```javascript
javascript 代码解读复制代码function iterate(obj){
   var res=[];
   for(var key in obj){
        if(obj.hasOwnProperty(key))
           res.push(key+': '+obj[key]);
   }
   return res;
} 
```

### 4、列举一些原型链指向

###  原型链指向

```javascript
javascript 代码解读复制代码p.__proto__  // Person.prototype
Person.prototype.__proto__  // Object.prototype
p.__proto__.__proto__ //Object.prototype
p.__proto__.constructor.prototype.__proto__ // Object.prototype
Person.prototype.constructor.prototype.__proto__ // Object.prototype
p1.__proto__.constructor // Person
Person.prototype.constructor  // Person
```

### 5、原型相关的操作方法

#### 1. **检查原型关系**

- **`instanceof`**：检查对象是否是某个构造函数的实例（基于原型链）。
- **`Object.prototype.isPrototypeOf()`**：检查一个对象是否在另一个对象的原型链上。

javascript

复制

```
console.log(dog instanceof Dog); // true
console.log(Dog.prototype.isPrototypeOf(dog)); // true
```

#### 2. **操作原型**

- **`Object.create(proto)`**：创建一个新对象，并指定其原型。
- **`Object.setPrototypeOf(obj, proto)`**：设置对象的原型（性能较差，慎用）。
- **`Object.getPrototypeOf(obj)`**：获取对象的原型。

### **6、类继承与现代替代方案（ES6 Class）**

**如何实现“类式继承”？**

- **组合 `Object.create` 和构造函数**：

  ```js
  function Parent() {}
  function Child() {
    Parent.call(this); // 继承属性
  }
  Child.prototype = Object.create(Parent.prototype); // 继承方法
  Child.prototype.constructor = Child; // 修复构造函数指向
  ```

#### **1. 继承父类的实例属性**

```js
function Child() {
  Parent.call(this); // 关键代码
}
```

- **作用**：在子类构造函数中调用父类构造函数 (`Parent`)，将父类中通过 `this` 定义的**实例属性**复制到子类实例中。
- **原理**：
  - `Parent.call(this)` 将父类构造函数中的 `this` 绑定到当前子类实例（即 `new Child()` 创建的对象）。
  - 这一步实现了父类实例属性（如 `this.name`）的继承。

#### **2. 继承父类的方法**

```
Child.prototype = Object.create(Parent.prototype);
```

- **作用**：让子类 (`Child`) 的实例通过原型链继承父类 (`Parent`) 原型上的方法。
- **原理**：
  - `Object.create(Parent.prototype)` 创建一个新对象，其原型指向 `Parent.prototype`。
  - 将 `Child.prototype` 指向这个新对象，使得 `Child` 的实例能通过原型链访问父类原型的方法。

#### 3、现代替代方案

ES6 的 `class` 和 `extends` 语法糖底层正是基于寄生组合继承，无需手动处理原型链：

```js
class Parent {
  constructor(name) {
    this.name = name;
  }
  sayHello() {
    console.log(`Hello, I'm ${this.name}`);
  }
}

class Child extends Parent {
  constructor(name, age) {
    super(name); // 相当于 Parent.call(this, name)
    this.age = age;
  }
  sayAge() {
    console.log(`I'm ${this.age} years old`);
  }
}
```