# ES6八股

## 1. let、const、var的区别

**（1）块级作用域：** 块作用域由 `{ }`包括，let和const具有块级作用域，var不存在块级作用域。块级作用域解决了ES5中的两个问题：

- 内层变量可能覆盖外层变量
- 用来计数的循环变量泄露为全局变量

**（2）变量提升：** var存在变量提升，let和const不存在变量提升，即在变量只能在声明之后使用，否在会报错。

**（3）给全局添加属性：** 浏览器的全局对象是window，Node的全局对象是global。var声明的变量为全局变量，并且会将该变量添加为全局对象的属性，但是let和const不会。

**（4）重复声明：** var声明变量时，可以重复声明变量，后声明的同名变量会覆盖之前声明的遍历。const和let不允许重复声明变量。

**（5）暂时性死区：** 在使用let、const命令声明变量之前，该变量都是不可用的。这在语法上，称为**暂时性死区**。使用var声明的变量不存在暂时性死区(因为有变量提升)。

**（6）初始值设置：** 在变量声明时，var 和 let 可以不用设置初始值。而const声明变量必须设置初始值。

**（7）指针指向：** let和const都是ES6新增的用于创建变量的语法。 let创建的变量是可以更改指针指向（可以重新赋值）。但const声明的变量是不允许改变指针的指向。

## 2. const对象的属性可以修改吗

原始数据类型不可以，引用数据类型内部的属性可以改变

const保证的并不是变量的值不能改动，而是变量指向的那个**内存地址不能改动**。对于基本类型的数据（数值、字符串、布尔值），其值就**保存**在变量指向的那个内存地址，内存地址不能改变，因此等同于常量。

但对于引用类型的数据（主要是对象和数组）来说，变量指向数据的内存地址，保存的只是一个指针，const只能保证这个**指针是固定不变**的，至于它指向的数据结构是不是可变的，就完全不能控制了。

如果希望对象完全不可变，需要使用 `Object.freeze()`

## 3. 如果new一个箭头函数的会怎么样

箭头函数是ES6中的提出来的，它没有prototype，也没有自己的this指向，更不可以使用arguments参数，所以不能New一个箭头函数。

new操作符的实现步骤如下：

1. 创建一个对象
2. 将构造函数的作用域赋给新对象（也就是将对象的__proto__属性指向构造函数的prototype属性）
3. 指向构造函数中的代码，构造函数中的this指向该对象（也就是为这个对象添加属性和方法）
4. 返回新的对象

所以，上面的第二、三步，箭头函数都是没有办法执行的。

## 4、普通函数和箭头函数的区别

在 JavaScript 中，普通函数（通过 `function` 关键字定义）和箭头函数（通过 `=>` 语法定义）有显著的区别，主要体现在 **语法、行为特性** 和 **适用场景** 上。以下是它们的核心区别：

------

### 1. **`this` 的绑定方式**

- **普通函数**：

  - 有自己的 `this`，其值由 **调用方式** 决定：
    - 直接调用时，`this` 指向全局对象（非严格模式）或 `undefined`（严格模式）。
    - 作为对象方法调用时，`this` 指向对象本身。
    - 通过 `new` 调用时，`this` 指向新创建的实例。

  ```
  const obj = {
    value: 10,
    normalFunc: function() {
      console.log(this.value); // 输出 10（this 指向 obj）
    }
  };
  obj.normalFunc();
  ```

- **箭头函数**：

  - **没有自己的 `this`**，`this` 的值继承自 **外层作用域**（词法作用域）。
  - 无法通过 `call`、`apply`、`bind` 改变 `this`。

  ```
  const obj = {
    value: 10,
    arrowFunc: () => {
      console.log(this.value); // 输出 undefined（this 指向全局对象）
    }
  };
  obj.arrowFunc();
  ```

------

### 2. **构造函数能力**

- **普通函数**：

  - 可以通过 `new` 关键字作为构造函数调用，创建实例对象。

  ```
  function Person(name) {
    this.name = name;
  }
  const alice = new Person("Alice"); // ✅ 正常
  ```

- **箭头函数**：

  - **不能作为构造函数**，使用 `new` 调用会报错。

  ```
  const Person = (name) => { this.name = name; };
  const bob = new Person("Bob"); // ❌ TypeError: Person is not a constructor
  ```

------

### 3. **`arguments` 对象**

- **普通函数**：

  - 内部可以访问 `arguments` 对象，包含所有传入的参数。

  ```
  function sum() {
    return Array.from(arguments).reduce((a, b) => a + b);
  }
  sum(1, 2, 3); // 输出 6
  ```

- **箭头函数**：

  - **没有自己的 `arguments` 对象**，但可以访问外层函数的 `arguments`。
  - 需使用剩余参数（...args）替代。

  ```
  const sum = (...args) => args.reduce((a, b) => a + b);
  sum(1, 2, 3); // 输出 6
  ```

------

### 4. **原型对象（`prototype`）**

- **普通函数**：

  - 有 `prototype` 属性，用于实现原型继承。

  ```
  function Animal() {}
  console.log(Animal.prototype); // 输出原型对象
  ```

- **箭头函数**：

  - **没有 `prototype` 属性**，无法用于原型链继承。

  ```
  const Animal = () => {};
  console.log(Animal.prototype); // 输出 undefined
  ```

------

### 5. **语法差异**

- **普通函数**：

  - 完整的函数声明语法，适合复杂逻辑。

  ```
  function multiply(a, b) {
    return a * b;
  }
  ```

- **箭头函数**：

  - 语法简洁，适合简单表达式或回调函数。
  - 如果函数体是单行表达式，可省略 `return` 和大括号。

  ```
  const multiply = (a, b) => a * b;
  ```

------

### 6. **适用场景**

- **普通函数**：
  - 需要动态 `this` 的场景（如对象方法、构造函数）。
  - 需要使用 `arguments` 的场景。
  - 需要作为生成器函数（`function*`）。
- **箭头函数**：
  - 需要固定 `this` 的场景（如回调函数、事件监听）。
  - 需要简洁语法的场景（如数组方法 `map`、`filter`）。
  - 避免意外修改 `this` 的场景。

------

### 总结对比表

| **特性**         | **普通函数**               | **箭头函数**               |
| :--------------- | :------------------------- | :------------------------- |
| `this` 绑定      | 动态绑定（由调用方式决定） | 静态绑定（继承外层作用域） |
| 构造函数能力     | ✅ 支持 `new`               | ❌ 不支持 `new`             |
| `arguments` 对象 | ✅ 有                       | ❌ 无（需用剩余参数）       |
| `prototype` 属性 | ✅ 有                       | ❌ 无                       |
| 语法简洁性       | 一般                       | 更简洁（适合单行表达式）   |

## 5. 箭头函数的**this**指向哪⾥？

箭头函数不同于传统JavaScript中的函数，箭头函数并没有属于⾃⼰的this，它所谓的this是捕获其所在上下⽂的 this 值，作为⾃⼰的 this 值，并且由于没有属于⾃⼰的this，所以是不会被new调⽤的，这个所谓的this也不会被改变。

可以⽤Babel理解⼀下箭头函数:

```javascript
javascript 代码解读复制代码// ES6 
const obj = { 
  getArrow() { 
    return () => { 
      console.log(this === obj); 
    }; 
  } 
}
```

转化后：

```javascript
javascript 代码解读复制代码// ES5，由 Babel 转译
var obj = { 
   getArrow: function getArrow() { 
     var _this = this; 
     return function () { 
        console.log(_this === obj); 
     }; 
   } 
};
```

## 6. 扩展运算符的作用及使用场景

**（1）对象扩展运算符**

对象的扩展运算符(...)用于取出参数对象中的所有可遍历属性，拷贝到当前对象之中。

```javascript
let bar = { a: 1, b: 2 };
let baz = { ...bar }; // { a: 1, b: 2 }
```

上述方法实际上等价于:

```javascript
let bar = { a: 1, b: 2 };
let baz = Object.assign({}, bar); // { a: 1, b: 2 }
```

`Object.assign`方法用于对象的合并，将源对象`（source）`的所有可枚举属性，复制到目标对象`（target）`。`Object.assign`方法的第一个参数是目标对象，后面的参数都是源对象。(**如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性**)。

同样，如果用户自定义的属性，放在扩展运算符后面，则扩展运算符内部的同名属性会被覆盖掉。

```javascript
let bar = {a: 1, b: 2};
let baz = {...bar, ...{a:2, b: 4}};  // {a: 2, b: 4}
```

利用上述特性就可以很方便的修改对象的部分属性。在`redux`中的`reducer`函数规定必须是**一个纯函数**，`reducer`中的`state`对象要求不能直接修改，可以通过扩展运算符把修改路径的对象都复制一遍，然后产生一个新的对象返回。

需要注意：**扩展运算符对对象实例的拷贝属于浅拷贝**。

**（2）数组扩展运算符**

数组的扩展运算符可以将一个数组转为用逗号分隔的参数序列，且每次只能展开一层数组。

```javascript
console.log(...[1, 2, 3])
// 1 2 3
console.log(...[1, [2, 3, 4], 5])
// 1 [2, 3, 4] 5
```

下面是数组的扩展运算符的应用：

- **将数组转换为参数序列**

```javascript
function add(x, y) {
  return x + y;
}
const numbers = [1, 2];
add(...numbers) // 3
```

- **复制数组**

```javascript
const arr1 = [1, 2];
const arr2 = [...arr1];
```

要记住：**扩展运算符(…)用于取出参数对象中的所有可遍历属性，拷贝到当前对象之中**，这里参数对象是个数组，数组里面的所有对象都是基础数据类型，将所有基础数据类型重新拷贝到新的数组中。

- **合并数组**

如果想在数组内合并数组，可以这样：

```javascript
const arr1 = ['two', 'three'];
const arr2 = ['one', ...arr1, 'four', 'five'];// ["one", "two", "three", "four", "five"]
```

- **扩展运算符与解构赋值结合起来，用于生成数组**

```javascript
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest  // [2, 3, 4, 5]
```

需要注意：**如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。**

```javascript
const [...rest, last] = [1, 2, 3, 4, 5];         // 报错
const [first, ...rest, last] = [1, 2, 3, 4, 5];  // 报错
```

- **将字符串转为真正的数组**

```javascript
[...'hello']    // [ "h", "e", "l", "l", "o" ]
```

- **任何 Iterator 接口的对象，都可以用扩展运算符转为真正的数组**

比较常见的应用是可以将某些数据结构转为数组：

```javascript
// arguments对象
function foo() {
  const args = [...arguments];
}
```

用于替换`es5`中的`Array.prototype.slice.call(arguments)`写法。

- **使用**`Math`**函数获取数组中特定的值**

```javascript
const numbers = [9, 4, 7, 1];
Math.min(...numbers); // 1
Math.max(...numbers); // 9
```

## 7. 对对象与数组的解构的理解

解构是 ES6 提供的一种新的提取数据的模式，这种模式能够从对象或数组里有针对性地拿到想要的数值。

 **1）数组的解构** 在解构数组时，以元素的位置为匹配条件来提取想要的数据的：

```javascript
const [a, b, c] = [1, 2, 3]
```

： ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e55fc36b191340e69698782fbd67ef4f~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

最终，a、b、c分别被赋予了数组第0、1、2个索引位的值， 数组里的0、1、2索引位的元素值，精准地被映射到了左侧的第0、1、2个变量里去，这就是数组解构的工作模式。还可以通过给左侧变量数组设置空占位的方式，实现对数组中某几个元素的精准提取：

```javascript
const [a,,c] = [1,2,3]
```

通过把中间位留空，可以顺利地把数组第一位和最后一位的值赋给 a、c 两个变量： ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a14ffbb3df2646a4a84f4a0c7d62d975~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

**2）对象的解构** 对象解构比数组结构稍微复杂一些，也更显强大。在解构对象时，是以属性的名称为匹配条件，来提取想要的数据的。现在定义一个对象：

```javascript
const stu = {
  name: 'Bob',
  age: 24
}
```

假如想要解构它的两个自有属性，可以这样：

```javascript
const { name, age } = stu
```

 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1ed2565845f2415b8243c8c355b2c6d6~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp)

这样就得到了 name 和 age 两个和 stu 平级的变量：注意，对象解构严格以属性名作为定位依据，所以就算调换了 name 和 age 的位置，结果也是一样的：

```javascript
const { age, name } = stu
```

## 8、**如何提取高度嵌套的对象里的指定属性？**

有时会遇到一些嵌套程度非常深的对象：

```javascript
const school = {
   classes: {
      stu: {
         name: 'Bob',
         age: 24,
      }
   }
}
```

像此处的 name 这个变量，嵌套了四层，此时如果仍然尝试老方法来提取它：

```javascript
const { name } = school
```

显然是不奏效的，因为 school 这个对象本身是没有 name 这个属性的，name 位于 school 对象的“儿子的儿子”对象里面。要想把 name 提取出来，一种比较笨的方法是逐层解构：

```javascript
const { classes } = school
const { stu } = classes
const { name } = stu
name // 'Bob'
```

但是还有一种更标准的做法，可以用一行代码来解决这个问题：

```javascript
const { classes: { stu: { name } }} = school
       
console.log(name)  // 'Bob'
```

可以在解构出来的变量名右侧，通过冒号+{目标属性名}这种形式，进一步解构它，一直解构到拿到目标数据为止。

## 9. 对 rest 参数（...args）的理解

扩展运算符被用在函数形参上时，**它还可以把一个分离的参数序列整合成一个数组**：

```javascript
function mutiple(...args) {
  let result = 1;
  for (var val of args) {
    result *= val;
  }
  return result;
}
mutiple(1, 2, 3, 4) // 24
```

这里，传入 mutiple 的是四个分离的参数，但是如果在 mutiple 函数里尝试输出 args 的值，会发现它是一个数组：

```javascript
function mutiple(...args) {
  console.log(args)
}
mutiple(1, 2, 3, 4) // [1, 2, 3, 4]
```

这就是 … rest运算符的又一层威力了，它可以把函数的多个入参收敛进一个数组里。这一点**经常用于获取函数的多余参数，或者像上面这样处理函数参数个数不确定的情况。**

## 10. ES6中模板语法与字符串处理

ES6 提出了“模板语法”的概念。在 ES6 以前，拼接字符串是很麻烦的事情：

```javascript
var name = 'css'   
var career = 'coder' 
var hobby = ['coding', 'writing']
var finalString = 'my name is ' + name + ', I work as a ' + career + ', I love ' + hobby[0] + ' and ' + hobby[1]
```

仅仅几个变量，写了这么多加号，还要时刻小心里面的空格和标点符号有没有跟错地方。但是有了模板字符串，拼接难度直线下降：

```javascript
var name = 'css'   
var career = 'coder' 
var hobby = ['coding', 'writing']
var finalString = `my name is ${name}, I work as a ${career} I love ${hobby[0]} and ${hobby[1]}`
```

字符串不仅更容易拼了，也更易读了，代码整体的质量都变高了。这就是模板字符串的第一个优势——允许用${}的方式嵌入变量。但这还不是问题的关键，模板字符串的关键优势有两个：

- 在模板字符串中，空格、缩进、换行都会被保留
- 模板字符串完全支持“运算”式的表达式，可以在${}里完成一些计算

基于第一点，可以在模板字符串里无障碍地直接写 html 代码：

```javascript
let list = `
	<ul>
		<li>列表项1</li>
		<li>列表项2</li>
	</ul>
`;
console.log(message); // 正确输出，不存在报错
```

基于第二点，可以把一些简单的计算和调用丢进 ${} 来做：

```javascript
function add(a, b) {
  const finalString = `${a} + ${b} = ${a+b}`
  console.log(finalString)
}
add(1, 2) // 输出 '1 + 2 = 3'
```

除了模板语法外， ES6中还新增了一系列的字符串方法用于提升开发效率：

（1）**存在性判定**：在过去，当判断一个字符/字符串是否在某字符串中时，只能用 indexOf > -1 来做。现在 ES6 提供了三个方法：includes、startsWith、endsWith，它们都会返回一个布尔值来告诉你是否存在。

- **includes**：判断字符串与子串的包含关系：

```javascript
const son = 'haha' 
const father = 'xixi haha hehe'
father.includes(son) // true
```

- **startsWith**：判断字符串是否以某个/某串字符开头：

```javascript
const father = 'xixi haha hehe'
father.startsWith('haha') // false
father.startsWith('xixi') // true
```

- **endsWith**：判断字符串是否以某个/某串字符结尾：

```javascript
const father = 'xixi haha hehe'
father.endsWith('hehe') // true
```

（2）**自动重复**：可以使用 repeat 方法来使同一个字符串输出多次（被连续复制多次）：

```javascript
const sourceCode = 'repeat for 3 times;'
const repeated = sourceCode.repeat(3) 
console.log(repeated) // repeat for 3 times;repeat for 3 times;repeat for 3 time
```