# JS八股（js基础）

## 1、new操作符的实现原理

### 一、`new` 操作符的作用

当使用 `new` 调用构造函数时，会发生以下事情：

1. **创建新对象**：创建一个空的普通对象。
2. **链接原型**：将该对象的原型（`__proto__`）指向构造函数的 `prototype` 属性。
3. **绑定 `this`**：执行构造函数，并将新对象作为 `this` 的上下文，从而将构造函数的原型对象中的属性绑定到新对象中。
4. **处理返回值**：若构造函数返回一个对象（非原始值），则返回该对象；否则返回新创建的对象。

------

### 二、手动实现 `new` 操作符

以下是一个模拟 `new` 的函数 `myNew`：

```js
function myNew(constructor, ...args) {
  // 1. 创建新对象，并链接到构造函数的原型
  const obj = Object.create(constructor.prototype);

  // 2. 执行构造函数，绑定 this 到新对象
  const result = constructor.apply(obj, args);

  // 3. 处理返回值：若构造函数返回对象，则返回它；否则返回新对象
  return result instanceof Object ? result : obj;
}
```

------

### 三、关键步骤详解

#### 1. 创建对象并链接原型

- 使用 `Object.create(constructor.prototype)` 创建新对象，直接继承构造函数的原型属性。
- 这一步等价于 `obj.__proto__ = constructor.prototype`，使得实例能访问构造函数原型上的方法。

#### 2. 执行构造函数

- 通过 `apply` 调用构造函数，将 `this` 指向新对象，并传递参数。
- 构造函数内部对 `this` 的属性赋值（如 `this.name = 'John'`）会应用到新对象。

#### 3. 处理返回值

- 若构造函数返回一个对象（如 `return { ... }`），则覆盖 `new` 默认行为，直接返回该对象。
- 若返回原始值（如 `return 42` 或没有 `return`），则忽略返回值，返回新对象。

------

### 四、示例分析

#### 示例 1：默认行为

```js
function Person(name) {
  this.name = name;
}
const p = myNew(Person, 'Alice');
console.log(p.name); // 输出 "Alice"
console.log(p instanceof Person); // true
```

- 创建 `Person` 实例，构造函数无显式返回值，返回新对象 `obj`。

#### 示例 2：构造函数返回对象

```js
function Dog() {
  return { bark: 'Woof!' };
}
const d = myNew(Dog);
console.log(d.bark); // 输出 "Woof!"
console.log(d instanceof Dog); // false
```

- 构造函数返回一个对象，`myNew` 直接返回该对象，因此 `d` 不是 `Dog` 的实例。

#### 示例 3：构造函数返回原始值

```js
function Cat() {
  return 'Meow';
}
const c = myNew(Cat);
console.log(c instanceof Cat); // true（返回的是新对象，而非 'Meow'）
```

- 构造函数返回字符串（原始值），`myNew` 忽略返回值，返回新对象。

## 2、map和Object的区别

### **1. 键（Key）的类型**

| **特性**     | **Map**                                      | **Object**                                     |
| :----------- | :------------------------------------------- | :--------------------------------------------- |
| **键的类型** | 允许**任意类型**的键（对象、函数、原始值等） | 键只能是**字符串**或 **Symbol**                |
| **自动转换** | 无类型转换，键保持原始类型                   | 非字符串键会被隐式转为字符串（如 `1` → `"1"`） |

**示例**：

javascript

复制

```
const map = new Map();
const objKey = { id: 1 };

map.set(objKey, "value"); // 支持对象作为键
console.log(map.get(objKey)); // "value"

const obj = {};
obj[objKey] = "value";
console.log(obj["[object Object]"]); // "value"（键被转为字符串）
```

------

### **2. 键的顺序**

| **特性**     | **Map**                   | **Object**                                           |
| :----------- | :------------------------ | :--------------------------------------------------- |
| **插入顺序** | 严格保留键的插入顺序      | ES6 后普通字符串键保留插入顺序，但数字键会按升序排序 |
| **迭代顺序** | `for...of` 按插入顺序遍历 | 需手动处理顺序（如 `Object.keys` 返回的数组）        |

**示例**：

javascript

复制

```
const map = new Map();
map.set(3, "three");
map.set(1, "one");
map.set(2, "two");
console.log([...map.keys()]); // [3, 1, 2]

const obj = { 3: "three", 1: "one", 2: "two" };
console.log(Object.keys(obj)); // ["1", "2", "3"]（数字键被排序）
```

------

### **3. 性能**

| **场景**     | **Map**                    | **Object**                                   |
| :----------- | :------------------------- | :------------------------------------------- |
| **频繁增删** | 针对高频增删优化，性能更佳 | 频繁增删可能导致性能下降（如 `delete` 操作） |
| **查找速度** | 查找键的时间复杂度为 O(1)  | 查找键的时间复杂度为 O(1)                    |

**适用场景**：

- **Map**：需要频繁增删键值对（如缓存、动态数据存储）。
- **Object**：键值对相对静态（如配置对象）。

------

### **4. 内置方法**

| **特性**     | **Map**                                            | **Object**                               |
| :----------- | :------------------------------------------------- | :--------------------------------------- |
| **操作方法** | 提供 `set`、`get`、`has`、`delete`、`clear` 等方法 | 通过属性赋值或 `delete` 操作符操作       |
| **迭代支持** | 原生支持 `for...of` 和 `forEach`                   | 需转换为数组（如 `Object.entries(obj)`） |
| **长度获取** | 直接通过 `size` 属性获取                           | 需计算 `Object.keys(obj).length`         |

**示例**：

javascript

复制

```
// Map 的方法
map.set("key", "value");
map.has("key"); // true
map.size; // 1

// Object 的方法
obj.key = "value";
"key" in obj; // true
Object.keys(obj).length; // 1
```

------

### **5. 默认键与原型链**

| **特性**     | **Map**                    | **Object**                                                   |
| :----------- | :------------------------- | :----------------------------------------------------------- |
| **原型链**   | 无默认键，避免原型污染风险 | 继承 `Object.prototype`，可能存在意外覆盖（如 `constructor`、`toString`） |
| **安全创建** | 无需额外操作               | 需通过 `Object.create(null)` 创建无原型的对象                |

**示例**：

javascript

复制

```
const map = new Map();
map.has("toString"); // false（无原型链干扰）

const obj = {};
obj.hasOwnProperty("toString"); // false，但可以调用 obj.toString()
const safeObj = Object.create(null);
safeObj.toString; // undefined
```

------

### **6. 序列化与兼容性**

| **特性**   | **Map**                              | **Object**               |
| :--------- | :----------------------------------- | :----------------------- |
| **序列化** | 无法直接通过 `JSON.stringify` 序列化 | 天然支持 JSON 序列化     |
| **兼容性** | ES6+ 环境支持                        | 所有 JavaScript 环境支持 |

**解决方案**：

javascript

复制

```
// Map 转为 JSON
const map = new Map([["key", "value"]]);
const mapJson = JSON.stringify([...map]);

// JSON 转为 Map
const restoredMap = new Map(JSON.parse(mapJson));
```

------

### **7. 内存占用**

- **Map**：针对动态键值对优化，存储大量数据时内存效率更高。
- **Object**：内存占用可能更高，尤其在键频繁变化时。

------

### **何时使用 Map？**

1. 键需要是**非字符串类型**（如对象、函数）。
2. 需要严格保留**插入顺序**。
3. 高频**增删键值对**（如缓存、动态数据集）。
4. 需要避免**原型污染**的风险。

### **何时使用 Object？**

1. 键是**静态的字符串或 Symbol**。
2. 需要与 JSON **无缝互操作**。
3. 使用**简单结构**（如配置对象、DTO）。
4. 依赖**广泛兼容性**（如旧版浏览器环境）。

------

### **总结**

| **特性**   | **Map**                     | **Object**                     |
| :--------- | :-------------------------- | :----------------------------- |
| **键类型** | 任意类型                    | 字符串或 Symbol                |
| **顺序**   | 严格保留插入顺序            | 字符串键按插入顺序，数字键排序 |
| **性能**   | 高频增删更优                | 静态数据更高效                 |
| **原型链** | 无默认键                    | 可能受原型污染                 |
| **序列化** | 需手动处理                  | 原生支持 JSON                  |
| **方法**   | 专用 API（`set`、`get` 等） | 属性操作和 `delete`            |

## 3. map和weakMap的区别

**（1）Map** map本质上就是键值对的集合，但是普通的Object中的键值对中的键只能是字符串。而ES6提供的Map数据结构类似于对象，但是它的键不限制范围，可以是任意类型，是一种更加完善的Hash结构。如果Map的键是一个原始数据类型，只要两个键严格相同，就视为是同一个键。

实际上Map是一个数组，它的每一个数据也都是一个数组，其形式如下：

```javascript
javascript 代码解读复制代码const map = [
     ["name","张三"],
     ["age",18],
]
```

Map数据结构有以下操作方法：

- **size**： `map.size` 返回Map结构的成员总数。
- **set(key,value)**：设置键名key对应的键值value，然后返回整个Map结构，如果key已经有值，则键值会被更新，否则就新生成该键。（因为返回的是当前Map对象，所以可以链式调用）
- **get(key)**：该方法读取key对应的键值，如果找不到key，返回undefined。
- **has(key)**：该方法返回一个布尔值，表示某个键是否在当前Map对象中。
- **delete(key)**：该方法删除某个键，返回true，如果删除失败，返回false。
- **clear()**：map.clear()清除所有成员，没有返回值。

Map结构原生提供是三个遍历器生成函数和一个遍历方法

- keys()：返回键名的遍历器。
- values()：返回键值的遍历器。
- entries()：返回所有成员的遍历器。
- forEach()：遍历Map的所有成员。

```javascript
javascript 代码解读复制代码const map = new Map([
     ["foo",1],
     ["bar",2],
])
for(let key of map.keys()){
    console.log(key);  // foo bar
}
for(let value of map.values()){
     console.log(value); // 1 2
}
for(let items of map.entries()){
    console.log(items);  // ["foo",1]  ["bar",2]
}
map.forEach( (value,key,map) => {
     console.log(key,value); // foo 1    bar 2
})
```

**（2）WeakMap** WeakMap 对象也是一组键值对的集合，其中的键是弱引用的。**其键必须是对象**，原始数据类型不能作为key值，而值可以是任意的。

该对象也有以下几种方法：

- **set(key,value)**：设置键名key对应的键值value，然后返回整个Map结构，如果key已经有值，则键值会被更新，否则就新生成该键。（因为返回的是当前Map对象，所以可以链式调用）
- **get(key)**：该方法读取key对应的键值，如果找不到key，返回undefined。
- **has(key)**：该方法返回一个布尔值，表示某个键是否在当前Map对象中。
- **delete(key)**：该方法删除某个键，返回true，如果删除失败，返回false。

其clear()方法已经被弃用，所以可以通过创建一个空的WeakMap并替换原对象来实现清除。

WeakMap的设计目的在于，有时想在某个对象上面存放一些数据，但是这会形成对于这个对象的引用。一旦不再需要这两个对象，就必须手动删除这个引用，否则垃圾回收机制就不会释放对象占用的内存。

而WeakMap的**键名所引用的对象都是弱引用**，即垃圾回收机制不将该引用考虑在内。因此，只要所引用的对象的其他引用都被清除，垃圾回收机制就会释放该对象所占用的内存。也就是说，一旦不再需要，WeakMap 里面的**键名对象和所对应的键值对会自动消失，不用手动删除引用**。

**总结：**

- Map 数据结构。它类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。
- WeakMap 结构与 Map 结构类似，也是用于生成键值对的集合。但是 WeakMap 只接受对象作为键名（ null 除外），不接受其他类型的值作为键名。而且 WeakMap 的键名所指向的对象，不计入垃圾回收机制。

## 4. JavaScript有哪些内置对象

全局的对象（ global objects ）或称标准内置对象，不要和 "全局对象（global object）" 混淆。这里说的全局的对象是说在 全局作用域里的对象。全局作用域中的其他对象可以由用户的脚本创建或由宿主程序提供。

**标准内置对象的分类：**

以下为常用内置对象

（1）值属性，这些全局属性返回一个简单值，这些值没有自己的属性和方法。例如 Infinity、NaN、undefined、null 字面量

（2）函数属性，全局函数可以直接调用，不需要在调用时指定所属对象，执行结束后会将结果直接返回给调用者。例如 eval()、parseFloat()、parseInt() 等

（3）基本对象，基本对象是定义或使用其他对象的基础。基本对象包括一般对象、函数对象和错误对象。例如 Object、Function、Boolean、Symbol、Error 等

（4）数字和日期对象，用来表示数字、日期和执行数学计算的对象。例如 Number、Math、Date

（5）字符串，用来表示和操作字符串的对象。例如 String、RegExp

（6）可索引的集合对象，这些对象表示按照索引值来排序的数据集合，包括**数组**和类型数组，以及类数组结构的对象。例如 Array

（7）使用键的集合对象，这些集合对象在存储数据时会使用到键，支持按照插入顺序来迭代元素。 例如 Map、Set、WeakMap、WeakSet

以下的不常用

（8）矢量集合，SIMD 矢量集合中的数据会被组织为一个数据序列。 例如 SIMD 等

（9）结构化数据，这些对象用来表示和操作结构化的缓冲区数据，或使用 JSON 编码的数据。例如 JSON 等

（10）控制抽象对象 例如 Promise、Generator 等

（11）反射。例如 Reflect、Proxy

（12）国际化，为了支持多语言处理而加入 ECMAScript 的对象。例如 Intl、Intl.Collator 等

（13）WebAssembly

（14）其他。例如 arguments

**总结：** js 中的内置对象主要指的是在程序执行前存在全局作用域里的由 js 定义的一些全局值属性、函数和用来实例化其他对象的构造函数对象。一般经常用到的如全局变量值 NaN、undefined，全局函数如 parseInt()、parseFloat() 用来实例化对象的构造函数如 Date、Object 等，还有提供数学计算的单体内置对象如 Math 对象。

## 5、对JSON的理解

JSON 是一种基于文本的轻量级的数据交换格式。它可以被任何的编程语言读取和作为数据格式来传递。

在项目开发中，使用 JSON 作为前后端数据交换的方式。在前端通过将一个符合 JSON 格式的数据结构序列化为 JSON 字符串，然后将它传递到后端，后端通过 JSON 格式的字符串解析后生成对应的数据结构，以此来实现前后端数据的一个传递。

因为 JSON 的语法是基于 js 的，因此很容易将 JSON 和 js 中的对象弄混，但是应该注意的是 JSON 和 js 中的对象不是一回事，JSON 中对象格式更加严格，比如说在 JSON 中属性值不能为函数，不能出现 NaN 这样的属性值等，因此大多数的 js 对象是不符合 JSON 对象的格式的。

在 js 中提供了两个函数来实现 js 数据结构和 JSON 格式的转换处理，

- JSON.stringify 函数，通过传入一个符合 JSON 格式的数据结构，将其转换为一个 JSON 字符串。如果传入的数据结构不符合 JSON 格式，那么在序列化的时候会对这些值进行对应的特殊处理，使其符合规范。在前端向后端发送数据时，可以调用这个函数将数据对象转化为 JSON 格式的字符串。
- JSON.parse() 函数，这个函数用来将 JSON 格式的字符串转换为一个 js 数据结构，如果传入的字符串不是标准的 JSON 格式的字符串的话，将会抛出错误。当从后端接收到 JSON 格式的字符串时，可以通过这个方法来将其解析为一个 js 数据结构，以此来进行数据的访问。

### 6. JavaScript 伪数组对象的定义？

一个拥有 length 属性和若干索引属性的对象就可以被称为伪数组对象，伪数组对象和数组类似，但是**不能调用数组的方法**。常见的伪数组对象有 arguments 和 DOM 方法的返回结果，还有一个函数也可以被看作是伪数组对象，因为它含有 length 属性值，代表可接收的参数个数。

常见的类数组转换为数组的方法有这样几种：

（1）通过 call 调用数组的 slice 方法来实现转换

```javascript
Array.prototype.slice.call(arrayLike);
```

（2）通过 call 调用数组的 splice 方法来实现转换

```javascript
Array.prototype.splice.call(arrayLike, 0);
```

（3）通过 apply 调用数组的 concat 方法来实现转换

```javascript
Array.prototype.concat.apply([], arrayLike);
```

（4）通过 Array.from 方法来实现转换

```javascript
Array.from(arrayLike);
```

## 7、数组方法有哪些

**参见数据结构资料中的数组常见方法**

## 8、 常见的位运算符有哪些？其计算规则是什么？

现代计算机中数据都是以二进制的形式存储的，即0、1两种状态，计算机对二进制数据进行的运算加减乘除等都是叫位运算，即将符号位共同参与运算的运算。

常见的位运算有以下几种：

| 运算符 | 描述 | 运算规则                                                 |                          |
| ------ | ---- | -------------------------------------------------------- | ------------------------ |
| `&`    | 与   | 两个位都为1时，结果才为1                                 |                          |
| `      | `    | 或                                                       | 两个位都为0时，结果才为0 |
| `^`    | 异或 | 两个位相同为0，相异为1                                   |                          |
| `~`    | 取反 | 0变1，1变0                                               |                          |
| `<<`   | 左移 | 各二进制位全部左移若干位，高位丢弃，低位补0              |                          |
| `>>`   | 右移 | 各二进制位全部右移若干位，正数左补0，负数左补1，右边丢弃 |                          |

#### 1. 按位与运算符（&）

**定义：** 参加运算的两个数据**按二进制位**进行“与”运算。 **运算规则：**

```javascript
javascript 代码解读复制代码0 & 0 = 0  
0 & 1 = 0  
1 & 0 = 0  
1 & 1 = 1
```

总结：两位同时为1，结果才为1，否则结果为0。 例如：3&5 即：

```javascript
javascript 代码解读复制代码0000 0011 
   0000 0101 
 = 0000 0001
```

因此 3&5 的值为1。 注意：负数按补码形式参加按位与运算。

**用途：**

**（1）判断奇偶**

只要根据最未位是0还是1来决定，为0就是偶数，为1就是奇数。因此可以用`if ((i & 1) == 0)`代替`if (i % 2 == 0)`来判断a是不是偶数。

**（2）清零**

如果想将一个单元清零，即使其全部二进制位为0，只要与一个各位都为零的数值相与，结果为零。

#### 2. 按位或运算符（|）

**定义：** 参加运算的两个对象按二进制位进行“或”运算。

**运算规则：**

```javascript
javascript 代码解读复制代码0 | 0 = 0
0 | 1 = 1  
1 | 0 = 1  
1 | 1 = 1
```

总结：参加运算的两个对象只要有一个为1，其值为1。 例如：3|5即：

```javascript
javascript 代码解读复制代码0000 0011
  0000 0101 
= 0000 0111
```

因此，3|5的值为7。 注意：负数按补码形式参加按位或运算。

#### 3. 异或运算符（^）

**定义：** 参加运算的两个数据按二进制位进行“异或”运算。

**运算规则：**

```javascript
javascript 代码解读复制代码0 ^ 0 = 0  
0 ^ 1 = 1  
1 ^ 0 = 1  
1 ^ 1 = 0
```

总结：参加运算的两个对象，如果两个相应位相同为0，相异为1。 例如：3|5即：

```javascript
javascript 代码解读复制代码0000 0011
  0000 0101 
= 0000 0110
```

因此，3^5的值为6。 异或运算的性质:

- 交换律：`(a^b)^c == a^(b^c)`
- 结合律：`(a + b)^c == a^b + b^c`
- 对于任何数x，都有 `x^x=0，x^0=x`
- 自反性: `a^b^b=a^0=a`;

#### 4. 取反运算符 (~)

**定义：** 参加运算的一个数据按二进制进行“取反”运算。

**运算规则：**

```javascript
javascript

 代码解读
复制代码~ 1 = 0~ 0 = 1
```

总结：对一个二进制数按位取反，即将0变1，1变0。 例如：~6 即：

```javascript
javascript

 代码解读
复制代码0000 0110= 1111 1001
```

在计算机中，正数用原码表示，负数使用补码存储，首先看最高位，最高位1表示负数，0表示正数。此计算机二进制码为负数，最高位为符号位。 当发现按位取反为负数时，就**直接取其补码**，变为十进制：

```javascript
javascript

 代码解读
复制代码0000 0110   = 1111 1001反码：1000 0110补码：1000 0111
```

因此，~6的值为-7。

#### 5. 左移运算符（<<）

**定义：** 将一个运算对象的各二进制位全部左移若干位，左边的二进制位丢弃，右边补0。 设 a=1010 1110，a = a<< 2 将a的二进制位左移2位、右补0，即得a=1011 1000。 若左移时舍弃的高位不包含1，则每左移一位，相当于该数乘以2。

#### 6. 右移运算符（>>）

**定义：** 将一个数的各二进制位全部右移若干位，正数左补0，负数左补1，右边丢弃。 例如：a=a>>2 将a的二进制位右移2位，左补0 或者 左补1得看被移数是正还是负。 操作数每右移一位，相当于该数除以2。

#### 7. 原码、补码、反码

上面提到了补码、反码等知识，这里就补充一下。 计算机中的**有符号数**有三种表示方法，即原码、反码和补码。三种表示方法均有符号位和数值位两部分，符号位都是用0表示“正”，用1表示“负”，而数值位，三种表示方法各不相同。

**（1）原码**

原码就是一个数的二进制数。例如：10的原码为0000 1010

**（2）反码**

- 正数的反码与原码相同，如：10 反码为 0000 1010
- 负数的反码为除符号位，按位取反，即0变1，1变0。

例如：-10

```javascript
javascript 代码解读复制代码原码：1000 1010
反码：1111 0101
```

**（3）补码**

- 正数的补码与原码相同，如：10 补码为 0000 1010
- 负数的补码是原码除符号位外的所有位取反即0变1，1变0，然后加1，也就是反码加1。

例如：-10

```javascript
javascript 代码解读复制代码原码：1000 1010
反码：1111 0101
补码：1111 0110
```

## 9、JavaScript脚本延迟加载的方式有哪些？

### **1. `async` 属性**

- **作用**：异步加载脚本，**下载完成立即执行**，不阻塞 HTML 解析。

- **特点**：

  - 脚本**不保证执行顺序**（谁先下载完谁先执行）。
  - 适合**独立脚本**（如统计代码、广告脚本）。

- **代码示例**：

  ```
  <script async src="script.js"></script>
  ```

------

### **2. `defer` 属性**

- **作用**：延迟加载脚本，**在 HTML 解析完成后、DOMContentLoaded 事件前按顺序执行**。

- **特点**：

  - 脚本**按文档中的顺序执行**。
  - 适合**依赖 DOM 或需要顺序执行的脚本**。

- **代码示例**：

  ```
  <script defer src="script.js"></script>
  ```

------

### **3. 动态创建 Script 标签**

- **作用**：通过 JavaScript 动态插入脚本，完全控制加载时机。

- **特点**：

  - 不阻塞页面渲染。
  - 可监听脚本加载完成事件（如 `onload`）。
  - 适合**按需加载或条件加载**的脚本。

- **代码示例**：

  ```
  function loadScript(src) {
    const script = document.createElement('script');
    script.src = src;
    script.async = true; // 可选，异步加载
    document.body.appendChild(script);
  }
  // 在需要时调用
  loadScript('script.js');
  ```

------

### **4. `setTimeout` 或 `requestIdleCallback`**

- **作用**：手动延迟脚本加载或执行。

- **特点**：

  - `setTimeout`：简单但不够精确。
  - `requestIdleCallback`：在浏览器空闲时执行（兼容性需注意）。

- **代码示例**：

  ```
  // 使用 setTimeout
  setTimeout(() => {
    const script = document.createElement('script');
    script.src = 'script.js';
    document.body.appendChild(script);
  }, 3000); // 延迟 3 秒加载
  
  // 使用 requestIdleCallback
  if ('requestIdleCallback' in window) {
    window.requestIdleCallback(() => {
      loadScript('script.js');
    });
  }
  ```

------

### **5. 脚本放在页面底部**

- **作用**：将 `<script>` 标签放在 `</body>` 前，减少渲染阻塞。

- **特点**：

  - 非真正延迟加载，但能缩短首次渲染时间。
  - 适用于无需延迟但希望减少阻塞的脚本。

- **代码示例**：

  ```
  <body>
    <!-- 页面内容 -->
    <script src="script.js"></script>
  </body>
  ```

------

### **6. 模块化按需加载（ES6 Modules + Webpack）**

- **作用**：利用现代打包工具（如 Webpack）实现代码分割和动态导入。

- **特点**：

  - 按需加载，减少初始包体积。
  - 结合 `import()` 语法动态加载模块。

- **代码示例**：

  ```
  // 用户点击按钮时加载脚本
  button.addEventListener('click', async () => {
    const module = await import('./module.js');
    module.run();
  });
  ```

## 10.  **ES6**模块与**CommonJS**模块有什么异同？

- ES6模块与CommonJS模块是JavaScript中两种主要的模块系统，它们在使用场景、语法和运行机制上有显著差异。以下是它们的异同点总结：

  ### **相同点**

  1. **模块化目标**：都旨在将代码分割为独立的模块，便于组织和复用。
  2. **导出/导入功能**：支持通过导出（`export`/`module.exports`）和导入（`import`/`require`）实现模块间的依赖管理。

  ------

  ### **不同点**

  | **特性**         | **ES6 模块**                                    | **CommonJS 模块**                              |
  | :--------------- | :---------------------------------------------- | :--------------------------------------------- |
  | **语法**         | 使用 `import` 和 `export` 关键字。              | 使用 `require()` 和 `module.exports`。         |
  | **加载方式**     | 异步加载（浏览器环境），支持静态分析。          | 同步加载（Node.js 环境）。                     |
  | **值的传递**     | 导出的是值的**引用**（动态绑定）。              | 导出的是值的**拷贝**（静态快照）。             |
  | **执行时机**     | 预处理阶段解析依赖，代码执行时初始化。          | 模块在 `require()` 时立即执行。                |
  | **静态/动态**    | 静态结构（导入/导出必须在顶层，不可动态嵌套）。 | 动态结构（`require()` 可在代码任意位置调用）。 |
  | **循环依赖处理** | 通过引用处理，支持未完成模块的绑定。            | 可能返回未完全初始化的模块副本。               |
  | **顶层 `this`**  | 指向 `undefined`（严格模式）。                  | 指向当前模块的 `exports` 对象。                |
  | **严格模式**     | 默认启用，无需声明 `"use strict"`。             | 需显式声明 `"use strict"`。                    |
  | **动态导入**     | 支持 `import()` 函数（返回 `Promise`）。        | 通过 `require()` 动态加载（同步）。            |
  | **主要环境**     | 现代浏览器和 Node.js（需配置）。                | Node.js 传统方案，浏览器需打包工具支持。       |

  ------

  ### **关键差异详解**

  1. **值引用 vs 值拷贝**

     - **ES6**：导出的是值的引用，导入模块会实时反映原始值的修改。

       ```js
       // counter.mjs
       export let count = 0;
       export const increment = () => count++;
       
       // main.mjs
       import { count, increment } from './counter.mjs';
       console.log(count); // 0
       increment();
       console.log(count); // 1（值同步更新）
       ```

     - **CommonJS**：导出的是值的拷贝，后续修改不影响已导入的值。

       ```js
       // counter.js
       let count = 0;
       module.exports = { count, increment: () => count++ };
       
       // main.js
       const { count, increment } = require('./counter');
       console.log(count); // 0
       increment();
       console.log(count); // 0（拷贝值未变）
       ```

  2. **循环依赖处理**

     - **ES6**：通过引用绑定，模块未完成初始化时也能正确解析依赖。
     - **CommonJS**：可能导致未完全初始化的模块被引入（如 `a.js` 和 `b.js` 互相依赖时）。

  3. **静态分析与优化**

     - **ES6**：静态结构支持 Tree-shaking（删除未使用代码）。
     - **CommonJS**：动态特性难以静态优化。

## 11、什么是 DOM 和 BOM？

- **DOM**：操作网页内容的接口，关注 **“页面里有什么”**。
- **BOM**：操作浏览器窗口的接口，关注 **“浏览器能做什么”**。

**实际开发中**：

- 通过 DOM 实现交互功能（如表单验证、动态渲染）。
- 通过 BOM 控制浏览器行为（如页面跳转、历史管理）。

### **一、DOM（Document Object Model，文档对象模型）**

#### **定义**

DOM 是 **对 HTML/XML 文档的编程接口**，它将网页内容抽象为一个树状结构（节点树），允许开发者通过 JavaScript 动态访问和操作页面元素。

#### **核心特点**

1. **树状结构**：
   - 每个 HTML 标签、属性和文本都是一个 **节点（Node）**，构成层级分明的树形结构。
   - 示例：`<html>` 是根节点，包含 `<head>` 和 `<body>` 子节点。
2. **标准化**：
   - 由 W3C 组织制定标准，主流浏览器遵循统一的 DOM 规范（如 DOM Level 3）。
   - 不同浏览器对 DOM 的实现可能存在细微差异，但核心功能一致。
3. **操作页面内容**：
   - 增删改查 HTML 元素、属性、样式、事件等。
   - 示例：修改文本、添加子元素、绑定点击事件。

#### **常见 API**

- **查询元素**：

  ```
  document.getElementById('id');       // 通过 ID 获取元素
  document.querySelector('.class');   // 通过 CSS 选择器获取元素
  ```

- **操作元素**：

  ```
  element.textContent = '新内容';     // 修改文本
  element.setAttribute('href', '#'); // 修改属性
  element.style.color = 'red';       // 修改样式
  ```

- **事件处理**：

  ```
  button.addEventListener('click', () => {
    console.log('按钮被点击');
  });
  ```

------

### **二、BOM（Browser Object Model，浏览器对象模型）**

#### **定义**

BOM 是 **对浏览器窗口的编程接口**，它提供了一组对象，用于操作浏览器行为（如导航、窗口尺寸、历史记录等）。

#### **核心特点**

1. **无统一标准**：
   - BOM 没有官方标准，不同浏览器的实现可能不同，但主要对象（如 `window`）已被广泛支持。
2. **控制浏览器行为**：
   - 管理窗口、导航、浏览器信息、定时器等。
   - 示例：跳转页面、获取屏幕尺寸、设置定时任务。
3. **顶级对象 `window`**：
   - BOM 的所有对象都是 `window` 的属性和方法。
   - 在浏览器中，全局变量和函数实际上是 `window` 的成员。

#### **常见对象与 API**

| 对象/属性        | 说明                                         |
| :--------------- | :------------------------------------------- |
| **`window`**     | 浏览器窗口的顶级对象，代表当前标签页或窗口。 |
| **`navigator`**  | 提供浏览器信息（如名称、版本、操作系统）。   |
| **`location`**   | 操作当前页面的 URL（跳转、获取参数等）。     |
| **`history`**    | 管理浏览历史记录（前进、后退、跳转）。       |
| **`screen`**     | 获取屏幕信息（分辨率、可用宽高等）。         |
| **`setTimeout`** | 设置定时器。                                 |

#### **示例代码**

```
// 跳转到新页面
window.location.href = 'https://www.example.com';

// 获取浏览器信息
console.log(navigator.userAgent); // 输出浏览器 User-Agent

// 设置定时任务
setTimeout(() => {
  alert('3秒后弹出');
}, 3000);
```

## 12、js中的变量提升与问题

### **一、变量提升的设计原因**

1. **简化编译阶段的处理**
   JavaScript 引擎在执行代码前会进行**预编译**（创建执行上下文），此时会快速扫描代码中的**变量声明**（`var`、`function` 等）并提前分配内存。这种设计使得引擎无需逐行解析代码再处理声明，提升了执行效率。

2. **支持函数间的相互调用**
   在早期的 JavaScript 中，函数可以**在任何位置声明**，并允许在声明前被调用。例如：

   ```js
   foo(); // 正确执行，因为函数声明被提升
   function foo() { console.log("Hello"); }
   ```

   这种灵活性使得代码组织更自由，尤其是处理递归或相互调用的情况。

3. **历史设计决策**
   JavaScript 早期（由 Brendan Eich 在 10 天内设计）借鉴了其他语言（如 Scheme）的特性，但为了简化实现，采用了一种“先执行，后优化”的设计思路，变量提升是这一思路的产物。

------

### **二、变量提升的具体表现**

1. **变量声明提升**
   `var` 声明的变量会被提升到作用域顶部，但**赋值未被提升**，此时变量值为 `undefined`：

   ```js
   console.log(a); // undefined（而非报错）
   var a = 10;
   ```

2. **函数声明提升**
   函数声明整体被提升，因此可以在声明前调用：

   ```js
   foo(); // "Hello"
   function foo() { console.log("Hello"); }
   ```

3. **函数表达式不提升**
   使用 `var` 赋值的函数表达式仅提升变量名，函数体未被提升：

   ```js
   bar(); // TypeError: bar is not a function
   var bar = function() { console.log("World"); };
   ```

------

### **三、变量提升导致的问题**

1. **意外的变量覆盖**
   同一作用域内重复声明变量可能引发逻辑错误：

   ```js
   var x = 1;
   function test() {
     console.log(x); // undefined（块内 var x 被提升）
     var x = 2;
   }
   test();
   ```

2. **暂时性死区（TDZ）的缺失（ES6 前）**
   `var` 允许在声明前访问（值为 `undefined`），导致难以追踪的 Bug：

   ```js
   if (true) {
     console.log(tmp); // undefined（未抛出错误）
     var tmp = 5;
   }
   ```

3. **循环中的闭包问题**
   变量提升导致循环中异步操作引用同一变量：

   ```js
   for (var i = 0; i < 3; i++) {
     setTimeout(() => console.log(i)); // 输出 3, 3, 3
   }
   // 因 var i 被提升至全局作用域，同步先执行，i更新到3。异步setTimeout再执行，i都是3
   ```

4. **函数声明覆盖**
   同一作用域内函数声明可能被后续声明覆盖：

   ```js
   foo(); // "New"
   function foo() { console.log("Old"); }
   function foo() { console.log("New"); }
   ```

------

### **四、ES6 的改进方案**

为了规避变量提升的问题，ES6 引入了 `let`/`const` 和块级作用域：

1. **块级作用域**
   `let`/`const` 声明的变量仅在块内有效，避免变量污染：

   ```js
   if (true) {
     let x = 10;
   }
   console.log(x); // ReferenceError: x is not defined
   ```

2. **暂时性死区（TDZ）**
   变量在声明前不可访问，强制先声明后使用：

   ```js
   console.log(y); // ReferenceError: Cannot access 'y' before initialization
   let y = 20;
   ```

3. **禁止重复声明**
   同一作用域内 `let`/`const` 不允许重复声明：

   ```js
   let z = 1;
   let z = 2; // SyntaxError: Identifier 'z' has already been declared
   ```

4. **函数表达式的推荐使用**
   优先使用 `const` 声明函数，避免函数声明提升的副作用：

   ```js
   const func = () => { /* ... */ };
   ```

## 13、常见的DOM操作有哪些

### **一、获取元素**

1. **按 ID 获取**

   ```
   const element = document.getElementById('id');
   ```

2. **按类名获取**

   ```
   const elements = document.getElementsByClassName('class');
   ```

3. **按标签名获取**

   ```
   const elements = document.getElementsByTagName('div');
   ```

4. **按 CSS 选择器获取**

   ```
   const element = document.querySelector('.class'); // 首个匹配元素
   const elements = document.querySelectorAll('div'); // 所有匹配元素
   ```

5. **遍历关系获取**

   ```
   const parent = element.parentNode; // 父节点
   const children = element.children; // 直接子元素集合
   const firstChild = element.firstElementChild; // 第一个子元素
   const nextSibling = element.nextElementSibling; // 下一个兄弟元素
   ```

------

### **二、修改内容**

1. **修改 HTML 内容**

   ```
   element.innerHTML = '<strong>新内容</strong>'; // 解析 HTML
   element.textContent = '纯文本内容'; // 仅文本（性能更好）
   element.innerText = '可见文本'; // 考虑样式（如隐藏内容不显示）
   ```

2. **修改表单值**

   ```
   inputElement.value = '新值';
   ```

------

### **三、创建与插入元素**

1. **创建元素**

   ```
   const newDiv = document.createElement('div'); // 创建元素
   const textNode = document.createTextNode('文本节点'); // 创建文本节点
   ```

2. **插入元素**

   ```
   parent.appendChild(newDiv); // 末尾插入
   parent.insertBefore(newDiv, referenceElement); // 指定位置插入
   parent.replaceChild(newDiv, oldElement); // 替换子元素
   ```

3. **插入 HTML 字符串**

   ```
   element.insertAdjacentHTML('beforeend', '<p>插入内容</p>'); 
   // 参数位置：'beforebegin', 'afterbegin', 'beforeend', 'afterend'
   ```

------

### **四、删除元素**

1. **移除子元素**

   ```
   parent.removeChild(childElement);
   ```

2. **直接移除自身**

   ```js
   element.remove(); // 现代方法（IE 不支持）
   ```

------

### **五、操作属性**

1. **通用属性操作**

   ```js
   element.setAttribute('data-id', '123'); // 设置属性
   const value = element.getAttribute('data-id'); // 获取属性
   element.removeAttribute('data-id'); // 删除属性
   ```

2. **直接访问标准属性**

   ```js
   element.id = 'newId';
   element.className = 'class1 class2'; // 覆盖类名
   element.href = 'https://example.com';
   ```

3. **操作类名（推荐 `classList`）**

   ```js
   element.classList.add('active'); // 添加类
   element.classList.remove('active'); // 移除类
   element.classList.toggle('active'); // 切换类
   element.classList.contains('active'); // 检查是否存在类
   ```

------

### **六、样式操作**

1. **内联样式修改**

   ```js
   element.style.color = 'red'; // 单个属性
   element.style.cssText = 'color: red; font-size: 16px;'; // 批量设置
   ```

2. **获取计算样式**

   ```js
   const styles = window.getComputedStyle(element);
   const fontSize = styles.getPropertyValue('font-size');
   ```

------

### **七、事件处理**

1. **添加事件监听**

   ```js
   element.addEventListener('click', (event) => {
     console.log('点击事件', event.target);
   });
   ```

2. **移除事件监听**

   ```js
   const handler = () => { /* ... */ };
   element.addEventListener('click', handler);
   element.removeEventListener('click', handler); // 需绑定相同函数引用
   ```

3. **阻止默认行为或冒泡**

   ```js
   event.preventDefault(); // 阻止默认行为（如表单提交）
   event.stopPropagation(); // 阻止事件冒泡
   ```

------

### **八、其他常用操作**

1. **获取元素尺寸与位置**

   ```js
   const width = element.offsetWidth; // 包含边框的宽度
   const height = element.clientHeight; // 内容高度（不含边框）
   const rect = element.getBoundingClientRect(); // 返回位置和尺寸对象
   ```

2. **表单操作**

   ```js
   formElement.submit(); // 提交表单
   formElement.reset(); // 重置表单
   ```

## 14、use strict 严格模式

### **一、如何启用严格模式？**

在脚本或函数作用域顶部添加 `'use strict';` 即可启用：

javascript

复制

```
// 整个脚本文件启用（需在文件首行）
'use strict';
let x = 10;

// 单个函数内启用
function strictFunc() {
  'use strict';
  // 函数内部代码按严格模式执行
}
```

------

### **二、严格模式的核心变化**

#### **1. 变量必须声明后使用**

- **非严格模式**：未声明的变量赋值会隐式创建全局变量。

- **严格模式**：直接报错（`ReferenceError`）。

  javascript

  复制

  ```
  'use strict';
  x = 10; // ReferenceError: x is not defined
  ```

#### **2. 禁止删除不可删除的属性**

- **非严格模式**：删除变量、函数或 `configurable: false` 的属性会静默失败。

- **严格模式**：直接报错（`SyntaxError` 或 `TypeError`）。

  javascript

  复制

  ```
  'use strict';
  let y = 20;
  delete y; // SyntaxError: Delete of an unqualified identifier in strict mode
  ```

#### **3. 函数参数不可重名**

- **非严格模式**：允许函数参数同名，后定义的覆盖前者。

- **严格模式**：报错（`SyntaxError`）。

  javascript

  复制

  ```
  'use strict';
  function sum(a, a) { // SyntaxError: Duplicate parameter name not allowed
    return a + a;
  }
  ```

#### **4. 禁止使用 `with` 语句**

- **非严格模式**：允许使用 `with`（可能导致作用域混乱）。

- **严格模式**：报错（`SyntaxError`）。

  javascript

  复制

  ```
  'use strict';
  with (Math) { // SyntaxError: Strict mode code may not include a with statement
    console.log(PI);
  }
  ```

#### **5. `this` 指向的调整**

- **非严格模式**：全局函数中的 `this` 指向 `window`。

- **严格模式**：全局函数中的 `this` 为 `undefined`。

  javascript

  复制

  ```
  'use strict';
  function testThis() {
    console.log(this); // undefined
  }
  testThis();
  ```

#### **6. 禁止八进制字面量**

- **非严格模式**：允许 `var num = 0123;`（八进制写法）。

- **严格模式**：报错（`SyntaxError`）。需改用 `0o123`。

  javascript

  复制

  ```
  'use strict';
  let num = 0123; // SyntaxError: Octal literals are not allowed
  ```

#### **7. `eval` 的独立作用域**

- **非严格模式**：`eval` 内部声明的变量会污染外部作用域。

- **严格模式**：`eval` 内部变量仅在 `eval` 作用域内有效。

  javascript

  复制

  ```
  'use strict';
  eval('let z = 30;');
  console.log(z); // ReferenceError: z is not defined
  ```

#### **8. 禁止对只读属性赋值**

- **非严格模式**：对只读属性（如 `arguments.callee`）赋值会静默失败。

- **严格模式**：报错（`TypeError`）。

  javascript

  复制

  ```
  'use strict';
  Object.defineProperty({}, 'readOnlyProp', { value: 1, writable: false });
  obj.readOnlyProp = 2; // TypeError: Cannot assign to read only property
  ```

------

### **三、严格模式的优势**

1. **减少隐蔽错误**：如变量未声明、意外全局变量等。
2. **优化代码性能**：限制动态特性（如 `eval`、`with`），方便引擎优化。
3. **增强安全性**：防止 `this` 意外指向全局对象。
4. **未来兼容性**：提前适应未来可能废弃的语法（如八进制字面量）。

------

### **四、严格模式的注意事项**

1. **合并代码的风险**：严格模式作用于整个脚本或函数作用域，需避免与非严格模式代码混用。
2. **旧版浏览器兼容性**：ES5+ 环境支持严格模式，但老旧浏览器（如 IE9 以下）可能报错。
3. **模块和类默认启用**：ES6 的模块（`import/export`）和类（`class`）自动启用严格模式。

## 15、如何判断一个对象是否属于某个类？

- 第一种方式，使用 instanceof 运算符来判断构造函数的 prototype 属性是否出现在对象的原型链中的任何位置。
- 第二种方式，通过对象的 constructor 属性来判断，对象的 constructor 属性指向该对象的构造函数，但是这种方式不是很安全，因为 constructor 属性可以被改写。
- 第三种方式，如果需要判断的是某个内置的引用类型的话，可以使用 Object.prototype.toString() 方法来打印对象的[[Class]] 属性来进行判断。

## 16、for...in和for...of的区别

for…of 是ES6新增的遍历方式，允许遍历一个含有iterator接口的数据结构（数组、对象等）并且返回各项的值，和ES3中的for…in的区别如下

- for…of 遍历获取的是对象的**键值**，for…in 获取的是对象的**键名**；
- for… in 会遍历对象的整个原型链，性能非常差不推荐使用，而 for … of 只遍历当前对象不会遍历原型链；
- 对于数组的遍历，for…in 会返回数组中所有可枚举的属性(包括原型链上可枚举的属性)，for…of 只返回数组的下标对应的属性值；

**总结：** for...in 循环主要是为了遍历对象而生，不适用于遍历数组；for...of 循环可以用来遍历数组、类数组对象，字符串、Set、Map 以及 Generator 对象。

### forEach和map方法有什么区别

这方法都是用来遍历数组的，两者区别如下：

- forEach()方法会针对每一个元素执行提供的函数，对数据的操作会改变原数组，该方法没有返回值；
- map()方法不会改变原数组的值，返回一个新数组，新数组中的值为原数组调用函数处理之后的值；

## 17、for...in和for...of和forEach比较

### **一、核心方法对比**

| **特性**     | **`for...in`**                     | **`for...of`**                    | **`forEach`**                  |
| :----------- | :--------------------------------- | :-------------------------------- | :----------------------------- |
| **遍历目标** | **对象的可枚举属性**（包括原型链） | **可迭代对象的值**（数组、Map等） | **数组元素**（仅限于数组方法） |
| **返回值**   | 键（key）或索引（字符串形式）      | 值（value）                       | 无返回值（仅执行回调）         |
| **可中断性** | 支持 `break`、`return`             | 支持 `break`、`return`            | **不可中断**（无法跳出循环）   |
| **遍历顺序** | 不保证顺序（对象属性无序）         | 按迭代器顺序（数组等有序结构）    | 按数组索引顺序                 |
| **兼容性**   | 所有浏览器                         | ES6+                              | ES5+（数组方法）               |
| **适用场景** | 遍历对象属性                       | 遍历集合类数据结构的值            | 数组遍历且无需中断             |

------

### **二、详解与示例**

#### **1. `for...in`**

- **用途**：遍历对象的**可枚举属性**（包括原型链上的属性）。

- **示例**：

  ```
  const obj = { a: 1, b: 2 };
  for (const key in obj) {
    console.log(key); // 'a', 'b'
  }
  
  // 需注意原型链属性
  function Parent() { this.parentProp = 5; }
  function Child() { this.childProp = 10; }
  Child.prototype = new Parent();
  const child = new Child();
  for (const key in child) {
    if (child.hasOwnProperty(key)) { // 过滤自身属性
      console.log(key); // 'childProp'
    }
  }
  ```

#### **2. `for...of`**

- **用途**：遍历 **可迭代对象**（实现了 `Symbol.iterator` 接口的对象）的值。

- **支持的数据类型**：数组、字符串、Map、Set、`arguments`、NodeList 等。

- **示例**：

  ```
  const arr = [10, 20, 30];
  for (const value of arr) {
    console.log(value); // 10, 20, 30
  }
  
  const map = new Map([[1, 'a'], [2, 'b']]);
  for (const [key, value] of map) {
    console.log(key, value); // 1 'a', 2 'b'
  }
  ```

#### **3. `forEach`**

- **用途**：数组的遍历方法，对每个元素执行回调函数。

- **特性**：

  - 无法使用 `break` 或 `return` 终止循环（除非抛异常）。
  - 回调函数的参数为 `(value, index, array)`。

- **示例**：

  ```
  [1, 2, 3].forEach((value, index) => {
    console.log(value, index); // 1 0, 2 1, 3 2
  });
  ```

------

### **三、其他 `for` 相关语法**

#### **1. 传统 `for` 循环**

- **用途**：通过索引控制循环次数，适用于精确控制遍历过程。

- **示例**：

  ```
  for (let i = 0; i < 5; i++) {
    console.log(i); // 0, 1, 2, 3, 4
  }
  ```

#### **2. `for await...of`（ES2018）**

- **用途**：遍历 **异步可迭代对象**（如异步生成器、Promise 数组）。

- **示例**：

  ```
  async function* asyncGenerator() {
    yield await Promise.resolve(1);
    yield await Promise.resolve(2);
  }
  
  (async () => {
    for await (const value of asyncGenerator()) {
      console.log(value); // 1, 2
    }
  })();
  ```

------

### **四、常见误区与注意事项**

#### **1. `for...in` 遍历数组的问题**

- 遍历数组时会返回**字符串类型的索引**，且可能包含非数字键（如手动添加的属性）：

  ```
  const arr = [10, 20];
  arr.foo = 'bar';
  for (const key in arr) {
    console.log(key); // '0', '1', 'foo'（非预期结果）
  }
  ```

#### **2. `forEach` 无法中断**

- 使用 `return` 只能跳过当前迭代，无法终止整个循环：

  ```
  [1, 2, 3].forEach((num) => {
    if (num === 2) return; // 仅跳过 2，继续执行 3
    console.log(num); // 1, 3
  });
  ```

#### **3. `for...of` 不适用于普通对象**

- 普通对象默认不可迭代，需手动实现 `Symbol.iterator` 或转换为可迭代结构（如 `Object.entries`）：

  ```
  const obj = { a: 1, b: 2 };
  // 错误用法：for (const value of obj) { ... }
  // 正确用法：
  for (const [key, value] of Object.entries(obj)) {
    console.log(key, value); // 'a' 1, 'b' 2
  }
  ```

## 18、interator 迭代器

### **一、迭代器的核心概念**

#### **1. 迭代器协议（Iterator Protocol）**

任何对象满足以下条件即为迭代器：

- 必须实现一个 `next()` 方法。
- `next()` 方法返回一个对象，包含两个属性：
  - `value`：当前迭代的值（任意类型）。
  - `done`：布尔值，表示迭代是否完成（`true` 表示结束）。

**示例：手动创建迭代器**

```
const simpleIterator = {
  data: [1, 2, 3],
  index: 0,
  next() {
    return this.index < this.data.length
      ? { value: this.data[this.index++], done: false }
      : { value: undefined, done: true };
  }
};

console.log(simpleIterator.next()); // { value: 1, done: false }
console.log(simpleIterator.next()); // { value: 2, done: false }
console.log(simpleIterator.next()); // { value: 3, done: false }
console.log(simpleIterator.next()); // { value: undefined, done: true }
```

#### **2. 可迭代协议（Iterable Protocol）**

一个对象若想被 `for...of` 循环遍历，必须实现 **`Symbol.iterator`** 方法：

- 该方法返回一个**迭代器对象**（遵循迭代器协议）。
- 内置可迭代对象：数组、字符串、Map、Set、`arguments`、NodeList 等。

**示例：数组的可迭代性**

```
const arr = [10, 20, 30];
const arrIterator = arr[Symbol.iterator](); // 获取数组的迭代器

console.log(arrIterator.next()); // { value: 10, done: false }
console.log(arrIterator.next()); // { value: 20, done: false }
console.log(arrIterator.next()); // { value: 30, done: false }
console.log(arrIterator.next()); // { value: undefined, done: true }
```

------

### **二、迭代器的使用场景**

#### **1. `for...of` 循环**

直接遍历可迭代对象的值：

```
for (const num of [1, 2, 3]) {
  console.log(num); // 1 → 2 → 3
}
```

#### **2. 解构赋值**

利用迭代器提取值：

```
const [a, b] = new Set([10, 20]);
console.log(a, b); // 10 20
```

#### **3. 扩展运算符（`...`）**

将可迭代对象展开为数组：

```
const str = 'hello';
console.log([...str]); // ['h', 'e', 'l', 'l', 'o']
```

#### **4. 接受可迭代对象的 API**

如 `Array.from()`、`Promise.all()` 等：

```
const map = new Map([[1, 'a'], [2, 'b']]);
console.log(Array.from(map)); // [[1, 'a'], [2, 'b']]
```

------

### **三、自定义可迭代对象**

#### **1. 实现 `Symbol.iterator` 方法**

创建可被 `for...of` 遍历的自定义对象：

```
const range = {
  start: 1,
  end: 5,
  [Symbol.iterator]() {
    let current = this.start;
    return {
      next: () => {
        return current <= this.end
          ? { value: current++, done: false }
          : { done: true };
      }
    };
  }
};

for (const num of range) {
  console.log(num); // 1 → 2 → 3 → 4 → 5
}
```

#### **2. 生成器函数（Generator Function）**

用 `function*` 简化迭代器创建：

```
function* generateRange(start, end) {
  for (let i = start; i <= end; i++) {
    yield i; // 每次调用 next() 时暂停并返回 i
  }
}

const gen = generateRange(1, 3);
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: undefined, done: true }
```

------

### **四、迭代器的底层原理**

#### **1. 迭代过程解析**

当使用 `for...of` 时，引擎会：

1. 调用对象的 `Symbol.iterator` 方法获取迭代器。
2. 循环调用迭代器的 `next()` 方法，直到 `done` 为 `true`。

#### **2. 迭代器的状态性**

- 迭代器是**有状态的**，一旦遍历完成无法重置。
- 若需重新遍历，需重新调用 `Symbol.iterator` 获取新迭代器。
