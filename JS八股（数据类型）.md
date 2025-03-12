# JS八股（数据类型）

## 1、JavaScript有哪些数据类型，它们的区别？

JavaScript共有八种数据类型，分别是 Undefined、Null、Boolean、Number、String、Object、Symbol、BigInt。

其中 Symbol 和 BigInt 是ES6 中新增的数据类型：

- Symbol 代表创建后独一无二且不可变的数据类型，它主要是为了解决可能出现的全局变量冲突的问题。
- BigInt 是一种数字类型的数据，它可以表示任意精度格式的整数，使用 BigInt 可以安全地存储和操作大整数，即使这个数已经超出了 Number 能够表示的安全整数范围。

这些数据可以分为原始数据类型和引用数据类型：

- 栈：原始数据类型（Undefined、Null、Boolean、Number、String）
- 堆：引用数据类型（对象、数组和函数）

两种类型的区别在于**存储位置的不同：**

- 原始数据类型直接存储在栈（stack）中的简单数据段，占据空间小、大小固定，属于被频繁使用数据，所以放入栈中存储；
- 引用数据类型存储在堆（heap）中的对象，占据空间大、大小不固定。如果存储在栈中，将会影响程序运行的性能；引用数据类型在栈中存储了指针，该指针指向堆中该实体的起始地址。当解释器寻找引用值时，会首先检索其在栈中的地址，取得地址后从堆中获得实体。

### Symbol 类型

### **1. 核心特性**

- **唯一性**
  每个 `Symbol` 值都是唯一且不可变的，即使描述（description）相同，它们也不相等。

  ```
  const s1 = Symbol("key");
  const s2 = Symbol("key");
  console.log(s1 === s2); // false
  ```

- **不可实例化**
  不能通过 `new` 创建（与 `String`、`Number` 不同）：

  ```
  const s = new Symbol(); // TypeError: Symbol is not a constructor
  ```

- **非字符串属性键**
  可用作对象的属性键（避免命名冲突）：

  ```
  const obj = {
    [Symbol("id")]: 123,  // 唯一的属性键
    name: "Object"
  };
  ```

------

### **2. 主要用途**

#### **(1) 避免属性名冲突**

- **场景**：在扩展对象时，避免覆盖已有属性。

  ```
  const CACHE_KEY = Symbol("cache");
  const obj = {};
  obj[CACHE_KEY] = "内部数据"; // 外部无法直接访问
  ```

#### **(2) 内置 Symbol 值（Well-known Symbols）**

用于定义对象的内部行为（元编程），例如：

- **`Symbol.iterator`**：定义对象的迭代器。

  ```
  const iterable = {
    [Symbol.iterator]: function* () { yield 1; yield 2; }
  };
  console.log([...iterable]); // [1, 2]
  ```

- **`Symbol.toStringTag`**：自定义 `Object.prototype.toString` 的输出。

  ```
  class MyClass {
    get [Symbol.toStringTag]() { return "MyClass"; }
  }
  console.log(Object.prototype.toString.call(new MyClass())); // [object MyClass]
  ```

#### **(3) 私有属性模拟**

- **通过 Symbol 隐藏属性**（非严格私有，但外部难以直接访问）：

  ```
  const _private = Symbol("private");
  class MyClass {
    constructor() {
      this[_private] = "秘密数据";
    }
    getSecret() {
      return this[_private];
    }
  }
  ```

------

### **3. 注意事项**

- **不可枚举性**
  Symbol 属性默认不会被 `for...in`、`Object.keys()` 遍历，需用 `Object.getOwnPropertySymbols()` 获取。

- **全局注册表**
  通过 `Symbol.for(key)` 创建全局共享的 Symbol：

  ```
  const s1 = Symbol.for("global_key");
  const s2 = Symbol.for("global_key");
  console.log(s1 === s2); // true
  ```

------

### BigInt 类型

### **1. 核心特性**

- **表示大整数**
  用于处理超出 `Number` 安全范围（±2^53 -1）的整数。

  ```
  const bigNum = 9007199254740993n; // 超过 Number.MAX_SAFE_INTEGER(2^53 -1)
  ```

- **字面量后缀 `n`**
  直接书写 BigInt 时需在数字后加 `n`，或通过 `BigInt()` 函数转换：

  ```
  const a = 123n;          // BigInt 字面量
  const b = BigInt(456);   // 转换数值为 BigInt
  ```

------

### **2. 主要用途**

#### **(1) 高精度整数运算**

- **场景**：金融计算、科学计算、大数处理。

  ```
  // Number 类型精度丢失
  console.log(9007199254740992 === 9007199254740993); // true（超出安全范围）
  
  // BigInt 保持精度
  console.log(9007199254740992n === 9007199254740993n); // false
  ```

#### **(2) 兼容性处理**

- **与 Number 的互操作**
  BigInt 不能直接与 Number 混合运算，需显式转换：

  ```
  const sum = 1n + 2;      // TypeError: Cannot mix BigInt and other types
  const sumSafe = 1n + BigInt(2); // 正确：3n
  ```

------

### **3. 注意事项**

- **类型限制**
  BigInt 不支持小数（仅整数）和位运算（如 `>>>`）。

- **JSON 序列化**
  JSON 不支持 BigInt，需手动转换为字符串：

  ```
  const data = { value: 123n };
  JSON.stringify(data); // TypeError: Do not know how to serialize a BigInt
  // 解决方案：
  const safeData = { value: data.value.toString() };
  ```

## 2. 数据类型检测的方式有哪些

### 一、`typeof` 操作符

**用途**：检测基本数据类型（`undefined`、`boolean`、`number`、`string`、`symbol`、`bigint`）和 `function`。

**原理**：
`typeof` 是 JavaScript 的内置操作符，基于值的 **二进制类型标签**（在 V8 引擎中称为“类型标记”）判断类型。具体规则：

- 引擎在底层为每个值存储一个类型标记（如 `000` 表示对象，`1` 表示整数等）。
- `typeof` 根据这些标记快速返回类型字符串，**不涉及原型链查找**。

**关键细节**：

- `typeof null` 返回 `"object"`：历史遗留问题，早期规范中 `null` 的二进制标记被设计为 `000`（与对象相同）。
- 函数本质是对象，但 `typeof` 对其特殊处理，返回 `"function"`。

**示例**：

```js
console.log(typeof 42);         // "number"
console.log(typeof "hello");    // "string"
console.log(typeof true);       // "boolean"
console.log(typeof undefined);  // "undefined"
console.log(typeof Symbol());   // "symbol"
console.log(typeof 1n);         // "bigint"
console.log(typeof function(){}); // "function"

// 注意：typeof null 返回 "object"（历史遗留问题）
console.log(typeof null);       // "object"
```

**缺点**：

- **无法区分 `null` 和对象**（`typeof null` 返回 `"object"`）。
- **无法检测数组或其他对象的具体类型**（如 `typeof []` 返回 `"object"`）。

------

### 二、`instanceof` 操作符

**用途**：检测对象是否属于某个构造函数（基于原型链）。

**原理**：
检查对象的原型链中是否存在构造函数的 `prototype` 属性，**递归遍历原型链**。

- 执行 `A instanceof B` 时，引擎会执行：

  ```js
  A.__proto__ === B.prototype ? 
    true : 
    (A.__proto__ 继续向上查找，直到原型链顶端)
  ```

**关键细节**：

- 仅适用于对象（对原始类型无效）。

- 可能被手动修改原型链干扰：

  ```js
  const arr = [];
  arr.__proto__ = {}; 
  console.log(arr instanceof Array); // false（原型链被破坏）
  ```

**示例**：

```js
console.log([] instanceof Array);    // true
console.log({} instanceof Object);   // true
console.log(new Date() instanceof Date); // true

// 注意：无法检测原始类型（如字符串、数字）
console.log("hello" instanceof String); // false（因为原始类型不是对象）
```

**缺点**：

- **无法检测原始类型**（如 `string`、`number`）。

- **跨窗口/iframe 时失效**（不同全局环境中的同名构造函数不共享原型链）。

- **可能被手动修改原型链干扰**：

  ```js
  const arr = [];
  arr.__proto__ = Object.prototype;
  console.log(arr instanceof Array); // false（原型链被修改）
  ```

------

### 三、`Object.prototype.toString.call()`

**用途**：最通用的类型检测方法，返回 `[object Type]` 格式的字符串。

**原理**：
调用对象的内部方法 `[[Class]]`，返回 `[object Type]` 字符串。

- 规范定义：`Object.prototype.toString` 会读取对象的 `Symbol.toStringTag` 属性（若存在），否则返回内部 `[[Class]]` 值。
- **强制转换**：通过 `call()` 改变 `this` 指向，使任意值调用该方法。

**关键细节**：

- 精确区分所有类型（包括 `null` 和 `undefined`）。

- 自定义对象的 `Symbol.toStringTag` 可修改输出：

  ```js
  const obj = { [Symbol.toStringTag]: "MyClass" };
  console.log(Object.prototype.toString.call(obj)); // [object MyClass]
  ```

**示例**：

```js
console.log(Object.prototype.toString.call(42));        // [object Number]
console.log(Object.prototype.toString.call("hello"));    // [object String]
console.log(Object.prototype.toString.call(null));       // [object Null]
console.log(Object.prototype.toString.call(undefined)); // [object Undefined]
console.log(Object.prototype.toString.call([]));         // [object Array]
console.log(Object.prototype.toString.call({}));         // [object Object]
console.log(Object.prototype.toString.call(new Date())); // [object Date]
```

**优点**：

- **能准确区分所有数据类型**（包括 `null`、`undefined`、数组等）。
- **不受跨窗口环境影响**。

**用法封装**：

```js
function getType(obj) {
  return Object.prototype.toString.call(obj).slice(8, -1).toLowerCase();
}
console.log(getType([])); // "array"
console.log(getType(null)); // "null"
```

详细解释底层机制

1. **`[[Class]]` 内部属性**（ES5 及之前）

   - 在 ES5 及更早版本中，每个对象都有一个内部属性 `[[Class]]`，用于标识对象的类型（如 `"Array"`、`"Date"`、`"Object"` 等）。
   - `Object.prototype.toString` 方法通过访问 `[[Class]]` 的值，返回格式为 `[object [[Class]]]` 的字符串（例如 `[object Array]`）。

2. **`Symbol.toStringTag` 覆盖（ES6+）**

   - ES6 引入了 `Symbol.toStringTag` 符号属性，允许对象自定义 `Object.prototype.toString` 的输出结果。
   - 若对象定义了 `Symbol.toStringTag` 属性，`toString` 方法会优先使用它的值，而不是默认的 `[[Class]]`。

   javascript

   复制

   ```
   const obj = { [Symbol.toStringTag]: "MyCustomType" };
   console.log(Object.prototype.toString.call(obj)); // [object MyCustomType]
   ```

3. **强制转换上下文**

   - 直接调用 `Object.prototype.toString` 时，`this` 默认指向 `Object.prototype`，返回 `[object Object]`。
   - 通过 `call(value)` 改变 `this` 指向目标值，使方法能读取目标值的类型信息。

------

### 四、`Array.isArray()`

**用途**：专门检测数组类型（替代 `instanceof Array`）。

**原理**：
ES5 规范定义的数组检测方法，**直接检查值的内部 `[[Class]]` 标记**，而非依赖原型链。

- 底层实现类似：

  ```
  function isArray(arg) {
    return Object.prototype.toString.call(arg) === "[object Array]";
  }
  ```

- **跨窗口安全**：不依赖 `Array.prototype`，直接通过内部标记判断。

**示例**：

```js
console.log(Array.isArray([]));      // true
console.log(Array.isArray({}));      // false
console.log(Array.isArray("hello")); // false
```

**优点**：

- **跨窗口/iframe 时仍有效**（比 `instanceof` 更可靠）。

------

### 五、`constructor` 属性

**用途**：通过对象的 `constructor` 属性获取构造函数名称。

**原理**：
通过对象的 `constructor` 属性访问其**构造函数**。

- 默认情况下，对象的 `constructor` 指向创建它的构造函数。
- **原始类型**的 `constructor` 会被临时装箱为对象（如 `(42).constructor` 临时装箱为Number(42)返回 `Number`）。

**关键缺陷**：

- `constructor` 可能被手动修改：

  ```js
  function A() {}
  function B() {}
  const a = new A();
  a.constructor = B;
  console.log(a.constructor === B); // true（不可靠）
  ```

**示例**：

```js
const arr = [];
console.log(arr.constructor === Array); // true
console.log(arr.constructor.name);      // "Array"

const num = 42;
console.log(num.constructor === Number); // true（但原始类型会临时装箱为对象）
```

**缺点**：

- **不可靠**：`constructor` 可能被手动修改。
- **无法检测 `null` 和 `undefined`**（没有构造函数）。

------

### 总结：如何选择检测方式？

1. **基本类型（除 `null`）**：使用 `typeof`。
2. **检测 `null`**：用 `=== null`。
3. **检测数组**：优先用 `Array.isArray()`。
4. **通用类型检测**：用 `Object.prototype.toString.call()`。
5. **对象类型（构造函数）**：用 `instanceof`，但需注意跨窗口问题。
6. **避免使用 `constructor`**：除非明确知道对象未被篡改。

## 3、判断数组方法

### 判断数组的方式有哪些

- 通过Object.prototype.toString.call()做判断

```javascript
Object.prototype.toString.call(obj).slice(8,-1) === 'Array';
```

- 通过原型链做判断

```javascript
obj.__proto__ === Array.prototype;
```

- 通过ES6的Array.isArray()做判断

```javascript
Array.isArrray(obj);
```

- 通过instanceof做判断

```javascript
obj instanceof Array
```

- 通过Array.prototype.isPrototypeOf

```javascript
Array.prototype.isPrototypeOf(obj)
```

## 4、null和undefined区别

首先 Undefined 和 Null 都是基本数据类型，这两个基本数据类型分别都只有一个值，就是 undefined 和 null。

undefined 代表的含义是**未定义**，null 代表的含义是**空对象**。一般变量声明了但还没有定义的时候会返回 undefined，null主要用于赋值给一些可能会返回对象的变量，作为初始化。

undefined 在 JavaScript 中不是一个保留字，这意味着可以使用 undefined 来作为一个变量名，但是这样的做法是非常危险的，它会影响对 undefined 值的判断。我们可以通过一些方法获得安全的 undefined 值，比如说 void 0。

当对这两种类型使用 typeof 进行判断时，Null 类型化会返回 “object”，这是一个历史遗留的问题。当使用双等号对两种类型的值进行比较时会返回 true，使用三个等号时会返回 false。

**typeof null 的结果是Object。**

## 5、为什么0.1+0.2 ! == 0.3，如何让其相等  

在开发过程中遇到类似这样的问题：

```javascript
let n1 = 0.1, n2 = 0.2
console.log(n1 + n2)  // 0.30000000000000004
```

这里得到的不是想要的结果，要想等于0.3，就要把它进行转化：

```javascript
(n1 + n2).toFixed(2) // 注意，toFixed为四舍五入
```

`toFixed(num)` 方法可把 Number 四舍五入为指定小数位数的数字。

因为Number存储的都是双精度数，所以加起来并不严格等于0.3

## 6、typeof NaN 的结果是什么？

结果是number

NaN 指“不是一个数字”（not a number），NaN 是一个“警戒值”（sentinel value，有特殊用途的常规值），用于指出数字类型中的错误情况，即“执行数学运算没有成功，这是失败后返回的结果”。

```javascript
typeof NaN; // "number"
```

NaN 是一个特殊值，它和自身不相等，是唯一一个非自反（自反，reflexive，即 x === x 不成立）的值。而 NaN !== NaN 为 true。



## 7、isNaN 和 Number.isNaN 函数的区别？

- ### **1. `isNaN` 函数**

  **行为特点**：

  1. **强制类型转换**：`isNaN` 会先将参数转换为数值类型，再判断是否为 `NaN`。
  2. **宽松的检测逻辑**：若转换后的结果为 `NaN`，则返回 `true`；否则返回 `false`。
  3. **潜在问题**：可能误判非数值但可转换的输入（如字符串）。

  **示例**：

  ```
  isNaN(NaN);       // true（符合预期）
  isNaN("Hello");   // true（字符串转换为数值是 NaN）
  isNaN("123");     // false（字符串 "123" 转换为数值 123）
  isNaN(undefined); // true（undefined 转换为数值是 NaN）
  isNaN({});        // true（对象转换为数值是 NaN）
  ```

  ------

  ### **2. `Number.isNaN` 函数（ES6+）**

  **行为特点**：

  1. **无类型转换**：直接判断参数是否为 `NaN`，不进行类型转换。
  2. **严格的检测逻辑**：仅当参数是数值类型且值为 `NaN` 时，返回 `true`。
  3. **精确性**：避免误判非数值类型的输入。

  **示例**：

  ```
  Number.isNaN(NaN);       // true（仅数值类型 NaN 返回 true）
  Number.isNaN("Hello");   // false（非数值类型直接返回 false）
  Number.isNaN("123");     // false（字符串非数值类型）
  Number.isNaN(undefined); // false（undefined 非数值类型）
  Number.isNaN({});        // false（对象非数值类型）
  ```

## 8、如何获取安全的 undefined 值？

### **使用 `void 0`**

**原理**：
`void` 运算符会对任何表达式求值并始终返回 `undefined`，且无法被重写。
**示例**：

```js
const safeUndefined = void 0;

console.log(safeUndefined === undefined); // true（即使全局 undefined 被覆盖）
```

**优点**：

- 代码简洁，无副作用。
- 兼容所有环境（包括旧浏览器）。

**适用场景**：
需要直接获取安全的 `undefined` 值。

##  9、其他值到字符串的转换规则？

- Null 和 Undefined 类型 ，null 转换为 "null"，undefined 转换为 "undefined"，
- Boolean 类型，true 转换为 "true"，false 转换为 "false"。
- Number 类型的值直接转换，不过那些极小和极大的数字会使用指数形式。
- Symbol 类型的值直接转换，但是只允许显式强制类型转换，使用隐式强制类型转换会产生错误。
- 对普通对象来说，除非自行定义 toString() 方法，否则会调用 toString()（Object.prototype.toString()）来返回内部属性 [[Class]] 的值，如"[object Object]"。如果对象有自己的 toString() 方法，字符串化时就会调用该方法并使用其返回值。

## 10、 其他值到数字值的转换规则？

- Undefined 类型的值转换为 NaN。
- Null 类型的值转换为 0。
- Boolean 类型的值，true 转换为 1，false 转换为 0。
- String 类型的值转换如同使用 Number() 函数进行转换，如果包含非数字值则转换为 NaN，空字符串为 0。
- Symbol 类型的值不能转换为数字，会报错。
- 对象（包括数组）会首先被转换为相应的基本类型值，如果返回的是非数字的基本类型值，则再遵循以上规则将其强制转换为数字。

为了将值转换为相应的基本类型值，抽象操作 ToPrimitive 会首先（通过内部操作 DefaultValue）检查该值是否有valueOf()方法。如果有并且返回基本类型值，就使用该值进行强制类型转换。如果没有就使用 toString() 的返回值（如果存在）来进行强制类型转换。

如果 valueOf() 和 toString() 均不返回基本类型值，会产生 TypeError 错误。

## 11、 其他值到布尔类型的值的转换规则？

以下这些是假值： • undefined • null • false • +0、-0 和 NaN • ""

假值的布尔强制类型转换结果为 false。从逻辑上说，假值列表以外的都应该是真值。

## 12、 || 和 && 操作符的返回值？

|| 和 && 首先会对第一个操作数执行条件判断，如果其不是布尔值就先强制转换为布尔类型，然后再执行条件判断。

- 对于 || 来说，如果条件判断结果为 true 就返回第一个操作数的值，如果为 false 就返回第二个操作数的值。
- && 则相反，如果条件判断结果为 true 就返回第二个操作数的值，如果为 false 就返回第一个操作数的值。

|| 和 && 返回它们其中一个操作数的值，而非条件判断的结果

### 13、 Object.is() 与比较操作符 “===”、“==” 的区别？

- 使用双等号（==）进行相等判断时，如果两边的类型不一致，则会进行强制类型转化后再进行比较。
- 使用三等号（===）进行相等判断时，如果两边的类型不一致时，不会做强制类型准换，直接返回 false。
- 使用 Object.is 来进行相等判断时，一般情况下和三等号的判断相同，它处理了一些特殊的情况，比如 -0 和 +0 不再相等，两个 NaN 是相等的。

## 14、 什么是 JavaScript 中的包装类型？

在 JavaScript 中，基本类型是没有属性和方法的，但是为了便于操作基本类型的值，在调用基本类型的属性或方法时 JavaScript 会在后台隐式地将基本类型的值转换为对象，如：

```javascript
javascript 代码解读复制代码const a = "abc";
a.length; // 3
a.toUpperCase(); // "ABC"
```

在访问`'abc'.length`时，JavaScript 将`'abc'`在后台转换成`String('abc')`，然后再访问其`length`属性。

JavaScript也可以使用`Object`函数显式地将基本类型转换为包装类型：

```javascript
javascript 代码解读复制代码var a = 'abc'
Object(a) // String {"abc"}
```

也可以使用`valueOf`方法将包装类型倒转成基本类型：

```javascript
javascript 代码解读复制代码var a = 'abc'
var b = Object(a)
var c = b.valueOf() // 'abc'
```

看看如下代码会打印出什么：

```javascript
javascript 代码解读复制代码var a = new Boolean( false );
if (!a) {
	console.log( "Oops" ); // never runs
}
```

答案是什么都不会打印，因为虽然包裹的基本类型是`false`，但是`false`被包裹成包装类型后就成了对象，所以其非值为`false`，所以循环体中的内容不会运行。

## 15、javascript中的隐式转换

### 一、隐式转换的核心规则

#### 1. **ToPrimitive 规则**

对象转换为原始值时，会按优先级调用方法：

1. 优先调用 `valueOf()`（若返回原始值则停止）
2. 否则调用 `toString()`（若返回原始值则停止）
3. 否则报错（如 `Symbol` 类型无法转换）

**示例**：

```js
const obj = {
  valueOf() { return 42; },
  toString() { return "Hello"; }
};
console.log(obj + 1); // 42 + 1 = 43（优先 valueOf）
```

#### 2. **转换优先级**

- **数字运算**：优先转为 `Number`
- **字符串运算**：优先转为 `String`
- **逻辑判断**：转为 `Boolean`
- **宽松比较（`==`）**：按复杂规则转换

------

### 二、常见隐式转换场景

#### 1. **算术运算符（`+`, `-`, `\*`, `/`）**

- **`+` 运算符**：若存在字符串，转为字符串拼接；否则转为数字相加。

  ```js
  console.log(1 + "2");      // "12"（String 优先）
  console.log(1 + true);     // 2（true → 1）
  console.log(1 + null);     // 1（null → 0）
  console.log(1 + undefined);// NaN（undefined → NaN）
  ```

- **其他运算符**（`-`, `*`, `/`）：转为数字运算。

  ```js
  console.log("5" - 3);     // 2（"5" → 5）
  console.log("5" * "2");   // 10（转为数字）
  ```

#### 2. **逻辑判断（`if`, `&&`, `||`）**

所有值会被隐式转换为 `Boolean`：

- **Falsy 值**：`false`, `0`, `""`, `null`, `undefined`, `NaN`
- **Truthy 值**：其他所有值（包括对象、空数组 `[]`、空对象 `{}`）

**示例**：

```js
if ("") console.log("空字符串");   // 不执行（falsy）
if ([]) console.log("空数组");     // 执行（truthy）
```

#### 3. **宽松比较（`==`）**

按规则转换后比较，而 `===` 严格比较类型和值。

**转换规则**：

1. **对象 vs 非对象**：对象转为原始值（ToPrimitive）。
2. **字符串 vs 数字**：字符串转为数字。
3. **布尔值 vs 其他**：布尔值转为数字（`true→1`, `false→0`）。

**示例**：

```js
console.log([] == 0);       // true（[] → "" → 0）
console.log([] == "");      // true（[] → ""）
console.log(null == undefined); // true（特殊规则）
console.log("5" == 5);      // true（字符串转数字）
console.log({} == "[object Object]"); // true（对象转字符串）
```

#### 4. **对象参与的运算**

对象转换为原始值后参与运算：

```js
console.log([] + {});      // "" + "[object Object]" → "[object Object]"
console.log({} + []);      // 0（{} 被解析为代码块，+[] → 0）
```

#### 5. **模板字符串（`${}`）**

非字符串值自动转为字符串：

```js
console.log(`Value: ${123}`);    // "Value: 123"
console.log(`Value: ${{}}`);     // "Value: [object Object]"
```

------

### 三、隐式转换对照表

| 原始值/表达式 | 转为 Number | 转为 String         | 转为 Boolean |
| :------------ | :---------- | :------------------ | :----------- |
| `""`          | 0           | `""`                | `false`      |
| `"0"`         | 0           | `"0"`               | `true`       |
| `"123"`       | 123         | `"123"`             | `true`       |
| `"Hello"`     | NaN         | `"Hello"`           | `true`       |
| `true`        | 1           | `"true"`            | `true`       |
| `false`       | 0           | `"false"`           | `false`      |
| `null`        | 0           | `"null"`            | `false`      |
| `undefined`   | NaN         | `"undefined"`       | `false`      |
| `[]`          | 0 → "" → 0  | `""`                | `true`       |
| `[5]`         | 5           | `"5"`               | `true`       |
| `{}`          | NaN         | `"[object Object]"` | `true`       |

------

### 四、避免隐式转换的技巧

1. **使用严格比较（`===` 和 `!==`）**：

   ```js
   if (value === 0) { ... } // 避免 0 == false 的问题
   ```

2. **显式类型转换**：

   ```js
   const num = Number("123");  // 显式转为数字
   const str = String(123);    // 显式转为字符串
   ```

3. **谨慎处理对象转换**：

   ```js
   const obj = { valueOf: () => 42 };
   console.log(obj + 1); // 43（显式控制转换逻辑）
   ```

------

### 五、特殊案例解析

#### 1. `[] == ![]` 结果为 `true`

```js
// 步骤拆解：
// 1. 右侧 ![] → !true → false
// 2. 比较 [] == false
// 3. [] 转为原始值：[] → "" → 0
// 4. false → 0
// 结果：0 == 0 → true
```

#### 2. `{} + []` 结果为 `0`

```js
// 解析：{} 被当作空代码块，表达式变为 +[]
// +[] → +"" → 0
```

#### 3. `"foo" + +"bar"` 结果为 `"fooNaN"`

```js
// 步骤拆解：
// 1. +"bar" → NaN（字符串转数字失败）
// 2. "foo" + NaN → "fooNaN"
```