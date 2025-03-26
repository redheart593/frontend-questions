# React八股

# 基础知识

## 1. 什么是 React？它的主要特点是什么？

**React** 是一个用于构建用户界面的 JavaScript 库，由 Facebook 开发并维护。它主要用于构建单页应用程序（SPA）和复杂的用户界面。React 的主要特点包括：

- **组件化**：React 将 UI 分解成独立的、可重用的组件。每个组件都有自己的逻辑和控制。
- **虚拟 DOM**：React 使用虚拟 DOM 来提高性能。虚拟 DOM 是一个内存中的树结构，React 会先在虚拟 DOM 中进行操作，然后批量更新真实 DOM。
- **声明式编程**：React 采用声明式编程风格，开发者只需描述 UI 应该是什么样的，React 会负责处理 UI 的变化。
- **JSX**：React 使用 JSX（JavaScript XML）语法，允许在 JavaScript 中编写类似 HTML 的标记。
- **生态系统丰富**：React 拥有丰富的生态系统，包括路由器（React Router）、状态管理库（Redux、MobX）等。

## 2、react虚拟DOM的原理

**虚拟 DOM** 是一个轻量级的内存中的 DOM 树表示。React 使用虚拟 DOM 来提高性能，避免频繁的操作真实 DOM。

React通过比较虚拟DOM的差异，计算出需要更新的部分，然后再将这些部分更新到真实DOM上。



React虚拟DOM的原理是：

1. 首先，**初始化**时React将组件的状态和属性传入组件的render方法，得到一个**虚拟DOM树**。
2. 当组件的**状态或属性发生变化**时，React会再次调用render方法得到**新的虚拟DOM树**。
3. React会将新旧两棵虚拟DOM树进行**比较**，得到它们的**不同之处**，**最小化的 DOM 操作指令**。
4. React会将这些**不同之处记录**下来，然后批量的**更新到真实的DOM树**上，**批量更新**：将多次 DOM 操作合并，减少浏览器重排/重绘次数。

React通过虚拟DOM树的比较，**避免**了直接操作真实DOM树带来的**性能问题**，因为直接操作真实DOM树会带来大量的**重排**和**重绘**，而React的虚拟DOM树的比较和更新是基于JavaScript对象进行的，不会导致页面的重排和重绘。比较后再**批量更新**真实DOM，大量减少浏览器重排重绘。

总结起来，React虚拟DOM的原理就是：通过比较虚拟DOM树的不同，批量的更新真实的DOM树，从而提高页面的性能。



为什么需要虚拟DOM

1、框架设计原因：render函数无法绑定更新的数据，会全量生成渲染dom，开销比较高；

2、跨平台 



## 3、Diff算法

### **一、Diff 算法的基本原则**

1. **同层比较**：仅对比同一层级的节点，不跨层级深度遍历。
2. **类型优先**：若节点类型不同，直接替换整个子树，跳过子节点对比。
3. **Key 优化列表**：通过 `key` 标识列表项，减少节点位置变化时的低效操作。

### **二、同级比较如何影响子节点对比？**

#### 1. **父节点类型不同 → 不比较子节点**

- 若父节点类型不同（如旧树是 `<div>`，新树是 `<span>`），直接销毁旧子树，创建新子树。
- **子节点不会被对比**，因为父节点已不同，子树结构大概率完全变化。

#### 2. **父节点类型相同 → 递归对比子节点**

- 若父节点类型相同（如都是 `<div>`），则保留父节点对应的真实 DOM，继续对比其子节点列表。

- **子节点列表的对比仍遵循同级比较规则**：

  - 对比同一父节点下的直接子节点（如 `<h1>` 和 `<p>`）。
  - 对每个子节点递归执行同样的逻辑（即对比其属性和子节点）。

  

### **三、Key 如何优化列表渲染？**

它通过为列表项提供**唯一标识符**，帮助框架**高效识别**节点的移动、新增或删除，从而减少不必要的 DOM 操作。

- 通过 `key` 唯一标识节点，识别节点的移动、新增或删除。
- 算法会优先匹配相同 `key` 的节点，减少不必要的操作。

- 若找到相同 `key` 的节点，且类型相同，则复用 DOM 节点，仅更新属性和子节点。
- 若未找到，则创建新节点或删除旧节点。

#### **一、为什么需要 Key？**

#### **问题场景：无 Key 的列表更新**

假设有一个待办事项列表，初始状态为 `["Task 1", "Task 2", "Task 3"]`，渲染为：

```
<ul>
  <li>Task 1</li>  <!-- index=0 -->
  <li>Task 2</li>  <!-- index=1 -->
  <li>Task 3</li>  <!-- index=2 -->
</ul>
```

若在列表头部插入新任务 `"Task 0"`，新列表变为 `["Task 0", "Task 1", "Task 2", "Task 3"]`。
**无 Key 的对比过程**：

1. 按索引逐个对比新旧节点：
   - `index=0`：旧节点 `Task 1` vs 新节点 `Task 0` → 不同，销毁 `Task 1`，创建 `Task 0`。
   - `index=1`：旧节点 `Task 2` vs 新节点 `Task 1` → 不同，销毁 `Task 2`，创建 `Task 1`。
   - `index=2`：旧节点 `Task 3` vs 新节点 `Task 2` → 不同，销毁 `Task 3`，创建 `Task 2`。
   - 新增 `Task 3`。
2. **结果**：3 次 DOM 销毁，4 次 DOM 创建，**性能极差**。

------

#### **二、Key 的作用**

#### **精准匹配节点**

为每个列表项分配唯一的 `key`，框架通过 `key` **识别节点身份**，而非依赖索引。
示例（使用唯一 `id` 作为 `key`）：

```
<!-- 旧列表 -->
<ul>
  <li key="1">Task 1</li>  <!-- key="1" -->
  <li key="2">Task 2</li>  <!-- key="2" -->
  <li key="3">Task 3</li>  <!-- key="3" -->
</ul>

<!-- 插入新任务后 -->
<ul>
  <li key="0">Task 0</li>  <!-- 新增 key="0" -->
  <li key="1">Task 1</li>  <!-- key="1"（位置变化） -->
  <li key="2">Task 2</li>  <!-- key="2"（位置变化） -->
  <li key="3">Task 3</li>  <!-- key="3"（位置变化） -->
</ul>
```

**对比过程**：

1. 通过 `key` 匹配新旧节点：
   - 发现 `key="0"` 是新增节点。
   - `key="1"`、`key="2"`、`key="3"` 仅位置变化。
2. **结果**：移动现有 DOM 节点到新位置，仅创建 `key="0"` 的新节点。



## 4、什么是 JSX？它有什么优点？

**JSX** 是一种语法扩展，允许在 JavaScript 中编写类似 HTML 的标记。JSX 最终会被编译成普通的 JavaScript 代码。

**优点**：

- **可读性强**：JSX 使得模板代码更加直观和易读，特别是对于复杂的 UI 结构。
- **类型检查**：JSX 可以在编译时进行类型检查，减少运行时错误。
- **表达式支持**：可以在 JSX 中嵌入 JavaScript 表达式，使得动态生成 UI 变得更加方便。
- **工具支持**：现代开发工具（如 Babel）可以将 JSX 编译成兼容所有浏览器的 JavaScript 代码。

## 5. 什么是 Props 和 State？它们的区别是什么？

**Props**（属性）：

- **定义**：Props 是组件之间传递数据的一种方式。父组件可以通过 props 向子组件传递数据。
- **不可变**：Props 是只读的，子组件不能修改传入的 props。
- **用途**：用于配置组件的行为和外观。

**State**（状态）：

- **定义**：State 是组件内部的状态，用于存储组件的数据。
- **可变**：State 是可变的，可以通过 `setState` 方法更新。
- **用途**：用于管理组件的内部状态，控制组件的行为和渲染。

**区别**：

- **来源**：Props 来自父组件，State 是组件自身的状态。
- **可变性**：Props 是只读的，State 是可变的。
- **用途**：Props 用于配置组件，State 用于管理组件的内部状态。

## 6. 如何在 React 中创建一个组件？

**函数组件**：

```js
const MyComponent = (props) => {
  return <div>Hello, {props.name}!</div>;
};
```

**类组件**：

```js
class MyComponent extends React.Component {
  render() {
    return <div>Hello, {this.props.name}!</div>;
  }
}
```

## 7. 什么是函数组件和类组件？它们有什么区别？

**函数组件**：

- **定义**：函数组件是一个简单的 JavaScript 函数，接收 props 作为参数，返回 JSX。
- **优点**：代码更简洁，性能更好（因为没有类的开销）。
- **限制**：早期版本的函数组件不支持生命周期方法和状态管理，但随着 Hooks 的引入，这些限制已经被解除。

**类组件**：

- **定义**：类组件是继承自 `React.Component` 的 ES6 类，可以定义生命周期方法和管理状态。
- **优点**：支持生命周期方法和状态管理，功能更强大。
- **缺点**：代码相对复杂，性能略逊于函数组件。

## 8. 什么是纯组件？为什么要使用纯组件？

**纯组件**：

- **定义**：纯组件是一种特殊的组件，它通过 `React.memo`（函数组件）或 `PureComponent`（类组件）来实现。纯组件会在 props 或 state 发生变化时进行浅比较，如果前后值相同，则跳过重新渲染。
- **优点**：
  - **性能优化**：减少不必要的重新渲染，提高应用性能。
  - **简化逻辑**：开发者不需要手动实现 `shouldComponentUpdate` 方法来优化性能。

**使用场景**：

- **静态数据**：组件的 props 和 state 不经常变化。
- **复杂组件**：组件内部逻辑复杂，重新渲染开销大。

## 9、React Fiber

 React Fiber是 React 16 引入的一种新的协调引擎，旨在提高 React 应用的性能和响应能力。
Fiber 使得 React 可以更灵活地处理**大量更新**和**复杂组件树**。

工作原理：
调度：Fiber 引入了新的调度机制，允许 React 根据任务的优先级来调度任务。React 会根据任务的紧急程度将任务放入不同的队列中，并按照队列的顺序执行任务。
渲染：在渲染阶段，React 会遍历组件树，并构建一个 Fiber 树。Fiber 树中的每个节点代表一个组件，并包含组件的状态、属性等信息。
更新：当组件的状态或属性发生变化时，React 会触发更新。Fiber 会根据变化的类型和优先级来决定如何更新组件。
提交：在更新阶段完成后，React 会将 Fiber 树转换为实际的 DOM 树，并提交给浏览器进行渲染。

## 10、样式和类

```js
1、组件中的内联样式
class Header extends React.Component {
  render() {
    return (<header style={{color:"red"}}>这是头</header>)    //外层{}为jsx语法，内层{}为对象写法
  }
}

2、直接导入css
import "XXX.css";     //导入定义过的css文件
const Main = ()=>{
  return (<main className="orange big">这是身体</main>)      //class为关键字，必须使用className
}

3、不同的条件添加不同的样式-使用classnames这个包
//下载安装classnames包
$npm i classnames
//引入classnames包
import classNames from "classNames/bind"
//引入CSS文件
import styles from './classNames.css'
let cx = classNames.bind(styles);

function Footer() {
  let className = cx({
    blue: true,
    red: false
  }) ;
  return <footer className={className}>这是脚</footer>;
}

4、在js中写css改变样式
//安装包
$npm i styled-components
//新建含有css的js文件，导入模块并导出样式
import styled from "styled-components";
const Pstyled = styled.h1`                      //h1为标签名，后面接模板字符串
  color: red;
  font-size: ${(props) => props.size + "px"}; `;  //可以通过props传值
export { Pstyled };
//组件中使用
import React, { Component } from "react";
import { Pstyled } from "./scc-in-js.js";

class App extends Component {
  render() {
    return (
      <>
        <Pstyled size="60">styled-components</Pstyled>
      </>
    );
  }
}
export default App;
```

## 11、**React Portal 详解**

React Portal 是 React 提供的一种特殊功能，允许将子组件**渲染到父组件 DOM 结构之外**的 **DOM 节点**中。它的核心用途是**解决组件层级与 DOM 层级不匹配的问题**，常用于**模态框**、全局提示、悬浮菜单等需要**脱离当前容器限制**的场景。

------

### **一、为什么需要 Portal？**

#### **1. 突破样式限制**

- **问题场景**：父组件的 CSS 属性（如 `overflow: hidden`、`z-index`、`position`）可能限制子组件的显示。
- **示例**：
  父容器设置了 `overflow: hidden`，子组件中的模态框可能被裁剪，无法完整展示。

#### **2. 避免层级冲突**

- **问题场景**：组件需要渲染到 DOM 树的更高层级（如直接挂载到 `<body>` 下），以确保其覆盖其他元素。
- **示例**：
  全局通知或弹窗需要显示在页面最顶层，不受父组件 `z-index` 的影响。

#### **3. 保持 React 组件树结构**

- **优势**：Portal 的子组件在 React 组件树中仍属于原父组件，事件冒泡和上下文传递遵循 React 树结构，而非实际 DOM 结构。

------

## 12、浅拷贝与深拷贝，浅比较与深比较

**浅拷贝（Shallow Copy）**

- **定义**：仅复制对象的顶层属性。如果属性是引用类型（如对象、数组），则复制的是引用地址，而非实际数据。
- **特点**：新旧对象共享嵌套的引用类型数据，修改其中一个会影响另一个。
- 使用：

```js
// 1. 对象展开运算符（...）
const shallowCopy1 = { ...originalObject };

// 2. Object.assign()
const shallowCopy2 = Object.assign({}, originalObject);

// 3. 数组的浅拷贝（slice/concat）
const shallowArray = originalArray.slice();
```

**深拷贝（Deep Copy）**

- **定义**：递归复制对象的所有层级，新旧对象完全独立，不共享任何引用。

- **特点**：完全独立的数据，修改不影响原对象。

  ```js
  // 1. JSON方法（有局限性：无法处理函数、循环引用等）
  const deepCopy1 = JSON.parse(JSON.stringify(originalObject));
  
  // 2. 递归实现
  function deepClone(obj) {
    if (typeof obj !== 'object' || obj === null) return obj;
    const result = Array.isArray(obj) ? [] : {};
    for (const key in obj) {
      if (obj.hasOwnProperty(key)) {
        result[key] = deepClone(obj[key]);
      }
    }
    return result;
  }
  
  // 3. 使用第三方库（如 Lodash 的 _.cloneDeep）
  const deepCopy2 = _.cloneDeep(originalObject);
  ```

| 浅拷贝   | 深拷贝                     |                                  |
| :------- | :------------------------- | -------------------------------- |
| 嵌套对象 | 共享引用（修改会互相影响） | 完全独立（互不影响）             |
| 性能     | 快（仅复制顶层）           | 慢（递归所有层级）               |
| 实现难度 | 简单                       | 复杂（需处理循环引用等边界情况） |

**浅比较（Shallow Compare）**

- **定义**：仅比较对象的**第一层属性**是否相等。
  - 对基本类型（如 `number`, `string`, `boolean`）直接比较值是否相等。
  - 对引用类型（如 `object`, `array`）比较其**引用地址**是否相同（即是否指向同一内存地址）。
- **特点**：
  - 速度快，但无法检测嵌套对象的内容变化。
  - 适用于性能敏感场景（如 React 组件的 `props` 和 `state` 比较）。

```
function shallowCompare(obj1, obj2) {
  if (obj1 === obj2) return true;
  if (typeof obj1 !== 'object' || obj1 === null || typeof obj2 !== 'object' || obj2 === null) {
    return false;
  }

  const keys1 = Object.keys(obj1);
  const keys2 = Object.keys(obj2);
  if (keys1.length !== keys2.length) return false;

  for (const key of keys1) {
    if (!obj2.hasOwnProperty(key) || obj1[key] !== obj2[key]) {
      return false;
    }
  }
  return true;
}
```

**深比较（Deep Compare）**

- **定义**：递归比较对象的**所有层级**，确保所有嵌套属性值完全相等。

  - 对引用类型会深入比较其内容，而非仅比较引用地址。

- **特点**：

  - 完全精确，但性能开销较大。
  - 适用于需要严格判断对象内容是否一致的场景（如状态管理、数据快照）。

- **实现方式**：

  ```
  function deepCompare(obj1, obj2) {
    if (obj1 === obj2) return true;
    if (typeof obj1 !== 'object' || obj1 === null || typeof obj2 !== 'object' || obj2 === null) {
      return false;
    }
  
    const keys1 = Object.keys(obj1);
    const keys2 = Object.keys(obj2);
    if (keys1.length !== keys2.length) return false;
  
    for (const key of keys1) {
      if (!obj2.hasOwnProperty(key)) return false;
      if (!deepCompare(obj1[key], obj2[key])) return false;
    }
    return true;
  }
  ```

**关键区别**

|          | 浅比较                             | 深比较                           |
| :------- | :--------------------------------- | :------------------------------- |
| 比较层级 | 仅第一层                           | 递归所有层级                     |
| 引用类型 | 比较引用地址是否相同               | 比较内容是否完全一致             |
| 性能     | 快                                 | 慢（层级越深，性能越低）         |
| 使用场景 | 简单对象、性能敏感场景（如 React） | 复杂嵌套对象、严格数据一致性场景 |

### **二、Portal 的基本用法**

#### **1. 语法**

通过 `ReactDOM.createPortal` 创建 Portal，需指定目标 DOM 节点：

```js
import ReactDOM from 'react-dom';

function Modal({ children }) {
  // 将 children 渲染到 body 下的指定 DOM 节点
  return ReactDOM.createPortal(
    children,
    document.getElementById('modal-root')
  );
}
```

![img](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1c6efd458eaa40479781306c057a157a~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp#?w=780&h=740&s=151852&e=png&b=1f1f1f)

以上就是把<p>aaaa<p>绑定到body下边

# 组件

## 1. 什么是受控组件和非受控组件？它们的区别是什么？

**受控组件（Controlled Components）** ：

- **定义**：受控组件是指那些其**输入值由 React 的状态（state）控制**的表单组件。每次用户输入时，都会触发一个事件处理器，**更新组件的状态**，从而更新表单的值。

- **特点**：

  - **状态管理**：表单的值由 React 状态管理。
  - **事件处理**：每次用户输入时，都会触发事件处理器。

- **示例**：

  **实时验证**
  需要即时验证用户输入（如密码强度、格式校验）。

  ```js
  import React, { useState } from 'react';
  
  const ControlledForm = () => {
    const [value, setValue] = useState('');
  
    const handleChange = (e) => {
      setValue(e.target.value);
    };
  
    const handleSubmit = (e) => {
      e.preventDefault();
      console.log('Form submitted with value:', value);
    };
  
    return (
      <form onSubmit={handleSubmit}>
        <input type="text" value={value} onChange={handleChange} />
        <button type="submit">Submit</button>
      </form>
    );
  };
  
  export default ControlledForm;
  ```

**非受控组件（Uncontrolled Components）** ：

- **定义**：非受控组件是指那些其输入值**不由 React 状态管理**的表单组件。相反，它们依赖于 DOM API 来**获取表单的值**。

- **特点**：

  - **DOM API**：通过 **`ref`** 获取表单的值。
  - **初始值**：可以通过 `defaultValue` 或 `defaultChecked` 属性设置初始值。

- **示例**：

  **简单表单提交**
  只需在提交时一次性获取数据，无需中间状态（如登录表单）

  ```js
  import React, { useRef } from 'react';
  
  const UncontrolledForm = () => {
    const inputRef = useRef(null);
  
    const handleSubmit = (e) => {
      e.preventDefault();
      console.log('Form submitted with value:', inputRef.current.value);
    };
  
    return (
      <form onSubmit={handleSubmit}>
        <input type="text" ref={inputRef} />
        <button type="submit">Submit</button>
      </form>
    );
  };
  
  export default UncontrolledForm;
  ```

## 2. 如何在 React 中实现条件渲染？

**条件渲染** 是指根据条件决定是否渲染某个组件或元素。React 提供了多种方式来实现条件渲染：

- **三元运算符**：

  ```jsx
  const App = ({ isLoggedIn }) => {
    return (
      <div>
        {isLoggedIn ? <h1>Welcome back!</h1> : <h1>Please log in.</h1>}
      </div>
    );
  };
  ```

- **逻辑与运算符**：

  ```jsx
  const App = ({ user }) => {
    return (
      <div>
        {user && <h1>Welcome, {user.name}!</h1>}
      </div>
    );
  };
  ```

- **使用 `if` 语句**：

  ```jsx
  const App = ({ items }) => {
    if (items.length === 0) {
      return <h1>No items found.</h1>;
    }
  
    return (
      <ul>
        {items.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    );
  };
  ```

## 3. 什么是高阶组件（HOC）？请给出一个实际的例子。

**高阶组件（Higher-Order Component, HOC）** 是一个函数，它接受一个组件并返回一个新的组件。HOC 用于复用组件逻辑，增强组件的功能。

**示例**： 假设我们有一个 `withLogging` HOC，用于在组件渲染时记录日志。

```js
js 代码解读复制代码import React from 'react';

// 高阶组件
const withLogging = (WrappedComponent) => {
  return class extends React.Component {
    componentDidMount() {
      console.log(`Component ${WrappedComponent.name} mounted`);
    }

    componentWillUnmount() {
      console.log(`Component ${WrappedComponent.name} will unmount`);
    }

    render() {
      return <WrappedComponent {...this.props} />;
    }
  };
};

// 被包裹的组件
const MyComponent = (props) => {
  return <h1>Hello, {props.name}!</h1>;
};

// 使用 HOC
const MyComponentWithLogging = withLogging(MyComponent);

export default MyComponentWithLogging;
```

## 4. 什么是 Fragment？它的作用是什么？

**Fragment** 是 React 提供的一个特殊组件，用于包裹多个子元素，而不会在 DOM 中添加额外的节点。这在需要返回多个根节点时非常有用。

**作用**：

- **避免额外的 DOM 节点**：在需要返回多个根节点时，使用 Fragment 可以避免添加不必要的 DOM 节点。
- **保持代码整洁**：可以使 JSX 代码更加简洁和易读。

**示例**：

```js
js 代码解读复制代码import React from 'react';

const App = () => {
  return (
    <React.Fragment>
      <h1>Title</h1>
      <p>Paragraph 1</p>
      <p>Paragraph 2</p>
    </React.Fragment>
  );
};

// 简写形式
const App = () => {
  return (
    <>
      <h1>Title</h1>
      <p>Paragraph 1</p>
      <p>Paragraph 2</p>
    </>
  );
};

export default App;
```

## 5、什么是 Refs？如何使用 Refs？

**Refs** 是 React 提供的一种访问 DOM 节点或在类组件中访问实例的方法。Refs 可以用于获取输入值、管理焦点、触发动画等。

**使用方法**：

- **创建 Ref**：使用 `useRef` Hook 或 `React.createRef`。
- **附加 Ref**：将 Ref 附加到需要访问的 DOM 节点或组件实例。
- **访问 Ref**：通过 `ref.current` 访问 DOM 节点或组件实例。



# 类组件生命周期

## 1、请列出 React 组件的生命周期方法，并简要说明每个方法的作用。

React 组件的生命周期可以分为三个阶段：挂载（Mounting）、更新（Updating）和卸载（Unmounting）。以下是各个生命周期方法及其作用：

##### 挂载阶段（Mounting）

1. **`constructor(props)`** ：
   - **作用**：初始化组件的 state 和绑定事件处理器。
   - **调用时机**：组件实例被创建时。
2. **`static getDerivedStateFromProps(props, state)`** ：
   - **作用**：在组件实例被创建和更新时调用，用于根据 props 更新 state。
   - **调用时机**：组件实例被创建和每次更新之前。
3. **`render()`** ：
   - **作用**：返回组件的 JSX，描述 UI 的结构。
   - **调用时机**：组件实例被创建和每次更新时。
4. **`componentDidMount()`** ：
   - **作用**：组件挂载完成后调用，通常用于发起网络请求、设置定时器等。
   - **调用时机**：组件首次渲染到 DOM 后。

##### 更新阶段（Updating）

1. **`static getDerivedStateFromProps(props, state)`** ：
   - **作用**：在组件更新时调用，用于根据新的 props 更新 state。
   - **调用时机**：组件接收到新 props 或 state 变化时。
2. **`shouldComponentUpdate(nextProps, nextState)`** ：
   - **作用**：决定组件是否需要重新渲染，默认返回 `true`。
   - **调用时机**：组件接收到新 props 或 state 变化时。
3. **`render()`** ：
   - **作用**：返回组件的 JSX，描述 UI 的结构。
   - **调用时机**：组件实例被创建和每次更新时。
4. **`getSnapshotBeforeUpdate(prevProps, prevState)`** ：
   - **作用**：在组件更新前捕获一些信息，这些信息可以在 `componentDidUpdate` 中使用。
   - **调用时机**：组件更新前。
5. **`componentDidUpdate(prevProps, prevState, snapshot)`** ：
   - **作用**：组件更新完成后调用，通常用于更新 DOM 或发起网络请求。
   - **调用时机**：组件更新后。

##### 卸载阶段（Unmounting）

1. **`componentWillUnmount()`** ：
   - **作用**：组件卸载前调用，通常用于清理工作，如取消网络请求、清除定时器等。
   - **调用时机**：组件从 DOM 中移除前。



## 2、react子组件更新触发父组件更新

 当React子组件发生改变时，父组件可能会触发以下生命周期方法：

1. `componentWillReceiveProps(nextProps)`

   当父组件接收到新的props时，会触发该生命周期方法。子组件的改变可能会导致父组件的props发生变化，从而触发该方法。

2. `shouldComponentUpdate(nextProps, nextState)`

   当父组件的props或state发生改变时，会触发该方法。该方法用于判断是否需要重新渲染组件。如果返回false，组件将不会更新。

3. `componentWillUpdate(nextProps, nextState)`

   在组件更新之前调用，可以在此方法中进行一些准备工作或进行一些副作用操作。

4. `render()`

   无论父组件的props或state是否发生改变，都会触发render方法重新渲染父组件。

5. `componentDidUpdate(prevProps, prevState)`

   在组件更新之后调用，可以在此方法中进行一些副作用操作，比如更新DOM或发送网络请求。

需要注意的是，以上生命周期方法中的一些在未来版本的React中可能会被废弃或改变。因此，建议在使用时查看React官方文档以获取最新信息。

## 3、类组件生命周期过程

```
// componentWillMount是被废弃了, 改名成了UNSAFE_componentWillMount
  UNSAFE_componentWillMount() {
    console.log("componentWillMount");
    // componentWillMount不能做数据请求
    // 因为fiber算法的原因,可能导致这个生命周期被执行多次
  }

  // render本身就是一个生命周期,每次数据改变都重新渲染
  render() {
    console.log("render");
    return (
      <>
        <h3>老版生命周期</h3>
        <p>count: {this.state.count}</p>
      </>
    );
  }

  // 组件挂载完成之后
  componentDidMount() {
    console.log("componentDidMount");
    this.setState({
      count: 20,
    });
  }
// 被废弃了, 因为现在由更好的生命周期来代替
// 当父组件传递的自定义属性发生改变时就会触发
  UNSAFE_componentWillReceiveProps(nextProps) {
    console.log(nextProps);
  }
  
//决定子组件是否重新渲染
// 必须要返回true或者false, 通过返回true或者false来控制是否要重新渲染当前组件
// 可以接收两个参数，新的props和新的state，旧的还是this.props.xxx
// PureComponent 纯组件   用于代替Component
shouldComponentUpdate(nextProps, nextState) {
    return nextState.count !== this.state.count;
  }
  
// 被弃用
  UNSAFE_componentWillUpdate() {
    console.log("componentWillUpdate");
  }
   
// 更新结束
// 不能在这里更新数据
  componentDidUpdate() {
    console.log("componentDidUpdate");
  } 
//组件将要销毁
componentWillUnmount() {
    // 定时器，事件监听，websocket, 插件等
    console.log("componentWillUnmount");
  }
```

## 4、类组件的生命周期-新版

#### 什么是 `getDerivedStateFromProps`？它的作用是什么？

**`getDerivedStateFromProps`** 是一个静态方法，用于在组件实例被创建和更新时根据 props 更新 state。它是一个纯函数，不能执行副作用操作。

**作用**：

- **同步更新 state**：确保组件的 state 始终与 props 保持一致。
- **避免不必要的更新**：通过返回 `null`，可以避免不必要的 state 更新。

**示例**：

```jsx
class MyComponent extends React.Component {
  static getDerivedStateFromProps(props, state) {
    if (props.counter !== state.prevCounter) {
      return { prevCounter: props.counter, hasChanged: true };
    }
    return null;
  }

  render() {
    return (
      <div>
        <p>Counter: {this.props.counter}</p>
        {this.state.hasChanged && <p>Counter has changed!</p>}
      </div>
    );
  }
}
```

#### 什么是 `getSnapshotBeforeUpdate`？它的作用是什么？

**`getSnapshotBeforeUpdate`** 是一个生命周期方法，用于在组件更新前捕获一些信息，这些信息可以在 `componentDidUpdate` 中使用。它通常用于捕获 DOM 的当前状态。

**作用**：

- **捕获 DOM 状态**：在组件更新前捕获 DOM 的当前状态，以便在更新后进行比较或处理。
- **避免闪烁**：确保在更新后能够平滑地过渡到新的状态。

**示例**：

```jsx
class ScrollingList extends React.Component {
  constructor(props) {
    super(props);
    this.listRef = React.createRef();
  }

  getSnapshotBeforeUpdate(prevProps, prevState) {
    if (prevProps.list.length < this.props.list.length) {
      const list = this.listRef.current;
      return list.scrollHeight - list.scrollTop;
    }
    return null;
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    if (snapshot !== null) {
      const list = this.listRef.current;
      list.scrollTop = list.scrollHeight - snapshot;
    }
  }

  render() {
    return (
      <div ref={this.listRef}>
        {this.props.list.map(item => (
          <div key={item.id}>{item.name}</div>
        ))}
      </div>
    );
  }
}
```

####  什么是 `componentDidCatch`？它的作用是什么？

**`componentDidCatch`** 是一个生命周期方法，用于捕获和处理组件树中的错误。它类似于 JavaScript 中的 `try...catch` 语句，但作用于 React 组件。

**作用**：

- **捕获错误**：在组件树中捕获任何未捕获的错误。
- **记录错误**：可以用于记录错误信息，发送错误报告等。
- **显示错误页面**：可以用于显示友好的错误页面，提升用户体验。

**示例**：

```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false, error: null, info: null };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true, error };
  }

  componentDidCatch(error, info) {
    this.setState({ error, info });
    // 可以在这里记录错误信息
    console.error("Caught an error:", error, info);
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}

const App = () => (
  <ErrorBoundary>
    <MyComponent />
  </ErrorBoundary>
);
```

# 状态管理

## 1. 什么是 Redux？它的主要特点是什么？

**Redux** 是一个用于管理应用状态的 JavaScript 库，通常与 React 一起使用。它提供了一种集中管理应用状态的方式，使得状态管理更加可预测和可维护。

**主要特点**：

- **单一数据源**：整个应用的状态存储在一个单一的 store 中，确保了状态的一致性。
- **状态不可变**：状态是不可变的，每次状态变化时，都会生成一个新的状态对象。
- **reducer**：通过reducer来处理状态变化，使得状态变化可预测。
- **中间件支持**：支持中间件，可以扩展 Redux 的功能，如异步操作、日志记录等。
- **开发者工具**：提供了强大的开发者工具，可以调试、回溯和重放状态变化。

## 2. 如何在 React 中使用 Redux？

**步骤**：

1. **安装 Redux 和 React-Redux**：

   ```bash
   npm install redux react-redux
   ```

2. **创建 Reducer**：

   ```javascript
   const initialState = { count: 0 };
   
   const counterReducer = (state = initialState, action) => {
     switch (action.type) {
       case 'INCREMENT':
         return { ...state, count: state.count + 1 };
       case 'DECREMENT':
         return { ...state, count: state.count - 1 };
       default:
         return state;
     }
   };
   ```

3. **创建 Store**：

   ```javascript
   import { createStore } from 'redux';
   import counterReducer from './counterReducer';
   
   const store = createStore(counterReducer);
   ```

4. **提供 Store**：

   ```js
   import React from 'react';
   import ReactDOM from 'react-dom';
   import { Provider } from 'react-redux';
   import store from './store';
   import App from './App';
   
   ReactDOM.render(
     <Provider store={store}>
       <App />
     </Provider>,
     document.getElementById('root')
   );
   ```

5. **连接组件**：

   ```javascript
   import React from 'react';
   import { useSelector, useDispatch } from 'react-redux';
   
   const Counter = () => {
     const count = useSelector(state => state.count);
     const dispatch = useDispatch();
   
     const increment = () => dispatch({ type: 'INCREMENT' });
     const decrement = () => dispatch({ type: 'DECREMENT' });
   
     return (
       <div>
         <h1>Count: {count}</h1>
         <button onClick={increment}>Increment</button>
         <button onClick={decrement}>Decrement</button>
       </div>
     );
   };
   
   export default Counter;
   ```

## 3. 什么是 Redux Thunk？它解决了什么问题？

**Redux Thunk** 是一个中间件，允许你在 action 创建函数中返回一个函数而不是一个 action 对象。这个返回的函数可以包含异步逻辑，并在适当的时候 dispatch 一个或多个 action。

**解决问题**：

- **异步操作**：Redux Thunk 允许你处理异步操作，如 AJAX 请求，而不需要在 reducer 中处理异步逻辑。
- **复杂逻辑**：可以处理复杂的业务逻辑，如条件 dispatch、多次 dispatch 等。

**示例**：

```javascript
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';

const initialState = { data: null };

const reducer = (state = initialState, action) => {
  switch (action.type) {
    case 'FETCH_DATA_SUCCESS':
      return { ...state, data: action.payload };
    default:
      return state;
  }
};

const fetchData = () => async (dispatch) => {
  const response = await fetch('/api/data');
  const data = await response.json();
  dispatch({ type: 'FETCH_DATA_SUCCESS', payload: data });
};

const store = createStore(reducer, applyMiddleware(thunk));

store.dispatch(fetchData());
```

## 4. 什么是 Redux Saga？它与 Redux Thunk 有什么区别？

**Redux Saga** 是一个用于管理应用副作用（如异步操作）的库，使用 Generator 函数来处理异步逻辑。

**区别**：

- **Generator 函数**：Redux Saga 使用 Generator 函数，提供了更强大的控制流和错误处理机制。
- **可测试性**：Redux Saga 的副作用可以更容易地进行单元测试。
- **复杂逻辑**：Redux Saga 更适合处理复杂的异步逻辑和并发操作。

**示例**：

```javascript
import { takeLatest, call, put } from 'redux-saga/effects';
import { fetchDataSuccess, fetchDataFailure } from './actions';

function* fetchDataSaga() {
  try {
    const response = yield call(fetch, '/api/data');
    const data = yield response.json();
    yield put(fetchDataSuccess(data));
  } catch (error) {
    yield put(fetchDataFailure(error));
  }
}

function* watchFetchData() {
  yield takeLatest('FETCH_DATA_REQUEST', fetchDataSaga);
}

export default function* rootSaga() {
  yield watchFetchData();
}
```

## 5、什么是React Redux-Tookit

### **一、为什么需要 Redux Toolkit？**

#### **传统 Redux 的痛点**

1. **繁琐的样板代码**：需手动编写 action types、action creators、reducers。
2. **配置复杂**：需自行集成中间件（如 Thunk、Logger）、DevTools。
3. **不可变更新易错**：手动使用 `...` 或 `Object.assign` 更新状态，容易出错。
4. **异步处理麻烦**：需依赖额外库（如 Redux-Saga、Redux-Observable）。

#### **Redux Toolkit 的优势**

- **简化代码**：内置 `createSlice` 自动生成 action 和 reducer。
- **开箱即用**：预置 `@reduxjs/toolkit` 包含 Immer、Thunk、DevTools。
- **类型安全**：完美支持 TypeScript。
- **高效开发**：减少 80% 的 Redux 代码量。

### **二、核心 API 与功能**

#### **1. `configureStore`：创建 Store**

替代 `createStore`，自动集成 Thunk、DevTools 和中间件。

```js
import { configureStore } from '@reduxjs/toolkit';
import rootReducer from './reducers';

const store = configureStore({
  reducer: rootReducer,
  middleware: (getDefaultMiddleware) => getDefaultMiddleware().concat(logger),
  devTools: process.env.NODE_ENV !== 'production'
});
```

#### **2. `createSlice`：定义 Slice**

自动生成 action types 和 action creators，简化 reducer 编写。

```js
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: 0,
  reducers: {
    increment: (state) => state + 1,
    decrement: (state) => state - 1,
    addBy: (state, action) => state + action.payload
  }
});

export const { increment, decrement, addBy } = counterSlice.actions;
export default counterSlice.reducer;
```

#### **3. `createAsyncThunk`：处理异步操作**

简化异步逻辑（如 API 请求），自动生成 pending/fulfilled/rejected 状态。

```js
import { createAsyncThunk } from '@reduxjs/toolkit';
import api from './api';

export const fetchUser = createAsyncThunk(
  'user/fetchUser',
  async (userId, { rejectWithValue }) => {
    try {
      const response = await api.get(`/users/${userId}`);
      return response.data;
    } catch (error) {
      return rejectWithValue(error.message);
    }
  }
);
```

#### 4、**集成 React**

使用 `Provider` 包裹应用，并通过 `useSelector`/`useDispatch` 访问状态。

```js
// store.ts
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from '../features/counter/counterSlice';

export const store = configureStore({
  reducer: {
    counter: counterReducer
  }
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;

// App.tsx
import { Provider } from 'react-redux';
import { store } from './app/store';

function App() {
  return (
    <Provider store={store}>
      <Counter />
    </Provider>
  );
}

// Counter.tsx
import { useDispatch, useSelector } from 'react-redux';
import { increment } from './counterSlice';
import type { RootState } from '../app/store';

function Counter() {
  const count = useSelector((state: RootState) => state.counter);
  const dispatch = useDispatch();

  return (
    <button onClick={() => dispatch(increment())}>Count: {count}</button>
  );
}
```

#### **5、`useDispatch`：派发 Action**

**作用**

**获取 Redux Store 的 `dispatch` 函数**，用于触发 Action 以更新 Store 中的状态。

**使用场景**

当组件需要触发状态变更时（如用户点击按钮、提交表单、发起异步请求等）。

**代码示例**

```js
import { useDispatch } from 'react-redux';
import { increment } from './counterSlice'; // 导入 Action Creator

function CounterButton() {
  const dispatch = useDispatch();

  return (
    <button onClick={() => dispatch(increment())}>
      Increment Counter
    </button>
  );
}
```

**关键特性**

1. **直接派发 Action**：
   可以派发同步 Action（如 `dispatch(increment())`）或异步 Action（如 `dispatch(fetchData())`）。
2. **无需手动订阅 Store**：
   React-Redux 自动处理与 Store 的连接。

------

#### **6、`useSelector`：获取 Store 状态**

**作用**

**从 Redux Store 中提取需要的状态值**，并订阅该状态的更新。当状态变化时，组件会自动重新渲染。

**使用场景**

当组件需要读取 Store 中的状态时（如显示计数器值、用户信息等）。

**代码示例**

```js
import { useSelector } from 'react-redux';

function CounterDisplay() {
  const count = useSelector((state) => state.counter.value);

  return <div>Current Count: {count}</div>;
}
```

**关键特性**

1. **选择器函数**：
   接受一个函数 `(state) => selectedValue`，返回需要的状态片段。
2. ****严格相等****：
   默认使用严格相等（`===`）比较前后状态值。**如果返回值是对象或数组，需确保引用稳定**，否则会导致不必要的重新渲染。
3. **性能优化**：
   - **使用记忆化选择器**（如 `createSelector`）避免重复计算。
   - **按需提取状态**：避免返回整个 Store 的根状态。

**`shallowEqual` 的作用**

- **默认行为问题**：
  `useSelector` 默认使用 `===` 比较前后两次选择器返回的值。如果返回的是新对象（如 `{ a: 1, b: 2 }`），即使内容相同，引用不同也会触发重新渲染。
- **`shallowEqual` 的优化**：
  比较对象或数组的第一层属性/元素的值，若所有值相同，则判定为“未变化”，跳过重新渲染。

如果对reducer状态值有计算，可以使用**`createSelector`** 

**createSelector作用**

- **缓存输入和输出**：记录上一次的输入参数和计算结果。
- **参数未变化时直接返回缓存结果**：跳过重复计算。
- **参数变化时重新计算**：更新缓存。



# 传参与通信

## 1. 父传子组件通信

父传子组件通信是 React 中最基本的通信方式之一。在这种模式下，数据是从父组件通过 **props** 传递给子组件的，子组件接收到 props 后进行渲染或其他操作。

#### 特点：

- **单向数据流：数据从父组件流向子组件，子组件无法直接修改父组件传递过来的 props。**
- 该 **`props`** 是个**对象**，里面包含了 **父组件** 传递的 **所有数据**
- 可对props进行解构设置**默认值**
- `props` **可传递** **任意类型** 的 **数据**
- 当我们把 **内容嵌套** 在 **子组件标签中时**，父组件会自动在名为 **`children`** 的 `prop`属性中 **接收该内容**；

```js
import React, { useState } from "react";

function Parent() {
  const [count, setCount] = useState<number>(0);

  return (
    <>
      <button type="button" onClick={() => setCount(count + 1)}>Add</button>
      <Child count={count}>Children</Child>
    </>
  );
}

function Child(props) {
  const { count, children } = props;

  return (
    <>
      <p>Received props from parent: {count}</p>
      <p>Received children from parent: {children}</p>
    </>
  );
}
```



## 2. 子传父组件通信

子传父组件通信是指子组件向父组件传递数据或事件的过程。通常通过在父组件中定义**回调函数**，并将其**作为 props** 传递给子组件，在子组件中执行回调函数，向父组件传值。

#### 特点：

- 子组件向父组件传递数据或事件：**子组件通过调用父组件传递的回调函数，向父组件传递数据或触发事件**。
- 灵活性高：可以在需要的时候向父组件传递数据，实现灵活的交互。

```js
import React, { useState } from "react";

function Parent() {
  const [count, setCount] = useState<number>(0);

  return (
    <>
      <div>Received from child: {count}</div>
      <Child updateCount={(value: number) => setCount(value)} />
    </>
  );
}

function Child(props) {
  const { updateCount } = props;
  const [count, setCount] = useState<number>(0);

  return (
    <button
      type="button"
      onClick={() => {
        const newCount: number = count + 1;
        setCount(newCount);
        updateCount(newCount);
      }}
    >
      Add
    </button>
  );
}
```

## 3. 兄弟组件通信

兄弟组件通信是指不具有直接父子关系的两个组件之间进行数据传递和交互的过程。在 React 中，通常需要通过共享父组件来实现兄弟组件之间的通信。

> - 实现思路
>   - 借助 **“状态提升”** 机制，通过 **共同的父组件** 进行 **兄弟组件之间** 的 **数据传递**；
> - 状态提升
>   - 将需要使用的状态提升到他们共同的父组件中；



注意：**兄弟组件使用共同的父组件作为桥梁，本质是父子之间通信。**

```js
import { useState } from 'react';

const App = () => {
  const [brotherDate, setBrotherDate] = useState('');
  return (
    <div>
      <div>
        父组件接收的数据（子组件触发条件之后才能传递过来）：
        <span style={{ color: 'red', fontSize: '20px' }}>{brotherDate}</span>
      </div>
      <br />
      <A bDate={brotherDate} onUpdateBrotherDate={(val) => setBrotherDate(val)} />
      <br />
      <B aDate={brotherDate} onUpdateBrotherDate={(val) => setBrotherDate(val)} />
    </div>
  );
};

const A = ({ bDate, onUpdateBrotherDate }) => {
  const [val, setVal] = useState('this is A name');
  return (
    <div>
      <div>A组件： A向B传递的数据： {val}</div>
      <div>
        A组件： B传递的数据：
        <span style={{ color: 'red', fontSize: '20px' }}>{bDate}</span>
      </div>
      <input type="text" value={val} onChange={(e) => setVal(e.target.value)} />
      <button onClick={() => onUpdateBrotherDate(val)}>A组件数据 ➡ B组件</button>
    </div>
  );
};

const B = ({ aDate, onUpdateBrotherDate }) => {
  const [val, setVal] = useState('this is B name');
  return (
    <div>
      <div>
        B组件： A传递过来的数据：
        <span style={{ color: 'red', fontSize: '20px' }}>{aDate}</span>
      </div>
      <div>B组件： B向A传递的数据： {val}</div>
      <input type="text" value={val} onChange={(e) => setVal(e.target.value)} />
      <button onClick={() => onUpdateBrotherDate(val)}>B组件数据 ➡ A组件</button>
    </div>
  );
};

export default App;

```

## 4. 使用Context进行跨层级组件通信

当组件层级较深或通信的组件距离较远时，可以使用React的Context API进行跨层级通信。**Context允许我们在组件树中传递数据，而不必手动通过Props一层层传递**。

- 使用 **`createContext`** 方法创建一个 **上下文对象** **`Ctx`**（上下文对象的名字可以随便起）；

- 在 **顶层组件**（App）中通过 **`Ctx.Provider`组件**（高阶组件） **提供数据**；
- 在 **底层组件**（B）中通过 **`useContext`** 钩子函数 **使用数据**；

> 注意
>
> - **`Ctx.Provider`** 组件上有个 `value`属性，用来提供数据，属性名只能是`value`；
> - 一定要使用 **`Ctx.Provider`** 组件 **包裹住 层级组件**，否则在底层组件使用 `useContext` 钩子得到的数据就是 `undefined`；

```js
import { useContext } from 'react';
import { createContext } from 'react';

// 1、使用 createContext 创建上下文对象
const MsgContext = createContext();

const A = () => {
  return (
    <div>
      <h2>this is A Component</h2>
      <B />
    </div>
  );
};

const B = () => {
  // 3、在底层组件，使用 useContext 钩子函数获取数据
  const msg = useContext(MsgContext);
  return (
    <div>
      <h3>this is B Component</h3>
      {msg}
    </div>
  );
};

const App = () => {
  const msg = 'this is app msg';
  // 2、在顶层组件，通过 Provider将数据传递给底层组件
  return (
    <div>
      {/* value属性提供数据，不能更改属性名，只能使用value */}
      {/* 一定要使用 MsgContext.provider 组件包裹住层级组件 */}
      <MsgContext.Provider value={msg}>
        <h1>this is App Component</h1>
        <A />
      </MsgContext.Provider>
    </div>
  );
};

export default App;

```

# 路由

React Router 是一个流行的库，用于在 React 应用程序中处理路由。它允许你定义应用程序的路由结构，并且可以基于当前的 URL 来渲染不同的组件。

**主要特点**：

- **声明式路由**：使用声明式的方式来定义路由，使代码更加清晰和易于维护。
- **动态路由匹配**：支持动态参数匹配，可以根据 URL 参数动态加载不同的组件。
- **嵌套路由**：支持嵌套路由，可以轻松实现多级嵌套的页面结构。
- **编程式导航**：提供编程式导航的方法，可以在代码中控制页面的跳转。
- **路由守卫**：支持路由守卫，可以在路由切换前后执行特定的逻辑。
- **懒加载**：支持代码分割和懒加载，可以按需加载组件，提高应用性能。

## 1、路由模式

### （1）hash模式

**核心原理**

 **URL 结构**

- Hash 模式通过 URL 的 **哈希（Fragment）** 部分标识路由，格式为：
  `http://example.com/#/path`
  其中 `#/path` 是哈希值，服务器会忽略 `#` 后的内容，仅前端处理。

2. **实现机制**

- **监听哈希变化**：

  通过 `hashchange` 事件监听 URL 哈希的变化，然后渲染不同的内容，触发页面内容更新。

  这种路由不向服务器发送请求，不需要服务端的支持
  
  ```
  window.addEventListener('hashchange', () => {
    const path = window.location.hash.substr(1); // 获取 # 后的路径（如 "/home"）
    renderComponentBasedOnPath(path);
  });
  ```

#### **（2）Hash 模式的核心特点**

#### **优点**

1. **兼容性好**
   - 支持所有浏览器（包括 IE9 及以下），无需服务器配置。
   - 通过监听 `hashchange` 事件实现路由跳转，无需后端配合。
2. **无服务器配置问题**
   - 无论 URL 如何变化，浏览器只会请求 `#` 前的路径（如 `http://example.com`），服务器只需返回单页应用**入口文件（如index.html）**即可。
3. **天然解决刷新问题**
   - 用户刷新页面时，不会因路径变化导致 404 错误。

#### **缺点**

1. **URL 不美观**
   - `#` 符号破坏 URL 的简洁性，可能影响用户体验或 SEO（现代搜索引擎已能抓取 Hash 内容，但不如普通路径友好）。
2. **功能限制**
   - `#` 后的内容不会被包含在 HTTP 请求中，无法直接用于服务端渲染（SSR）。
   - 无法利用 HTML5 History API 的完整功能（如添加页面状态）。

### （2）history模式

#### 概述

`window.history` 属性指向 History 对象，它表示当前窗口的浏览历史。当发生改变时，只会改变页面的路径，不会刷新页面。 History 对象保存了当前窗口访问过的所有页面网址。通过 history.length 可以得出当前窗口一共访问过几个网址。 由于安全原因，浏览器不允许脚本读取这些地址，但是允许在地址之间导航。 浏览器工具栏的“前进”和“后退”按钮，其实就是对 History 对象进行操作。

属性

History 对象主要有两个属性。

- `History.length`：当前窗口访问过的网址数量（包括当前网页）
- `History.state`：History 堆栈最上层的状态值（详见下文）

```js
// 当前窗口访问过多少个网页
history.length // 1

// History 对象的当前状态
// 通常是 undefined，即未设置

history.state // undefined
```

#### 方法

`History.back()`、`History.forward()`、`History.go()`，这三个方法用于在历史之中移动。

- `History.back()`：移动到上一个网址，等同于点击浏览器的后退键。对于第一个访问的网址，该方法无效果。
- `History.forward()`：移动到下一个网址，等同于点击浏览器的前进键。对于最后一个访问的网址，该方法无效果。
- `History.go()`：接受一个整数作为参数，以当前网址为基准，移动到参数指定的网址。如果参数超过实际存在的网址范围，该方法无效果；如果不指定参数，默认参数为0，相当于刷新当前页面。

```js
history.back();
history.forward();
history.go(1);//相当于history.forward()
history.go(-1);//相当于history.back()
history.go(0); // 刷新当前页面
```

**注意**：移动到以前访问过的页面时，页面通常是从**浏览器缓存**之中加载，而**不是重新要求服务器发送新的网页**。

#### ** history 核心 API**

**`pushState` 和 `replaceState` 是实现前端路由切换的核心 API**，但它们本身只负责修改 URL 和历史记录，**不直接触发页面渲染或组件切换**。需要结合其他逻辑（如监听 `popstate` 事件、更新 UI）才能完成完整的路由功能。

- **`history.pushState(state, title, url)`**
  向浏览器历史记录栈添加一条记录，**不触发页面刷新**。

  - `state`：与路由关联的状态对象（可通过 `history.state` 访问）。
  - `title`：浏览器忽略此参数（通常传空字符串）。
  - `url`：新的路由路径（必须同源）。

  ```
  // 添加一条历史记录，URL 变为 /home
  history.pushState({ page: 'home' }, '', '/home');
  ```

- **`history.replaceState(state, title, url)`**
  替换当前历史记录，**不触发页面刷新**。

  ```
  // 替换当前记录，URL 变为 /about
  history.replaceState({ page: 'about' }, '', '/about');
  ```

- **`popstate` 事件**
  当用户点击浏览器前进/后退按钮或调用 `history.back()`、`history.forward()` 时触发。

  ```
  window.addEventListener('popstate', (event) => {
    const path = window.location.pathname; // 获取当前路径
    renderComponent(path);
  });
  ```

**History 模式的工作流程**

#### **2. 具体实现方式**

##### **(1) 主动切换路由（如用户点击导航链接）**

```
// 示例：点击导航到 "/about" 页面
function navigateToAbout() {
  // 1. 使用 pushState 修改 URL（添加新历史记录）
  history.pushState({ page: "about" }, "", "/about");
  
  // 2. 手动更新 UI（例如渲染 About 组件）
  renderAboutPage();
}
```

##### **(2) 监听浏览器前进/后退（用户操作历史记录）**

```
// 监听 popstate 事件（用户点击前进/后退按钮时触发）
window.addEventListener("popstate", (event) => {
  // 从 event.state 获取关联的状态对象（即 pushState 时传入的 state）
  const state = event.state;
  
  // 根据当前 URL 或 state 更新 UI
  if (state && state.page === "about") {
    renderAboutPage();
  } else {
    renderHomePage();
  }
});
```

##### （3）hiistory模式的优缺点

**优点**

1. **URL 美观**
   - 无 `#` 符号，更接近传统 URL 结构，对 SEO 更友好。
2. **功能强大**
   - 可结合 HTML5 History API（`pushState`、`replaceState`）实现灵活的路由控制，支持添加历史记录或修改 URL 而不刷新页面。
   - 适合服务端渲染（SSR）或需要后端动态路由的场景。

**缺点**

1. **需要服务器支持**
   - 用户直接访问深层路径（如 `http://example.com/path`）或刷新页面时，浏览器会向服务器请求该路径。若服务器未配置将所有请求重定向到入口文件（如 `index.html`），会返回 404 错误。
   - **解决方案**：配置服务器（如 Nginx、Apache、Node.js）支持 Fallback，将所有路径指向入口文件。
2. **兼容性限制**
   - 依赖 HTML5 History API，不支持 IE9 及以下浏览器（可通过 Polyfill 部分解决）

**hash模式和history模式的核心区别**

| **特性**         | **Hash 模式**             | **History 模式**                |
| :--------------- | :------------------------ | :------------------------------ |
| **URL 格式**     | 含 `#`（如 `/#/home`）    | 无 `#`（如 `/home`）            |
| **服务器配置**   | 无需特殊配置              | 需配置所有路径返回 `index.html` |
| **兼容性**       | 所有浏览器（包括 IE9+）   | IE10+，依赖 HTML5 History API   |
| **SEO 友好性**   | 一般（依赖爬虫能力）      | 更友好（URL 规范）              |
| **实现复杂度**   | 简单（无需服务器支持）    | 较高（需服务器配合）            |
| **典型框架使用** | Vue Router 的 `hash` 模式 | Vue Router 的 `history` 模式    |

##### **（4）什么情况下会触发浏览器向服务器发送请求**

------

##### **1. 用户手动输入 URL 并回车（或通过书签、外部链接访问）**

- **场景**：用户直接在地址栏输入 `http://example.com/path` 并回车，或通过书签、其他网站链接访问该 URL。
- **行为**：
  浏览器认为这是一个全新的页面请求，会向服务器发送对 `/path` 的 HTTP 请求。
- **关键点**：
  若服务器未正确配置（未将所有路径重定向到入口文件 `index.html`），将返回 **404 错误**。

------

##### **2. 刷新当前页面（F5 或 Ctrl/Cmd + R）**

- **场景**：用户在当前页面（如 `http://example.com/path`）按下刷新键。
- **行为**：
  浏览器重新请求当前 URL 对应的资源（即 `/path`）。
- **关键点**：
  服务器仍需正确返回入口文件，否则刷新后页面将丢失，出现 404。

## 2、react router

### （1）react router中的两种路由模式

全局路由有常用两种路由模式可选：HashRouter 和 BrowserRouter 

#### **HashRouter**

##### **1. 用法**

- **引入并配置**：
  在应用入口文件（如 `App.js`）中使用 `BrowserRouter` 包裹路由组件：

  ```
  import { BrowserRouter as Router, Route, Link } from 'react-router-dom';
  
  function App() {
    return (
      <Router>
        <nav>
          <Link to="/">Home</Link>
          <Link to="/about">About</Link>
        </nav>
        <Route path="/" exact component={Home} />
        <Route path="/about" component={About} />
      </Router>
    );
  }
  ```

##### **2. 原理**

- **基于 History API**：
  `BrowserRouter` 使用 HTML5 的 `history` 对象（`pushState`、`replaceState`、`popstate` 事件）来操作浏览器历史记录，实现 URL 的更新和监听。
  - **修改 URL**：通过 `history.pushState()` 改变 URL，无页面刷新。
  - **监听变化**：通过 `popstate` 事件响应浏览器前进/后退操作。
- **URL 格式**：
  生成干净的 URL（如 `http://example.com/about`），无 `#` 符号。

#### **BrowserRouter**：

##### **1. 用法**

```
import { HashRouter as Router, Route, Link } from 'react-router-dom';

function App() {
  return (
    <Router>
      <nav>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
      </nav>
      <Route path="/" exact component={Home} />
      <Route path="/about" component={About} />
    </Router>
  );
}
```

- **无需服务器配置**：
  Hash 模式下的 URL（如 `http://example.com/#/about`）会将 `#` 后的路径视为前端路由，服务器仅返回 `index.html`。

##### **2. 原理**

- **基于 Hash 片段**：
  `HashRouter` 通过 URL 的哈希部分（`#`）实现路由，监听 `hashchange` 事件来响应导航。
  - **修改 URL**：通过 `window.location.hash` 改变哈希值。
  - **监听变化**：`hashchange` 事件捕获哈希变化。
- **URL 格式**：
  路径包含 `#`（如 `http://example.com/#/about`）。

#### **三、核心区别对比**

| **特性**           | **BrowserRouter**               | **HashRouter**                   |
| :----------------- | :------------------------------ | :------------------------------- |
| **URL 格式**       | 无 `#`（如 `/about`）           | 含 `#`（如 `/#/about`）          |
| **服务器配置**     | 需配置所有路径返回 `index.html` | 无需配置                         |
| **兼容性**         | 依赖 HTML5 History API（IE10+） | 兼容所有浏览器（包括 IE9）       |
| **SEO 友好性**     | 更友好（支持 SSR 或预渲染）     | 需额外处理（哈希部分可能被忽略） |
| **实现原理**       | History API（pushState 等）     | Hash 片段和 `hashchange` 事件    |
| **直接访问子路由** | 需服务器支持，否则 404          | 直接访问有效                     |
| **典型使用场景**   | 现代浏览器、可控服务器环境      | 静态托管、旧浏览器兼容           |

### （2）路由配置

路由配置就是把要被匹配到的路由都配置好，注意一定要配置

#### 1. 基础配置

```js
// 1. 安装依赖
// npm install react-router-dom

// 2. 定义路由表
import { BrowserRouter as Router, Route, Routes } from 'react-router-dom';
import Home from './Home';
import About from './About';
import User from './User';
import NotFound from './NotFound';

function App() {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/user/:userId" element={<User />} />
        <Route path="*" element={<NotFound />} /> {/* 404 页面 */}
      </Routes>
    </Router>
  );
}
```

可以将路由表抽离为独立文件，通过js写出路由表，再在app.jsx中通过map遍历得到Routes组件。提升代码可维护性

```
// routes.js
export const routes = [
  { path: '/', element: <HomePage /> },
  { path: '/login', element: <LoginPage /> },
  { path: '*', element: <NotFound /> },
];

// App.jsx
import { routes } from './routes';

function App() {
  return (
    <Router>
      <Routes>
        {routes.map((route) => (
          <Route key={route.path} {...route} />
        ))}
      </Routes>
    </Router>
  );
}
```

### **（3）数据路由**

##### 一、核心 API 作用

1. **`createBrowserRouter`**
   创建一个基于 HTML5 History API 的路由实例，支持数据加载、路由嵌套等特性。
   - 参数：路由配置数组（定义路径、组件、数据加载逻辑等）。
   - 返回：路由对象（传递给 `RouterProvider`）。
2. **`RouterProvider`**
   将路由实例注入 React 应用，替代传统的 `<BrowserRouter>` 包裹方式。
   - 参数：`router`（由 `createBrowserRouter` 创建的路由对象）。

##### 二、用法

**定义路由表**

```
// src/routes.js
import { createBrowserRouter } from "react-router-dom";
import Home from "./pages/Home";
import Login from "./pages/Login";
import ErrorPage from "./pages/ErrorPage";

// 定义路由配置
const router = createBrowserRouter([
  {
    path: "/",
    element: <Home />,       // 直接渲染组件
    errorElement: <ErrorPage />, // 全局错误边界
  },
  {
    path: "/login",
    element: <Login />,
    // 可添加数据加载或 Action
    loader: () => fetchUserData(), // 预加载数据
  },
  {
    path: "/user/:userId",
    element: <UserProfile />,
    loader: ({ params }) => fetchUser(params.userId), // 动态参数加载
  },
]);

export default router;
```

 **注入路由到应用**

```
// src/main.jsx
import React from "react";
import ReactDOM from "react-dom/client";
import { RouterProvider } from "react-router-dom";
import router from "./routes";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <RouterProvider router={router} />
  </React.StrictMode>
);
```

##### 三、高级功能实现

1. **数据加载（Data Loading）**

通过 `loader` 函数预加载路由所需数据：

以下是一个典型的数据加载流程：

1. **用户触发导航**（如点击链接 `/user/123`）。
2. React Router **挂起当前 UI**，显示 `Suspense` 的 `fallback`（若有）。
3. **执行目标路由的 `loader` 函数**，获取数据。
4. **数据加载完成后**，渲染目标路由的组件，并将数据通过 `useLoaderData` 传递给组件。
5. 若 `loader` **加载失败**，渲染 `errorElement` 组件。

```
// 路由配置
{
  path: "/posts",
  element: <PostsPage />,
  loader: async () => {
    const posts = await fetch("/api/posts").then(res => res.json());
    return posts; // 数据会传递给组件
  },
}

// 组件中获取数据
import { useLoaderData } from "react-router-dom";

function PostsPage() {
  const posts = useLoaderData(); // 直接获取 loader 返回的数据
  return (
    <div>
      {posts.map(post => <div key={post.id}>{post.title}</div>)}
    </div>
  );
}
```

2. **错误边界（Error Boundary）**

通过 `errorElement` 定义错误处理组件：

```
{
  path: "/dashboard",
  element: <Dashboard />,
  errorElement: <ErrorPage />, // 捕获子路由错误
  children: [
    { path: "profile", element: <Profile /> },
    { path: "settings", element: <Settings /> },
  ],
}
```

3. **懒加载（Code Splitting）**

结合 React 的 `lazy` 和 `Suspense`：

```
// 路由配置
import { lazy } from "react";

const LazyAbout = lazy(() => import("./pages/About"));

{
  path: "/about",
  element: (
    <React.Suspense fallback={<div>Loading...</div>}>
      <LazyAbout />
    </React.Suspense>
  ),
}
```

4. **表单提交与 Action**

通过 `action` 处理表单提交：

```
{
  path: "/login",
  element: <LoginForm />,
  action: async ({ request }) => {
    const formData = await request.formData();
    const email = formData.get("email");
    const password = formData.get("password");
    // 执行登录逻辑
    return loginUser(email, password);
  },
}
```



### （4）路由导航

#### 1. 什么是路由导航

路由系统中的多个路由之间需要进行路由**跳转**，并且在跳转的同时有可能需要传递参数进行通信

分为**声明式导航**和**编程式（命令式）导航**

#### 2. 声明式导航

> 声明式导航是指通过在模版中通过引入的 `<Link/> ` 组件描述出要跳转到哪里去，应用场景主要是菜单模式。点击这个链接会跳转到相应路由
>
> 也可以使用<NavLink>，与<LInk>的区别是具有active状态，所以可以为`active` 状态和非`active` 状态的导航链接添加样式


![image.png](C:\Users\HP\Desktop\React 基础 - 配套资料\React 基础 - day04+day05\React 基础 - day04+day05\02-笔记\assets\5.png)

语法说明：通过给组件的to属性指定要跳转到路由path，组件会被渲染为浏览器支持的a链接，如果需要传参直接通过字符串拼接的方式拼接参数即可

#### 3. 编程式导航

编程式导航是指通过引入的  `useNavigate` 钩子得到导航方法，然后通过调用方法以命令式的形式（调用函数）进行路由跳转，比如想在登录请求完毕之后跳转就可以选择这种方式，更加灵活

![image.png](C:\Users\HP\Desktop\React 基础 - 配套资料\React 基础 - day04+day05\React 基础 - day04+day05\02-笔记\assets\6.png)

语法说明：首先调用useNavigate，再通过调用navigate方法传入地址path实现跳转

### （5）嵌套路由

#### 1. 什么是嵌套路由

在一级路由中又内嵌了其他路由，这种关系就叫做嵌套路由，嵌套至一级路由内的路由又称作二级路由，例如：
![image.png](C:\Users\HP\Desktop\React 基础 - 配套资料\React 基础 - day04+day05\React 基础 - day04+day05\02-笔记\assets\8.png)

#### 2. 嵌套路由配置

> 实现步骤
>
>     1. 在router文件的index.js的一级路由中使用 `children`属性配置路由嵌套关系  
>     2. 先引入Outlet组件 在Layout的index.js中使用 `<Outlet/>` 组件配置二级路由渲染位置,注意二级路由位置由Outlet决定。二级路由需要一二级路径一起写

![image.png](C:\Users\HP\Desktop\React 基础 - 配套资料\React 基础 - day04+day05\React 基础 - day04+day05\02-笔记\assets\9.png)

#### 3. 默认二级路由

当访问的是一级路由时，默认的二级路由组件也可以得到渲染。

方法是只需要对默认二级路由的位置去掉path，设置index属性为true

注意此时跳转到默认的二级路由在Layout的index.js中path只需要一级路径即可	<Link to='/'>面板</Link>

![image.png](C:\Users\HP\Desktop\React 基础 - 配套资料\React 基础 - day04+day05\React 基础 - day04+day05\02-笔记\assets\10.png)

#### 4. 404路由配置

场景：当浏览器输入url的路径在整个路由配置中都找不到对应的 path，为了用户体验，可以使用 404 兜底组件进行渲染

实现步骤：

1. 准备一个NotFound组件
2. 在路由表数组的末尾，以*号作为路由path配置路由，渲染NotFound组件

![image.png](C:\Users\HP\Desktop\React 基础 - 配套资料\React 基础 - day04+day05\React 基础 - day04+day05\02-笔记\assets\11.png)

#### 5. 俩种路由模式

各个主流框架的路由常用的路由模式有俩种，history模式和hash模式, ReactRouter分别由 createBrowerRouter 和 createHashRouter 函数负责创建

| 路由模式 | url表现     | 底层原理                    | 是否需要后端支持 |
| -------- | ----------- | --------------------------- | ---------------- |
| history  | url/login   | history对象 + pushState事件 | 需要             |
| hash     | url/#/login | 监听hashChange事件          | 不需要           |

### （6）动态路由

> 有时路由无法确定（如根据不同用户id来确定相应路由），则成为动态路由
>
> 动态路由需要在设置路由时设置为 /: 形式
>
> #### **路由匹配**
>
> 当用户访问 `/user/123` 时，路由库会根据预先定义的路由规则（如 `/user/:id`）进行匹配。
>
> 路由库通过正则表达式或路径匹配算法，将 URL 中的动态部分（如 `123`）提取为参数 `id`。
>
> #### **参数注入**
>
> 路由库将提取的参数（`{ id: '123' }`）注入到组件的上下文中
>
> **React Router**：通过 `useParams()` 钩子函数返回参数
>
> #### **组件渲染**
>
> - 路由库根据匹配的路由规则，渲染对应的组件（如 `<User />`）。
> - 组件内部可以直接访问参数，无需手动解析。

```javascript
import React from 'react';
import { createBrowserRouter, RouterProvider } from 'react-router-dom';
import HomePage from './pages/HomePage';
import UserPage from './pages/UserPage';
import PostPage from './pages/PostPage';

const router = createBrowserRouter([
  {
    path: "/",
    element: <HomePage />,
  },
  {
    path: "/users/:userId",
    element: <UserPage />,
  },
  // 设置动态路由 /:
  {
    path: "/users/:userId/posts/:postId",
    element: <PostPage />,
  },
]);

const App = () => {
  return <RouterProvider router={router} />;
};

export default App;

```



### （7）导航传参

设置动态路由后，希望获取当前路由的参数

通过命令式传参有searchParams传参和params传参两种形式

通过

![image.png](C:\Users\HP\Desktop\React 基础 - 配套资料\React 基础 - day04+day05\React 基础 - day04+day05\02-笔记\assets\7.png)

> 例如在登录页中传参到文章页
>
> 首先是登录页传递参数：
>
> 1、searchParams传参，对应是路由router后加问号，参数中间由&隔开
>
> 2、params传参，对应是路由router后加斜杠/:，不同参数之间由/隔开

```javascript
import { Link, useNavigate } from 'react-router-dom'
const Login = () => {
  const navigate = useNavigate()
  return (
    <div>
      我是登录页
      {/* 声明式的写法 */}
      <Link to="/article">跳转到文章页</Link>
      {/* 命令式的写法 */}
      <button onClick={() => navigate('/article')}>跳转到文章页</button>
      <button onClick={() => navigate('/article?id=1001&name=jack')}>searchParams传参</button>
      <button onClick={() => navigate('/article/:1001/:jack')}>params传参</button>
    </div>
  )
}

export default Login
```

>接下来是文章页接受参数
>
>1、从react-router-dom中引入useParams或useSearchParams方法
>
>2、调用传参方法
>
>​	1、useSearchParams获得的是数组，解构出来的一个对象params，通过params.get('参数名')可以获得该参数的值
>
>​	2、useParams获得的是对象，通过params.参数名可获得该参数的值

```javascript
import { useParams, useSearchParams } from "react-router-dom"

const Article = () => {
  // const [params] = useSearchParams()
  // const id = params.get('id')
  // const name = params.get('name')

  const params = useParams()
  const id = params.id
  const name = params.name
  return <div>我是文章页{id}-{name}</div>
}

export default Article
```

>注意通过params传参的方法,需要提前在router中设置好传递参数的个数和名字
>
>语法是路由/:参数1:/参数2

```javascript
path: '/article/:id/:name'	// 设置好了id和name两个参数
```

### （8）路由懒加载

lazy函数和Suspense组件-组件中的使用

1、解构lazy、Suspense

```
import { lazy, Suspense } from "react";
```

2、使用lazy配合import引入组件

```
const Child = lazy(() => import("./Child"));
```

3、使用Supense包裹元素，切Supense中还有个属性 fallback ，里面写一个加载过程中显示内容的组件

```
<Suspense fallback={<div>loading...</div>}>

{this.state.isShow && <Child />}

</Suspense>
```

# Hooks

## 1. 什么是 Hooks？它们解决了什么问题？

**Hooks** 是 React 16.8 引入的新特性，允许你在不编写类组件的情况下使用状态和其他 React 特性。Hooks 使得函数组件可以拥有状态和生命周期方法，从而提高了代码的可读性和可维护性。

**解决的问题**：

- **状态管理**：在函数组件中管理状态，而不需要转换为类组件。
- **生命周期方法**：在函数组件中使用生命周期方法，而不需要编写复杂的类组件。
- **逻辑复用**：通过自定义 Hooks 复用组件逻辑，提高代码复用性。
- **代码简洁**：使得函数组件的代码更加简洁和易读。

## 2. 请列举并简要说明 React 中常用的 Hooks。

**常用 Hooks**：

- **`useState`**：用于在函数组件中添加状态。
- **`useEffect`**：用于在函数组件中执行副作用操作，如数据获取、订阅或手动更改 DOM。
- **`useContext`**：用于在函数组件中访问 React 的 Context。
- **`useReducer`**：用于在函数组件中管理复杂的状态逻辑。
- **`useMemo`**：用于 memoize 计算结果，避免不必要的计算。
- **`useCallback`**：用于 memoize 回调函数，避免不必要的重新渲染。
- **`useRef`**：用于在函数组件中创建一个可变的引用对象。
- **`useLayoutEffect`**：与 `useEffect` 类似，但在所有的 DOM 变更之后同步调用。

## 3、useState

**`useState`** 是一个 Hook，用于在函数组件中添加状态。

**工作原理**：

- **初始化状态**：第一次调用 `useState` 时，传入的初始值会被用作初始状态。如果括号内是函数，函数的返回值作为 useState 初始化的值。
- **更新状态**：返回一个数组，第一个元素是当前状态，第二个元素是一个用于更新状态的函数。

**useState 注意事项：**

① 在函数组件一次执行上下文中，state 的值是固定不变的。

② 如果两次 dispatchAction 传入相同的 state 值，那么组件就不会更新。

③ 当触发 dispatchAction 在当前执行上下文中获取不到最新的 state, 只有再下一次组件 rerender 中才能获取到。

 4  当更改引用数据类型时，应该创建一个新的对象或数组，以保持状态的不可变性。可以使用扩展运算符来实现。

```
返回值：是一个数组，第一个参数是变量，第二个参数是用于改变变量的方法，执行函数时可以传入一个默认值
//引入
    import React, { useState } from "react";
//使用并传入初始值
      const [count, set_count] = useState(5);    // 数组的解构依赖索引，所以用数组解构可以任意变量名
//改变数据
      const fn = () => {
        set_count(fn())
      }        
//整体使用
    const App = () => {
      const [count, set_count] = useState(5);
      const plus = () => {
        set_count((count) => count + 1);
      };
      const minus = () => {
        set_count((count) => count - 1);
      };
      return (
        <>
          <button onClick={minus}>-</button>
          <span>{count}</span>
          <button onClick={plus}>+ </button>
        </>
      );
    };    

// 当我们需要修改部分属性如name，并且不改变其他属性时，我们可以传入一个回调函数：
    setState((prev) => {return 
    	{  ...prev,  name: 'xxxx'  }
    });
```

## 4、useEffect

**`useEffect`** 是一个 Hook，用于在函数组件中执行副作用操作，如数据获取、订阅或手动更改 DOM。

**工作原理**：

第一个参数为回调函数，执行副作用

- **执行副作用**：在组件挂载和更新时执行副作用操作。
- **清理副作用（可选）**：return一个清理函数，用于在组件卸载或下次执行副作用前清理上一次的副作用。

第二个参数作为依赖项，useEffect的执行与否取决于依赖项的变化

**useEffect依赖说明** 

useEffect副作用函数的执行时机存在多种情况，根据传入依赖项的不同，会有不同的执行表现

| **依赖项**     | **副作用功函数的执行时机**                                   |
| -------------- | ------------------------------------------------------------ |
| 没有依赖项     | 组件初始渲染 + 组件更新时执行（组件更新指组件内的所有状态变量更新都会执行） |
| 空数组依赖     | 只在初始渲染时执行一次                                       |
| 添加特定依赖项 | 组件初始渲染 + 依赖项变化时执行 （只有绑定的依赖项状态变量更新才会执行） |

## 5、useMemo

- **作用**：用于 缓存 计算结果，避免不必要的计算。
- **工作原理**：传入一个计算函数和一个依赖数组，只有当依赖数组中的值发生变化时，才会重新计算。

**useMemo 基础介绍：**

```js
const cacheSomething = useMemo(create,deps)
```

- ① 第一个参数为一个函数，函数的返回值作为缓存值
- ② 第二个参数为一个数组，存放当前 useMemo 的依赖项，规则与useEffect相同

**应用场景：**

缓存计算结果：

```js
function Scope(){
    const style = useMemo(()=>{
      let computedStyle = {}
      // 经过大量的计算
      return computedStyle
    },[])
    return <div style={style} ></div>
}
```

缓存组件,减少子组件 rerender 次数：

```js
function Scope ({ children }){
   const renderChild = useMemo(()=>{ children()  },[ children ])
   return <div>{ renderChild } </div>
}
```

## 6、useCallback

- **作用**：用于 缓存 回调函数，避免不必要的重新渲染。
- **工作原理**：传入一个回调函数和一个依赖数组，只有当依赖数组中的值发生变化时，才会返回一个新的回调函数。

## 7、useReducer

用于 **管理复杂的状态逻辑**，特别适用于 **多个子状态相互关联或有复杂更新逻辑的情况**

```
const [state, dispatch] = useReducer(reducer, initialState);
```

- `reducer`：一个**纯函数**，接收 `state` 和 `action`，返回新的 `state`。
- `initialState`：状态的初始值。
- `dispatch`：一个函数，调用时传入 `action`，触发 `reducer` 更新 `state`。

高级版的useEffect，效果类似于redux

**应用场景**：

| 状态依赖 `action` 更新，如 `dispatch({ type: "add" })` | `useReducer` |
| ------------------------------------------------------ | ------------ |
| 复杂状态，如多个状态关联                               | `useReducer` |
| 需要对状态变化进行**调试（log）**                      | `useReducer` |

## 8、useRef

**`useRef`** 是一个 Hook，用于在函数组件中创建一个可变的引用对象。

**工作原理**：

- **创建引用**：返回一个可变的引用对象，其 `.current` 属性可以保存任何值。
- **持久化值**：引用对象在组件的整个生命周期内保持不变，可以用于保存 DOM 节点、计时器等。

**基础用法**

1、useRef 获取 DOM 元素

首先初始化，然后将DOM元素的ref绑定到

```
const myRef = useRef(null)
return <div>
        {/* ref 标记当前dom节点 */}
        <div ref={myRef} >表单组件</div>
    </div>
}
```

2、useRef 保存状态

通过更改.current属性更改ref值，可以利用 useRef 返回的 ref 对象来保存状态，只要当前组件不被销毁，那么状态就会一直存在。

```js
const status = useRef(false)
/* 改变状态 */
const handleChangeStatus = () => {
  status.current = true
}
```

## 9、useContext

**`useContext`** 是一个 Hook，用于在函数组件中访问 React 的 Context。

**工作原理**：

- **访问 Context**：传入一个 Context 对象，返回该 Context 的当前值。
- **订阅变化**：当 Context 的值发生变化时，使用 `useContext` 的组件会重新渲染。

可以使用 useContext ，来获取父级组件传递过来的 context 值，这个当前值就是最近的父级组件 Provider 设置的 value 值

可以父级上下文 context 传递的 ( 参数为 context )

useContext 可以代替 context.Consumer 来获取 Provider 中保存的 value 值。

## 10、useLayoutEffect

- 首先 useLayoutEffect 是在 DOM 更新之后，浏览器绘制之前执行，这样浏览器只会绘制一次

- useEffect 执行是在浏览器绘制视图之后，接下来又改 DOM ，就可能会导致浏览器再次回流和重绘，消耗性能。绘制两次视图上可能会造成闪现突兀的效果。

- 采用了同步执行，callback 中代码执行会阻塞浏览器绘制

## 11、useInsertionEffect

useInsertionEffect 的执行的时候，DOM 还没有更新，确保样式在首次渲染时就已存在，避免了闪屏或未应用样式的问题。

在运行时注入 `<style>` 标签可能会导致浏览器频繁地重新计算样式，影响性能。

本质上 useInsertionEffect 主要是解决 CSS-in-JS 在渲染中注入样式的性能问题，用于在 React 对 DOM 进行更改之前插入动态 CSS。

通过 useInsertionEffect 配合 StyleSheetManager，可以确保生成的 CSS 被插入到预先创建的 style 标签中，进而精确控制样式注入的时机和顺序。

useInsertionEffect 执行 -> useLayoutEffect 执行 -> useEffect 执行

```
import React, { memo, useInsertionEffect, useRef } from 'react';
import { StyleSheetManager } from 'styled-components';
import { BannerWrapper } from './style';

const HomeBanner = memo(() => {
  const styleElementRef = useRef(null);

  // useInsertionEffect 在 DOM 变更前创建 <style> 标签
  useInsertionEffect(() => {
    const styleEl = document.createElement('style');
    document.head.appendChild(styleEl);
    styleElementRef.current = styleEl;
    return () => {
      document.head.removeChild(styleEl);
    };
  }, []);

  // 在 styleElementRef.current 创建之前不渲染
  if (!styleElementRef.current) return null;

  return (
    <StyleSheetManager target={styleElementRef.current}>
      <BannerWrapper>
        {/* <img src={coverImg} alt="" /> */}
      </BannerWrapper>
    </StyleSheetManager>
  );
});

export default HomeBanner;

```

## 12、自定义HOOKS

**自定义 Hooks** 是一种将逻辑提取到可重用函数中的方法。自定义 Hooks 以 `use` 开头，可以组合多个内置 Hooks，封装复杂逻辑。

项目中的例子：

```
function useScrollPosition() {
    // 状态来记录位置，记录滚动到哪里
    const [scrollX, setScrollX] = useState(0)
    const [scrollY, setScrollY] = useState(0)

    // 监听window滚动
    useEffect(() => {
        // 一滚动就会设置新的值，所以通过underscore的throttle实现节流操作
        const handleScroll = throttle(function () {
            setScrollX(window.scrollX)
            setScrollY(window.scrollY)
        }, 100)
        // 给视窗滚动事件绑定handleScroll函数，记录当前位置
        window.addEventListener("scroll", handleScroll)
        // 结束时撤销绑定
        return () => {
            window.removeEventListener("scroll", handleScroll)
        }
    }, [])

    // 返回x轴和y轴滚动了多少
    return { scrollX, scrollY }
}
```

# React18 新增特性

#### 一. React18介绍

##### 1. 新的项目创建

```shell
npx create-react-app my-app
cd my-app
npm start
```

##### 2. 老的项目升级

```powershell
//先把依赖中的版本号改成最新，然后删掉 node_modules 文件夹，
//"react": "^18.2.0",
//"react-dom": "^18.2.0"

npm i 
```

如果您在使用 Node.js 17 的应用程序中遇到ERR_OSSL_EVP_UNSUPPORTED错误，很可能是您的应用程序或您正在使用的模块尝试使用 OpenSSL 3.0 默认不再允许的算法或密钥大小。

```shello
$env:NODE_OPTIONS="--openssl-legacy-provider"
```



#### 二. Render API

> React 18 引入了一个新的 root API`ReactDOM.createRoot` 来创建根节点，支持并发模式的渲染

**老的写法**

```jsx
import ReactDOM from 'react-dom';
ReactDOM.render(<App/>,document.getElementById("root"))
```

**新的写法**

```jsx
import ReactDOM from 'react-dom/client';
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

![image-20221012111108121](C:\Users\HP\Desktop\我的资源\资料\笔记\笔记.assets\image-20221012111108121.png)

#### 三.自动批量更新State

> React 18 默认开启批处理来实现性能提升。

在 React 18 之前，仅 React 事件处理函数中的状态更新会被批处理；现在所有场景（如 `setTimeout`、`Promise`、原生事件）的更新都会自动合并为一次渲染。

支持批处理：

- React 事件处理函数
- promise
- setTimeout
- 原生事件处理

> 不要开启严格模式测试

##### 1. setTimeout

```js
/*
 * @作者: kerwin
 */
import React, { useState } from 'react'

export default function App() {

    console.log("render")
    const [name,setName] = useState("kerwin")
    const [age,setAge] = useState(100)
    return (
        <div>
            {name}-{age}
            <button onClick={() => {
                setTimeout(() => {
                    setName("xiaoming")
                    setAge(18)
                },0)
            }}>click</button>
        </div>
    )
}

```



##### 2. promise

```js
/*
 * @作者: kerwin
 */
import React, { useState } from 'react'

const ajax = ()=>{
    return new Promise((resolve)=>{
        resolve()
    })
}
export default function App() {

    console.log("render")
    const [name,setName] = useState("kerwin")
    const [age,setAge] = useState(100)
    return (
        <div>
            {name}-{age}
            <button onClick={() => {
                ajax().then(res=>{
                    setName("xiaoming")
                    setAge(18)
                })
            }}>click</button>
        </div>
    )
}

```



##### 3.原生事件

```jsx
/*
 * @作者: kerwin
 */
import React, { useState,useEffect } from 'react'

export default function App() {

    console.log("render")
    const [name,setName] = useState("kerwin")
    const [age,setAge] = useState(100)

    useEffect(() => {
        document.body.addEventListener("click",()=>{
            console.log("click")
            setName("xiaoming")
            setAge(18)
        },false)
    }, [])
    return (
        <div>
            {name}-{age}
        </div>
    )
}

```



##### 4. flushSync

```js
flushSync(()=>{
    setName("xiaoming")
})
flushSync(()=>{
    setAge(18)
})
```





#### 四. Concurrent Mode（并发模式）

- 在之前的React 状态变更后，会开始准备虚拟 DOM，然后渲染真实 DOM，整个流程是串联的。一旦开始触发更新，只能等流程结束，期间是无法被中断的。

- 在 并发模式下，React 在执行过程中，每执行一个 Fiber，都会检查有没有更高优先级的更新，如果有，则的暂停当前低优先级，待高优先级任务执行完之后，再继续执行或重新执行

- **优先级调度**：
  - **紧急更新**：用户交互（点击、输入等）需即时响应。
  - **过渡更新**：UI 切换或非关键数据更新，使用 `startTransition` 或 `useTransition` 标记。


<img src="C:\Users\HP\Desktop\我的资源\资料\笔记\笔记.assets\image-20221013103206972.png" alt="image-20221013103206972" style="zoom:50%;float:left" />

<img src="C:\Users\HP\Desktop\我的资源\资料\笔记\笔记.assets\image-20221013095624110.png" alt="image-20221013095624110" style="zoom:50%;float:left" />



##### 1. useTransition

React 的状态更新可以分为两类：

- 紧急更新（Urgent updates）：比如打字、点击、拖动等，需要立即响应的行为，如果不立即响应会给人很卡的感觉。
- 过渡更新（Transition updates）：将 UI 从一个视图过渡到另一个视图。有些延迟，不立即相应是可以接受的。

**并发模式只是提供了可中断的能力，默认情况下，所有的更新都是紧急更新。**

所以它提供了 `startTransition`让我们**手动指定**哪些更新是**紧急**的，哪些是**非紧急**的。

非紧急的更新可以被延迟，这里延迟的**不是状态的更新**，而是状态更新后**延迟重新渲染**的过程。state状态会**马上更新**

```jsx
/*
 * @作者: kerwin
 */
import React, { useState, useEffect, useTransition } from 'react';

function List(props) {
    const [list, setList] = useState([]);
    useEffect(() => {
        setTimeout(()=>{
            setList(new Array(5000).fill(""));
        },100)
    }, [props.search]);
    return (
        <ul>
            {list.map((item, index) => (
                <Item key={index} value={index+"_"+props.search}></Item>
            ))}
        </ul>
    );

}

function Item(props) {
    return <li>
        {props.value}
    </li>
}

export default function App() {
    const [search, setSearch] = useState("")
    const [text, setText] = useState("")
    const [isPending, startTransition] = useTransition();

    return (
        <div>
            <input value={text} onChange={(evt) => {
                setText(evt.target.value)
                startTransition(()=>{
                    setSearch(evt.target.value)
                })
            }} /> {isPending && "等待..."}

            <List search={search} />
        </div>
    )
}


```



##### 2. useDeferredValue

这个方法返回一个**延迟响应的值**，可以让一个`state` **延迟生效**，只有当前没有紧急更新时，该值才会变为最新值。`useDeferredValue` 和 `startTransition` 一样，都是标记了一次非紧急任务更新。

- 相同：`useDeferredValue` 本质上和与 `useTransition` 内部实现一样，都是标记成了`延迟更新`任务。
- 不同：`useTransition` 是把**更新任务**变成了**延迟更新任务**，而 `useDeferredValue` 把一个**状态**变成**延迟的状态**。

```js
export default function App() {
    const [search, setSearch] = useState("")
    const [text, setText] = useState("")
    const [isPending, startTransition] = useTransition();

    return (
        <div>
            <input value={text} onChange={(evt) => {
                setText(evt.target.value)
                setSearch(evt.target.value)
            }} /> {isPending && "等待..."}

            <List search={useDeferredValue(search)} />
        </div>
    )
}
```



#### 五. 严格模式（muted colors）

![image-20221013181042367](C:\Users\HP\Desktop\我的资源\资料\笔记\笔记.assets\image-20221013181042367.png)



#### 六. Suspense组件的变化

> - **Fallback 处理变更**：未提供 `fallback` 的 Suspense 组件会渲染 `null`，而非向上查找父级边界。
>
> - **支持数据获取**：Suspense 不仅用于组件懒加载，还可等待异步数据。
> - **改进错误处理**：结合错误边界（Error Boundaries）实现更优雅的错误回退。

##### 1. 版本17

```jsx
<Suspense fallback={<Loading />}>   // <--- this boundary is used
  <Suspense>                        // <--- this boundary is skipped, no fallback
    <Page />
  </Suspense>
</Suspense>
```

##### 2. 版本18

```jsx
<Suspense fallback={<Loading />}>   // <--- not used
  <Suspense>                        // <--- this boundary is used, rendering null for the fallback
    <Page />
  </Suspense>
</Suspense>

```

**Suspense源码**

```jsx
class Suspense extends React.Component { 
    state = { promise: null } 
    componentDidCatch(e) { 
        if (e instanceof Promise) { 
            this.setState(
            { promise: e }, () => { 
                e.then(() => { 
                    this.setState({ promise: null }) 
                }) 
            }) 
        } 
    } 
    render() { 
        const { fallback, children } = this.props 
        const { promise } = this.state 
        return <> 
            { promise ? fallback : children } 
        </> 
    } 
}
```



#### 七. React 空组件的返回值

如果React返回一个`空组件`，React17只允许返回`null` 。React18 也允许返回undefined.

```jsx
function Animal({type}) {
  if (type === 'cat') {
    // Will error, missing return statement.
    // This is always a mistake.
    <Cat />;
  }

  // Will error, returns undefined.
  // Did you mean to return nothing,
  // or did you forget to return a default?
  return;
}

```



#### 八.组件卸载更新状态的警告删除

> 组件已经卸载，此时并没有内存泄漏，因为已经被垃圾回收机制回收了，此时，**警告具有误导性**，所以删除了。

![image-20221014101056972](C:\Users\HP\Desktop\我的资源\资料\笔记\笔记.assets\image-20221014101056972.png)

#### 九、新 Hooks 与 API

1. **`useId`**
   生成唯一 ID，解决服务端与客户端渲染的 ID 不一致问题。

   ```
   const id = useId(); // 输出唯一标识符
   ```

2. **`useInsertionEffect`**
   适用于 CSS-in-JS 库，在 DOM 更新后、布局前注入样式。

3. **`useTransition` 与 `startTransition`**
   管理过渡任务的状态（如加载提示）和优先级。

# React19新增特性

## 1、Actions

在 React 应用中，一个常见的用例是执行数据变更，然后响应更新状态。例如，当用户提交一个表单来更改他们的名字，你会发起一个 API 请求，然后处理响应。在过去，你需要手动处理待定状态、错误、乐观更新和顺序请求。

在 React 19 中，我们正在添加在过渡中使用异步函数的支持，以自动处理待定状态、错误、表单和乐观更新。

### （1）useTransition

```
const [isPending, startTransition] = useTransition();
```

isPending是状态，startTransition包裹用到状态的Promise。

这样就不用再设置判断状态的变量了，直接使用useTransition提供的isPending，判断的是startTransition包裹的Promise函数。

```
// 使用 Actions 中的待定状态
function UpdateName({}) {
  const [name, setName] = useState("");
  const [error, setError] = useState(null);
  const [isPending, startTransition] = useTransition();

  const handleSubmit = () => {
    startTransition(async () => {
      const error = await updateName(name);
      if (error) {
        setError(error);
        return;
      } 
      redirect("/path");
    })
  };

  return (
    <div>
      <input value={name} onChange={(event) => setName(event.target.value)} />
      <button onClick={handleSubmit} disabled={isPending}>
        Update
      </button>
      {error && <p>{error}</p>}
    </div>
  );
}
```

### （2）`useActionState` 

`useActionState` 是一个可以根据某个表单动作的结果更新 state 的 Hook。

语法：`useActionState(fn, initialState, permalink?)`

#### 参数 

- `fn`：当按钮被按下或者表单被提交时触发的函数。当函数被调用时，该函数会接收到表单的上一个 state（初始值为传入的 `initialState` 参数，否则为上一次执行完该函数的结果）作为函数的第一个参数，余下参数为普通表单动作接到的参数。
- `initialState`：state 的初始值。任何可序列化的值都可接收。当 action 被调用一次后该参数会被忽略。

#### 返回值 

`useActionState` 返回一个包含以下值的数组：

1. 当前的 state。第一次渲染期间，该值为传入的 `initialState` 参数值。在 action 被调用后该值会变为 action 的返回值。

2. 一个新的 action 函数用于在你的 `form` 组件的 `action` 参数或表单中任意一个 `button` 组件的 `formAction` 参数中传递。这个 fn 也可以手动在 [`startTransition`](https://zh-hans.react.dev/reference/react/startTransition) 中调用。

3. 一个 `isPending` 标识，用于表明是否有正在 pending 的 Transition。

   用法：

   ```
   import { useActionState } from 'react';
   import { fn } from './actions.js';
   
   function MyComponent() {
     const [state, action, isPending] = useActionState(fn, null);
     // ...
     return (
       <form action={action}>
         {/* ... */}
       </form>
     );
   }
   ```

   ### React DOM: `<form>` Actions 

   Actions 也与 React 19 的新 `<form>` 功能集成在 `react-dom` 中。我们已经添加了对将函数作为 `<form>`、`<input>` 和 `<button>` 元素的 `action` 和 `formAction` 属性的支持，以便使用 Actions 自动提交表单：

   ```
   <form action={actionFunction}>
   ```