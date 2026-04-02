### 一、Vue 3 的常用 API

**`ref` 和 `reactive`**

- **`ref`**：用于创建基本数据类型（如字符串、数字、布尔值等）的响应式引用。`ref` 通过 `.value` 来访问和修改数据。
- **`reactive`**：用于创建对象或数组的响应式数据，直接对对象进行响应式代理。修改对象的属性时，可以直接通过属性名来访问和修改。

**`computed`**

- 用于创建计算属性，计算属性会基于它所依赖的响应式数据进行缓存。只有依赖的响应式数据发生变化时，才会重新计算。适用于衍生数据或需要基于其他数据计算的场景。

**`watch`**

- 用于监听某个响应式数据的变化，并执行副作用操作。适合用于异步操作或其他副作用（如数据存储、API 请求等）。

**生命周期函数（组合式 API）**

- `onMounted`、`onUpdated`、`onUnmounted`：这些生命周期函数在组件的不同生命周期阶段执行，类似于 Vue 2 的生命周期钩子。
- 它们与 Vue 2 的选项式 API 生命周期钩子不同，采用了组合式 API，更加灵活和清晰。

**`defineComponent`**

- 用于定义组件，是 Vue 3 的标准组件定义方式。它支持组合式 API 和 TypeScript 类型推导，增强了组件的可读性和类型安全。

**`provide` 和 `inject`**

- `provide` 和 `inject` 是 Vue 3 中用于跨层级组件通信的 API。父组件通过 `provide` 提供数据，子组件通过 `inject` 获取数据，适用于多层级嵌套的组件树中。

**`nextTick`**

- 用于在 DOM 更新后执行某些操作。通常在修改数据后，Vue 会异步更新 DOM，而 `nextTick` 可以保证 DOM 更新完成后执行回调函数。

**`useContext`**

- 用于获取当前组件的上下文，适用于函数式组件中访问 Vue 的上下文信息。

------

### 二、`ref` 和 `reactive` 的区别

- **`ref`** 用于创建基本数据类型的响应式引用，它会包装原始数据，如字符串、数字等，访问时需要通过 `.value`。
- **`reactive`** 用于创建对象或数组的响应式数据，直接对对象进行响应式代理，可以直接通过属性名来访问。

这两者可以结合使用，例如：

```ini
ini 体验AI代码助手 代码解读复制代码const count = ref(0);
const user = reactive({ name: 'Alice', age: 25 });
```

------

### 三、`computed` 和 `watch` 的区别

- **`computed`**：适用于需要衍生数据或缓存计算结果的场景。它只会在依赖的数据变化时重新计算，避免不必要的性能开销。
  - 适用场景：表单的校验规则、动态标题、数据转换等。
- **`watch`**：用于侦听响应式数据的变化，并执行副作用操作。适用于异步操作或复杂的副作用，如数据提交、API 请求等。
  - 适用场景：请求外部 API、表单提交、页面跳转等。

------

### 四、Vue 2 的 `mixin` 和 Vue 3 的 `hooks` 的相似之处和区别

**相似之处**

- **`mixin` 和 `hooks`** 都可以用于逻辑复用，将多个组件中共享的功能提取到外部，使得代码更加模块化和可维护。

**区别**

- **Mixin**：Vue 2 的 mixin 是将多个组件选项中的逻辑混入当前组件，可能导致命名冲突或代码可维护性差，尤其在大型应用中。
- **Hooks（Vue 3）** ：Vue 3 提供的组合式 API 允许更加灵活和模块化的代码组织，避免了 mixin 中的命名冲突，逻辑更加清晰和可组合。

------

### 五、`v-if` 和 `v-for` 的使用及 `key` 的作用

- **`v-if`**：用于根据条件渲染 DOM 元素。适用于动态显示/隐藏元素。
- **`v-for`**：用于渲染列表，通常与数组或对象结合使用进行渲染。
- **`key`**：在 `v-for` 中使用 `key` 来标识每个节点。`key` 有助于 Vue 更高效地更新 DOM，确保虚拟 DOM 的正确复用，从而提高性能。

------

### 六、**Vue 3 组件生命周期及替代 API**

在 Vue 3 中，组件生命周期方法与 Vue 2 略有不同。Vue 2 中的生命周期方法包括：`beforeCreate`、`created`、`beforeMount`、`mounted`、`beforeUpdate`、`updated`、`beforeDestroy`、`destroyed`。这些方法描述了组件从创建到销毁的各个阶段。

- **beforeCreate**: 在实例创建之前被调用。
- **created**: 实例创建后调用，数据观测和事件配置已完成，但 DOM 尚未挂载。
- **beforeMount**: 在挂载开始之前调用，此时 DOM 尚未被渲染。
- **mounted**: 组件挂载完成后调用，DOM 已渲染并可访问。
- **beforeUpdate**: 在组件数据变化时调用，DOM 尚未更新。
- **updated**: 在组件数据变化后，DOM 更新完毕后调用。
- **beforeDestroy**: 在组件销毁前调用。
- **destroyed**: 在组件销毁后调用。

在 Vue 3 中，组合式 API 替代了传统的生命周期方法。常用的替代方法包括：

- `onMounted`: 替代 `mounted`，用于在组件挂载完成后执行操作。
- `onUpdated`: 替代 `updated`，用于在组件更新后执行操作。

通过组合式 API，生命周期方法可以在多个 `setup()` 中进行逻辑复用，增强了代码的可维护性和复用性。

### 七、**Vue 2 与 Vue 3 的区别**

- **性能**：Vue 3 引入了更高效的虚拟 DOM 和响应式系统，整体性能得到了大幅提升。特别是在大型应用中，Vue 3 的响应式性能比 Vue 2 更优。
- **组合式 API**：Vue 3 增加了组合式 API（Composition API），让开发者能通过 `setup()` 函数集中定义响应式状态、计算属性和生命周期钩子，提升了逻辑复用性和代码的可读性。
- **TypeScript 支持**：Vue 3 对 TypeScript 的支持更加完善，提供了更好的类型推导和静态类型检查，增强了开发体验。
- **响应式系统**：Vue 3 使用了基于 `Proxy` 的响应式系统，相比于 Vue 2 中使用的 `Object.defineProperty`，`Proxy` 提供了更高的性能和更强的灵活性，能够更好地处理嵌套对象和数组。

### 八、**Vue 2 响应式与 Vue 3 响应式的区别**

- **Vue 2**: 使用 `Object.defineProperty` 通过劫持对象的 getter 和 setter 来实现响应式。这种方式的缺点包括无法检测到数组索引的变化，且对于嵌套对象的处理也相对有限。
- **Vue 3**: 使用 `Proxy` 实现响应式，提供了更强大的功能。`Proxy` 可以直接代理整个对象，支持嵌套对象、数组及其变化的检测，并且性能也比 `Object.defineProperty` 更好。

### 九、**Vue 组件通信**

Vue 提供了多种方式来实现组件间的通信，主要包括以下几种：

- **父子组件通信**：
  - 父组件通过 `props` 向子组件传递数据。
  - 子组件通过 `$emit` 向父组件发送事件，实现双向数据流。
- **兄弟组件通信**：
  - 通过公共父组件传递数据，父组件将数据传递给两个兄弟组件。
  - 或者通过 `provide` 和 `inject` 实现祖先和后代组件之间的通信。
- **跨层级通信**：
  - 可以使用状态管理库，如 `Vuex` 或 `Pinia`，来管理全局状态，确保跨层级的组件都能访问和修改共享的数据。
  - 事件总线也是一种常见的跨层级通信方式，虽然在 Vue 3 中已经不推荐使用。

### 十、**Vuex 与 Pinia 的区别**

- **Vuex**：Vuex 是 Vue 2.x 的官方状态管理库。它使用 `mutations` 和 `actions` 来处理状态变更，管理全局状态，并且需要通过显式的 `commit` 和 `dispatch` 操作。
- **Pinia**：Pinia 是 Vue 3 推荐的状态管理库。它基于组合式 API，支持更好的类型推导、响应式状态管理，并且 API 更加简洁。Pinia 在 Vue 3 中的表现更自然，并且对于 TypeScript 的支持也更加友好。

总结：

- **Vuex** 适用于 Vue 2 项目，而 **Pinia** 更适合 Vue 3 项目。
- **Pinia** 提供了更简洁和现代化的 API，更好地集成了 Vue 3 的组合式 API。

------

### 十一、**选项式 API 和 组合式 API 的区别**

- **选项式 API**：
  - 通过组件的选项（如 `data`, `methods`, `computed`, `watch` 等）来组织逻辑。
  - 结构化明确，但对于大型或复杂的应用来说，代码容易变得繁琐且不易维护。
  - 适用于小型到中型的项目，易于上手。
- **组合式 API**：
  - 通过函数来组织组件的逻辑，使用 `setup()` 函数进行组合。
  - 逻辑复用性更强，可以按功能拆分逻辑，提高可维护性和可扩展性。
  - 特别适用于复杂的组件，能够提高代码的模块化程度，增强可读性和测试性。

**总结**：选项式 API 适合小型或中型项目，而组合式 API 更适合复杂或大型项目，且具有更高的灵活性和可复用性。

### 十二、**v-show 和 v-if 的区别**

- **`v-show`**：
  - 仅通过修改元素的 `display` 样式来控制元素的显示和隐藏。
  - 当切换频繁时，使用 `v-show` 比 `v-if` 性能更优，因为它不涉及元素的销毁和重新创建。
  - 适用于需要频繁切换的场景。
- **`v-if`**：
  - 根据条件判断是否渲染该元素，条件不成立时，元素将从 DOM 中完全移除。
  - 初次渲染时性能较差，尤其是在条件发生变化时，Vue 会销毁并重新创建 DOM 元素。
  - 适用于不常切换的元素。

**总结**：`v-show` 适合频繁切换的场景，`v-if` 适合不常切换的元素，能有效优化性能。

### 十三、**Vue 中的虚拟 DOM，diff 算法**

- **虚拟 DOM**：
  - Vue 使用虚拟 DOM 来优化页面渲染过程。当组件的数据发生变化时，Vue 会首先创建一个虚拟 DOM 树，而不是直接修改真实的 DOM。
- **Diff 算法**：
  - 当数据发生变化时，Vue 会用新生成的虚拟 DOM 树与旧的虚拟 DOM 树进行比较（diff）。它通过最小化的更新策略，计算出最小的 DOM 变更，最终应用到真实的 DOM 上。
  - 这一过程显著提升了渲染性能，尤其是在数据频繁变动的场景中。

**总结**：虚拟 DOM 和 Diff 算法的结合使得 Vue 在进行 DOM 更新时更加高效，减少了不必要的 DOM 操作，提高了性能。

### 十四、**v-bind 和 v-model**

- **`v-bind`**：
  - 用于动态绑定 HTML 属性或组件的特性，允许我们将一个变量或表达式的值绑定到属性上。例如：`v-bind:href="url"`。
  - 适用于需要动态改变属性值的场景。
- **`v-model`**：
  - 实现表单元素的双向数据绑定，常用于 `input`、`textarea` 等表单元素。它会自动绑定元素的 `value` 和相应的事件（如 `input` 事件）来同步更新数据。
  - 在 Vue 3 中，`v-model` 也可以用于自定义事件的双向绑定。

**总结**：`v-bind` 用于单向绑定属性，而 `v-model` 用于双向数据绑定，特别适合表单元素的交互。

### 十五、**Vue 3 里面的劫持有哪些**

- Vue 3 使用 **Proxy** 来实现响应式系统，取代了 Vue 2 中的 `Object.defineProperty`。
  - Proxy 可以拦截对象的读取、设置、删除等操作，并通过代理机制来触发视图的更新。
  - 通过对对象进行代理，Vue 3 可以更高效地追踪数据变化，并实现响应式更新。

**总结**：Vue 3 通过 `Proxy` 实现对数据的劫持和响应式更新，较 Vue 2 提升了性能并简化了代码逻辑。

------

### 十六、**Vue 的插槽**

Vue 插槽是一个强大的功能，允许父组件将内容传递给子组件，增加了组件的灵活性和可复用性。插槽有三种主要类型：

1. **默认插槽**：子组件定义一个插槽，父组件可以在该插槽中传递内容。子组件会渲染父组件传递的内容。

   ```xml
   xml 体验AI代码助手 代码解读复制代码<!-- 父组件 -->
   <child-component>
     <p>这是父组件传递的内容</p>
   </child-component>
   
   <!-- 子组件 -->
   <template>
     <slot></slot>  <!-- 显示父组件的内容 -->
   </template>
   ```

2. **具名插槽**：子组件定义多个具名插槽，父组件可以根据名称来传递不同的内容，增强了插槽的灵活性。

   ```xml
   xml 体验AI代码助手 代码解读复制代码<!-- 父组件 -->
   <child-component>
     <template v-slot:header>
       <h1>这是头部</h1>
     </template>
     <template v-slot:footer>
       <footer>这是底部</footer>
     </template>
   </child-component>
   
   <!-- 子组件 -->
   <template>
     <slot name="header"></slot>
     <slot name="footer"></slot>
   </template>
   ```

3. **作用域插槽**：作用域插槽允许父组件传递数据到插槽中，这样父组件能够通过插槽访问子组件的数据或状态。

   ```xml
   xml 体验AI代码助手 代码解读复制代码<!-- 父组件 -->
   <child-component>
     <template v-slot:default="slotProps">
       <p>插槽数据: {{ slotProps.message }}</p>
     </template>
   </child-component>
   
   <!-- 子组件 -->
   <template>
     <slot :message="message"></slot>  <!-- 传递数据给插槽 -->
   </template>
   <script>
   export default {
     data() {
       return { message: '这是子组件传递的消息' };
     }
   }
   </script>
   ```

### 十七、**Vue 追踪机制**

Vue 的响应式系统利用 **依赖收集** 和 **更新机制**，确保当数据变化时，相关的视图或组件会自动重新渲染。Vue 通过 `Proxy` 实现数据的响应式，当数据属性发生变化时，Vue 会自动触发相应的视图更新。

Vue 追踪机制的核心是：

- **依赖收集**：当组件渲染时，它会读取使用的数据属性并将其注册到依赖队列中，确保当这些数据发生变化时，能够触发更新。
- **懒更新**：只有数据发生变化时，相关的 DOM 会重新渲染，减少不必要的性能开销。

### 十八、**Vue SSR 的实现原理**

Vue SSR（服务器端渲染）通过在服务器端渲染页面，再将渲染后的 HTML 发送给客户端，可以显著提升 SEO 和首屏加载性能。在 Vue 3 中，SSR 通过 `renderToString` 和 `createApp` 实现。

- **`createApp`**：用于创建 Vue 应用的实例。
- **`renderToString`**：将 Vue 应用渲染为 HTML 字符串并返回给客户端。

这种机制让客户端能够尽早展示页面内容，而不是等待 JavaScript 加载和执行后再渲染，从而提升了性能和 SEO。

### 十九、**Pinia 中的数据在哪里储存**

Pinia 是 Vue 3 的状态管理库，默认情况下，Pinia 的状态存储在内存中。这意味着，当页面刷新时，所有的状态数据会丢失。不过，Pinia 提供了插件机制，可以将状态持久化到浏览器的存储中，如 `localStorage` 或 `sessionStorage`。

使用 Pinia 插件时，可以选择将状态存储到本地存储中，确保即使页面刷新，状态也能保持。

```javascript
javascript 体验AI代码助手 代码解读复制代码import { createPinia } from 'pinia'
import piniaPluginPersistedstate from 'pinia-plugin-persistedstate'

const pinia = createPinia()
pinia.use(piniaPluginPersistedstate)
```

### 二十、**Vue Router 有几种模式，有什么区别**

Vue Router 提供了三种模式来管理路由和 URL 地址：

1. **`hash` 模式**：通过 URL 中的哈希值（`#`）来进行路由控制，兼容性最好，但会在 URL 中留下 `#` 字符。适用于所有浏览器。

   示例：

   ```bash
   bash
   
    体验AI代码助手
    代码解读
   复制代码https://example.com/#/home
   ```

2. **`history` 模式**：利用 HTML5 History API 实现路由跳转，URL 中没有哈希值，URL 更简洁、易读。需要服务器配置支持，以避免直接刷新页面时出现 404 错误。 `Vite 开发服务器默认支持 history 模式，所有路径请求都会自动回退到 index.html，不会出现 404。`

   示例：

   ```arduino
   arduino
   
    体验AI代码助手
    代码解读
   复制代码https://example.com/home
   ```

3. **`Abstract` 模式** (较少使用):

   - 用于非浏览器环境（如 Node.js、Electron、Native 应用）
   - 不依赖浏览器 API
   - 路由会保存在内存中，不会修改 URL

- 配置方式

  :

  ```javascript
  javascript 体验AI代码助手 代码解读复制代码const router = new VueRouter({
    mode: 'abstract',
    routes: [...]
  })
  ```

### 二十一、**状态管理库什么时候用到**

状态管理库（如 Vuex 或 Pinia）通常在以下场景中使用：

- 应用中有多个组件需要共享同一份状态，避免组件间的直接传递或通过事件传递的复杂性。
- 状态的变化较为复杂或频繁，单一组件无法处理状态变化时，采用状态管理库能使状态变得更为可控和可预测。
- 需要跨多个视图或页面共享状态（如用户登录状态、购物车数据等）。
   Vuex 和 Pinia 提供了一种集中式的方式来管理状态，使得在多个组件间共享状态更加清晰和高效。

### 二十二、**动态路由怎么实现**

动态路由允许在路由配置中使用参数来构建灵活的 URL，可以通过以下方式实现：

- **动态路由参数**：通过在路由路径中使用 `:param` 语法来定义动态路由，例如 `/user/:id`，`id` 就是动态参数，可以在路由视图中访问到。
- **懒加载**：可以通过动态导入来实现组件的懒加载，例如 `component: () => import('./views/User.vue')`，这样只有在路由被访问时，组件才会被加载，减少了初始加载的体积。
- **beforeEnter 钩子**：通过 `beforeEnter` 钩子可以在路由进入前做数据加载，确保数据准备好再渲染页面。

### 二十三、**Vue 3 发布订阅者模式**

Vue 3 的响应式系统基于发布/订阅模式。每当数据发生变化时，所有订阅该数据的组件或函数都会收到通知并更新。这是通过 `Proxy` 实现的，当数据变化时，Proxy 会触发依赖收集，并在数据变化时通知相关的视图更新，从而实现响应式。

### 二十四、**keep-alive 是什么？如何使用**

`keep-alive` 是 Vue 提供的一个内置组件，它用于缓存不活动的组件，避免它们重新渲染，从而提升性能。常用于多页面的场景，避免频繁的组件销毁和重建。

- **使用方式**：将组件包裹在 `keep-alive` 标签内：

  ```xml
  xml 体验AI代码助手 代码解读复制代码<keep-alive>
    <my-component />
  </keep-alive>
  ```

- **控制缓存**：通过 `include` 和 `exclude` 属性，可以控制哪些组件需要被缓存，例如：

  ```xml
  xml 体验AI代码助手 代码解读复制代码<keep-alive include="ComponentA, ComponentB">
    <my-component />
  </keep-alive>
  ```

  这将仅缓存 `ComponentA` 和 `ComponentB`。

### 二十五、**Vue 如何实现懒加载**

Vue 中的懒加载通常通过动态导入实现。例如，在 Vue Router 中可以通过 `import()` 动态导入组件，从而实现路由级别的懒加载：

```javascript
javascript 体验AI代码助手 代码解读复制代码const routes = [
  {
    path: '/about',
    component: () => import('./views/About.vue')
  }
]
```

这种方式只有在访问到该路由时才会加载相应的组件，能够减少初始加载的包体积，提升性能。

### 二十六、**Vue 打包需要做什么配置**

Vue 的打包通常使用 `webpack` 或 `Vite` 进行配置，常见的配置项包括：

- **代码分割**：使用 `import()` 动态导入组件，实现按需加载，减少初始加载体积。
- **资源优化**：配置图片压缩、字体优化等，减少资源文件大小。
- **环境变量**：配置不同的环境变量（如 `process.env.NODE_ENV`）来区分开发和生产环境，进行不同的优化和配置。
- **压缩和混淆**：在生产环境下进行代码压缩和混淆，减小最终打包的文件大小。

### 二十七、**Vue 的优点**

- **易于上手**：Vue 提供了清晰、易懂的文档，学习曲线平缓，适合快速入门。
- **响应式系统性能优秀**：Vue 的响应式系统基于 `Proxy`，性能高效，能够处理复杂的状态变化。
- **组件化结构**：Vue 提供了组件化开发方式，方便开发和维护大型应用。
- **灵活性高**：支持选项式和组合式 API，开发者可以根据需求选择适合的方式来组织代码。

### 二十八、**Vue 中的 nextTick**

`nextTick` 是 Vue 提供的一个 API，用于在 DOM 更新之后执行回调。由于 Vue 的响应式更新是异步的，`nextTick` 可以确保在数据变化之后，获取到更新后的 DOM 状态。例如，在更新数据之后想要访问 DOM 元素，可以使用 `nextTick`：

```javascript
javascript 体验AI代码助手 代码解读复制代码this.$nextTick(() => {
  // DOM 更新完成后执行
});
```

它常用于在 Vue 更新 DOM 后进行某些操作，比如获取新的 DOM 状态或执行动画等。

##### Vue 的响应式原理？

数据驱动视图的过程：1、数据变化 2、生成vdom节点，使用diff算法比较vdom 节点的变化 3、更新视图

Vue 的响应式原理是核心是通过 `Object.defindeProperty`完成的，他为所有的data的属性绑定 `get`和 `set`方法，当读取 data 中的数据时自动调用 get 方法，当修改 data 中的数据时，自动调用 set 方法，

检测到数据的变化，会通知观察者 Wacher，重新render 当前组件（子组件不会重新渲染）,生成新的虚拟 DOM 树，Vue 框架会遍历并对比新虚拟 DOM 树和旧虚拟 DOM 树中每个节点的差别，并记录下来，最后，加载操作，将所有记录的不同点，局部修改到真实 DOM 树上。

##### Vue 框架原理

`vue`是一个**MVVM** 渐进式框架，**MVVM**是vue的设计模式，在vue框架中数据会自动驱动视图。

MVVM 框架的核心是数据绑定，Vue.js 通过 vue.js 中的指令实现数据双向绑定。指令中的表达式与模型中的数据绑定，一旦模型数据发生变化，指令表达式自动更新，从而实现了视图自动更新。当用户修改视图中的元素时，也会自动将修改的数据更新到模型中。

M：Model（模型），代表应用程序中业务逻辑和数据保存、检索的部分，通常与后端数据交互。**在vue中指data中的数据。**

V：View（视图），是应用程序的用户界面，通常由 HTML、CSS、JavaScript 组成，负责在屏幕上展示数据。**在vue中指templement中的html代码片段**

VM：ViewModel（视图模型），连接视图和模型之间的桥梁。Vue 采用了双向数据绑定技术（data binding），使用 ViewModel 构建并管理 View，也就是在 ViewModel 中定义 View 属性和行为，从而将 View 的状态和行为抽象成 ViewModel，使得 View 可以通过 ViewModel 进行绑定和操作，对于 View 中的数据变化可以通知 ViewModel，反之亦然。这样 View 和 Model 就可以互相独立，开发者只需要处理和调整 ViewModel 即可。 **在vue中例如：在代码某个标签上绑定了某个click事件，并且触发后这个事件会对视图上的数据进行修改。**

##### vue双向数据绑定原理？

Vue 的双向数据绑定是通过**数据劫持**和**发布/订阅模式**相结合来实现的，可以实现数据的同步更新，同时避免了手动更新视图的操作。

- 数据变化更新视图
- 视图变化更新数据。

我们已经知道实现数据的双向绑定，首先要对数据进行劫持监听，所以我们需要设置一个**监听器Observer**，用来监听所有属性。如果属性发上变化了，就需要告诉**订阅者Watcher**看是否需要更新。（因为订阅者Watcher是有很多个，所以我们需要有一个**消息订阅器**Dep来专门收集这些订阅者，然后在监听器Observer和订阅者Watcher之间进行统一管理的。）

接着，我们还需要有一个指令解析器Compile，替换模板数据或者绑定相应的函数（对每个节点元素进行扫描和解析，将相关指令对应初始化成一个订阅者Watcher），此时当订阅者Watcher接收到相应属性的变化，就会执行对应的更新函数，从而更新视图。

因此接下去我们执行以下3个步骤，实现数据的双向绑定：

1.实现一个监听器Observer，用来劫持并监听所有属性，如果有变动的，就通知订阅者。

2.实现一个订阅者Watcher，可以收到属性的变化通知并执行相应的函数，从而更新视图。

3.实现一个解析器Compile，可以扫描和解析每个节点的相关指令，并根据初始化模板数据以及初始化相应的订阅器。

- 实现一个订阅器 Dep：订阅器采用 发布-订阅 设计模式，用来收集订阅者 Watcher，对监听器 Observer 和 订阅者 Watcher 进行统一管理。

##### vue事件绑定原理？

1. **DOM事件绑定**

当在模板中绑定 DOM 事件时，Vue 在内部使用 addEventListener 将事件处理函数注册到真实的DOM元素上。例如： v-on:click="handleClick" 绑定click事件。

Vue 通过浏览器提供的DOM API向元素添加EventListener监听事件，当事件触发时，执行绑定的回调函数。

2.**自定义事件**

Vue 中可以使用 *on*方法监听自定义事件，emit 方法触发自定义事件。

Vue 内部通过Vue实例维护一个事件中心，通过事件中心来实现自定义事件的监听和触发。

一个组件在创建时会同时创建一个 vm 实例，vm 实例中会包含 *on*方法和emit 方法，在组件中你就可以通过 this 来调用这两个方法。在父组件中，通过 *emit*触发自定义事件，同时子组件通过on 来监听事件。

综上可知，Vue的事件绑定原理就是使用DOM事件和自定义事件两种方式，通过调用浏览器原生的DOM API和Vue内部提供的事件系统，来实现事件的触发和监听。

##### vue模板编译的原理？

Vue.js 是基于模板的渲染机制来实现页面的渲染的。Vue.js 通过将模板编译成渲染函数的方式来进行模板的渲染。模板编译的过程主要包括以下几个步骤：

1. 解析模板字符串：Vue.js 将模板字符串解析成 AST（Abstract Syntax Tree）。
2. 静态优化：在编译过程中，Vue.js 会静态地分析整个模板，检测不需要更改的部分，将其优化成静态内容，这些内容不需要在每次重新渲染时都重新生成。
3. 代码生成：将 AST 转换成渲染函数。这个过程包括将每个 AST 节点转换成代码字符串并拼接成渲染函数。
4. 渲染函数：将生成的渲染函数执行后，会得到一个 VNode（Virtual DOM 节点）。

这些步骤将模板编译成渲染函数，用于生成 Virtual DOM，并更新到页面中，从而实现页面的渲染。在每次有数据变化时，Vue.js 会重新调用渲染函数，生成新的 VNode，通过对比新旧 VNode 差异，从而仅仅更新需要更新的部分，提高页面的渲染效率。

##### 描述组件渲染的过程？

Vue组件渲染的过程是将组件的模板渲染成真实的DOM节点并插入页面中的过程。它大致分为以下几个步骤：

1. 创建Vue实例：在使用Vue框架时，需要创建Vue实例。Vue实例是Vue应用的入口，它将负责管理各个组件之间的数据传递和状态管理。
2. 模板解析：在Vue组件中，使用HTML模板描述组件的结构和样式。当Vue实例被创建时，它会解析组件的模板，并将解析后的模板存储在内存中。Vue使用HTML解析器将模板解析为AST（抽象语法树）。
3. 模板编译：在Vue实例渲染组件时，Vue将AST编译为渲染函数，渲染函数是一个返回HTML字符串的JavaScript函数。
4. 组件渲染：当Vue实例需要渲染组件时，它会使用渲染函数将组件渲染成HTML字符串。
5. 真实DOM生成：将HTML字符串转换成真实的DOM节点。Vue使用虚拟DOM算法，将HTML字符串转化为真实的DOM节点，Vue会尽量复用之前生成的DOM节点，以提高性能。
6. 真实DOM插入：将真实的DOM节点插入到页面中。Vue将生成的真实DOM节点插入到指定的挂载点，完成组件渲染的过程。

需要注意的是，Vue会对组件进行一定的优化，包括条件渲染、属性绑定、事件处理等，以提高渲染效率和用户体验。

##### event-bus原理?

EventBus是消息传递的一种方式，基于一个消息中心，**订阅和发布消息**的模式，称为发布订阅者模式。

1. on('name', fn)订阅消息，name:订阅的消息名称， fn: 订阅的消息
2. emit('name', args)发布消息, name:发布的消息名称 ， args：发布的消息

```kotlin
kotlin 体验AI代码助手 代码解读复制代码class Bus {
  constructor () {
    this.callbacks = {}
  }
  $on(name,fn) {
    this.callbacks[name] = this.callbacks[name] || []
    this.callbacks[name].push(fn)
  }
  $emit(name,args) {
    if(this.callbacks[name]){
       //存在遍历所有callback
       this.callbacks[name].forEach(cb => cb(args))
    }
  }
}
```

创建一个全局事件总线: 挂载在Vue.prototype之上

##### VUEX的原理？

Vuex是通过**全局注入store对象**，来实现组件间的状态共享

##### vue.$set方法的原理？

①传入的target如果是`undefined`、`null`或是原始类型，则直接跑出错误。

②如果传入的是一个数组的话，就会调用数组的splice方法进行实现响应式。

③如果不是一个数组，就当做对象来处理，先判断当前key在源对象是否存在，如果存在，说明当前key已经是响应式的，就直接进行操作对应的动作。如果key不在源对象中，就调用Object.defineReactive方法将该key添加到源对象上，并且实现了响应式

##### Vue中scoped原理？

给HTML的DOM节点加一个不重复data属性(形如：data-v-2311c06a)来表示他的唯一性。

在每句css选择器的末尾（编译后的生成的css语句）加一个当前组件的data属性选择器的哈希特征值（如[data-v-2311c06a]）来私有化样式。

##### nextTick的原理：

nextTick的参数（callback），在promise.resolve.than异步的行为里面去执行

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e41eda08a00146bfbf127746d4815f6b~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

##### vue.mixin的原理？

使用原理： Vue.mixin的实现原理是基于Vue对组件选项的合并策略，在创建和合并组件选项时，会递归地将对象的所有属性合并到目标对象上，如果有相同名字的属性，则会进行策略性合并。当合并的对象包含生命周期事件、methods、data等选项时，这些选项会以先来后到的顺序合并到目标组件中。

在使用Vue.mixin时，我们可以通过定义一个Object，将通用的逻辑封装到这个对象中，然后通过Vue.mixin()方法全局注册这个对象，从而让所有组件可通过“继承”这个Object的方式，达到共享的目的。

需要注意的是，使用Vue.mixin并不能完全解决代码复用的问题，过度使用Vue.mixin可能会导致代码难以维护，潜在影响可读性、可维护性等。因此，应慎重使用Vue.mixin，并确保对其的合理使用。

##### computed实现原理

实现原理的核心在于依赖收集。当计算属性被创建时，Vue.js会通过 Object.defineProperty() 方法将其定义为getter函数，并且在getter函数中，调用其依赖的属性的getter函数。当依赖的属性发生改变时，它们会通知计算属性的setter函数，从而触发计算属性的更新。

除了依赖收集，Vue.js还提供了缓存机制，以避免非必要的计算耗时。当一个计算属性首次被访问时，它的值会被计算，并缓存下来。只有当依赖的属性发生改变时，计算属性才会重新计算，否则直接返回缓存的值。

##### diff算法原理（重点）

是vdom中最核心、最关键的部分。Diff算法是虚拟DOM(Virtual DOM)的核心算法，用于比较新旧虚拟DOM树，找出变化的部分，最终只更新变化的部分，以达到优化渲染性能的目的。 **宗旨**：只比较同一层级， 时间复杂度O(N)，深度优先 ![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/08ee971e58a7488d9247a8e708e28981~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?) **步骤**：

> **patch方法**：对比当前同层的虚拟节点是否为**同一种类型的标签**
>
> - 是：继续执行`patchVnode方法`进行更深比对
>
> - 否：没必要比对了，直接整个节点替换成`新虚拟节点`
>
>   ```kotlin
>   kotlin 体验AI代码助手 代码解读复制代码function sameVnode(oldVnode, newVnode) {
>     return (
>       oldVnode.key === newVnode.key && // key值是否一样
>       oldVnode.tagName === newVnode.tagName && // 标签名是否一样
>       oldVnode.isComment === newVnode.isComment && // 是否都为注释节点
>       isDef(oldVnode.data) === isDef(newVnode.data) && // 是否都定义了data
>       sameInputType(oldVnode, newVnode) // 当标签为input时，type必须是否相同
>     )
>   }
>   ```
>
> **patchVnode方法：**
>
> - 找到对应的`真实DOM`，称为`el`
> - 判断`newVnode`和`oldVnode`是否指向同一个对象，如果是，那么直接`return`
> - 如果他们都有文本节点并且不相等，那么将`el`的文本节点设置为`newVnode`的文本节点。
> - 如果`oldVnode`有子节点而`newVnode`没有，则删除`el`的子节点
> - 如果`oldVnode`没有子节点而`newVnode`有，则将`newVnode`的子节点真实化之后添加到`el`
> - 如果两者都有子节点，则执行`updateChildren`函数比较子节点，这一步很重要
>
> **updateChildren方法:** 新旧虚拟节点的**子节点对比**
>
> `首尾指针法`，新的子节点集合和旧的子节点集合
>
> 然后会进行互相进行比较，总共有五种比较情况： ![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b4ee647a14c640ccb29d2a97ba70ebf1~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)
>
> - 1、`oldS 和 newS`使用`sameVnode方法`进行比较，`sameVnode(oldS, newS)`
> - 2、`oldS 和 newE`使用`sameVnode方法`进行比较，`sameVnode(oldS, newE)`
> - 3、`oldE 和 newS`使用`sameVnode方法`进行比较，`sameVnode(oldE, newS)`
> - 4、`oldE 和 newE`使用`sameVnode方法`进行比较，`sameVnode(oldE, newE)`
> - 5、如果以上逻辑都匹配不到，再把所有旧子节点的 `key` 做一个映射到旧节点下标的 `key -> index` 表，然后用新 `vnode` 的 `key` 去找出在旧节点中可以复用的位置。

日常应用场景：循环中key的使用

## **VUE2**

### 基础：

##### 为什么Vue中的v-if和v-for不建议一起用?

永远不要把 v-if 和 v-for 同时用在同一个元素上。

当 v-if 与 v-for 一起使用时，v-for 具有比 v-if 更高的优先级（vue2），这意味着 v-if 将分别重复运行于每个 v-for 循环中,一起使用会造成性能浪费

- **解决方案有两种，把v-if放在v-for的外层或者把需要v-for的属性先从计算属性中过滤一次**

##### vue图片懒加载？

$ npm i vue-lazyload -D

1. main.js 在入口文件** **import** **VueLazyload** **from** 'vue-lazyload' //引入这个懒加载插件
2. 在入口文件添加后，在组件任何地方都可以直接使用把 img 里的:src -> v-lazy**

##### v-show和v-if的区别

v-show：通过CSS的display:none控制显示和隐藏（频繁切换时候）

v-if：组件进行真正的渲染和销毁，删除DOM节点。

**补充问：为什么v-show用的不是visibility:hidden 隐藏元素?** 因为`visibility: hidden`可以隐藏元素**但保留其空间**，而在某些情况下，这并不是所需的效果。

在页面中不占有位置和删除DOM节点是有很大的区别的。

如果一个元素被删除了，那么它所占据的位置也会随之消失，可以让后面的元素顶上来，相当于从DOM树中移除了该元素。而如果一个元素不占有位置，可能是因为它被隐藏了，但它仍然存在于DOM树中，占据了一定的空间。这意味着即使被隐藏的元素不可见，但是它依然可以影响到相邻元素的排布，也可能会影响到整个页面的布局。

因此，删除DOM元素会对页面布局产生影响，而隐藏元素不会对布局产生影响。在实际的应用场景中，删除DOM元素通常用于释放一些无用的内存空间，而隐藏元素则用于在必要的时候暂时隐藏某个元素，等到需要重新显示的时候再将其显示出来。

##### 为何v-for中要使用key

1. 必须使用key，不能是index和random，因为**diff算法**中通过**tag和key来判断是否为相同节点。** 匹配了key，则只移动元素，性能较好，未匹配key，则删除重建，性能较差。
2. 这样可以减少渲染次数，提升渲染性能

##### scoped导致的问题？

父组件无`scoped`属性，子组件带有`scoped`，父组件是无法操作子组件的样式的（原因在原理中可知），虽然我们可以在全局中通过该类标签的标签选择器设置样式，但会影响到其他组件

父组件有`scoped`属性，子组件无，父组件也无法设置子组件样式，因为父组件的所有标签都会带有data-v-469af010唯一标志，但子组件不会带有这个唯一标志属性，与1同理，虽然我们可以在全局中通过该类标签的标签选择器设置样式，但会影响到其他组件

父子组建都有，同理也无法设置样式，更改起来增加代码量

##### 循环问：vue什么时候操作DOM比较合适

mounted和updated都不能保证子组件全部挂载完成，所有使用$nextTick的时机进行操作DOM。

##### nextTick的使用和原理？

功能：可以获取到更新后的DOM，nextTick返回一个Promise，是一个 异步行为。

因为vue采用的是异步更新策略，数据发生变化，DOM节点并不会立刻发生变化，而是开启一个队列，把组件更新函数保存在队列中，同一个事件循环中发生的所有数据变更会异步的批量更新。这一策略导致我们对数据的修改不能立刻的体现在DOM上，此时如果我们想获取更新后的DOM状态，就要使用nextTick。在开发时，一般两个场景用：

1、created中想要获取DOM时；

2、响应式数据变化后获取DOM更新后的状态

##### Vue组件如何通讯

- 父子组件：props和this.$emit
- 
- refs：在通过this.refs：在 通过this.refs：在通过this.refs.属性名称 调用子组件事件，
- parent通过this,parent 通过this,parent通过this,parent.方法名() 调用父组件事件
- 自定义事件（总线通讯）：开发者自己定义的事件，与浏览器自带的事件（如click、load、scroll等）不同。通常用于在应用程序**不同部分之间的通信**。

在JavaScript中，可以使用Event来创建自定义事件。用event.emit/event.on/event.off分别进行事件触发、监听和清除。

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c92bad4910c54f0fa4aaf75522f20e41~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/02862245ce2d4f6cb045ed274e10ae91~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

- vuex：可全局组件通讯
- $attrs：透传 attribute指的是传递给一个组件，却没有被该组件声明为 props或 emits的 attribute 或者 `v-on` 事件监听器。可以实现上下级组件的透传，但是依赖于v-bind = "attrs"进行一层一层的传递。
- provide/inject：用于上下级组件的透传。下面是响应式数据

> 父组件： ![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/04474bce466e497d89c0fb050981b405~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?) 子组件： ![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/94ab61c74fb64e5a9015d1622bfb35e6~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

##### 描述组件渲染和更新的过程

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/aeec28bdcf7a47df87a7aeb5a474e40e~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?) 给vue中所有data对象的每个属性分别定义了getter和setter，然后通过getter收集依赖，依赖是指每个数据属性的对应关系。使用setter对数据的赋值和修改进行拦截，然后vue就知道了数据的变化，然后就只会通知vue中的watcher，进行数据和页面的更改。

##### Vuex中action和mutation有什么区别？

- mutation是原子操作，必须同步代码
- action可包含多个mutation；可包含 异步代码

##### computed有什么特点，与watch有什么区别？

computed有缓存，data不变化时，不重新计算。可以提高性能。而method中的方法没有缓存

**区别**

> computed用于计算**产生新的数据**
>
> watch用于**监听**现有数据

##### 为什么组件data必须是函数？

因为定义的Vue文件是一个类，class文件，每个地方使用这个资源 的时候，相当于是对类的实例化。如果data不是函数，则每个组件的实例数据都会共享。

##### Ajax请求应该放在哪个生命周期？

mounted中，因为JS是单线程的，Ajax异步获取数据。只要JS没有渲染完，异步数据得到了也是在异步的排队过程中，不会有什么效果。

##### 如何将组建所有的props传递给子组件？

＄props ＜子组件 v－bind=“$props”／＞

##### 多个组件有相同逻辑，如何抽离？

mixin

##### vue.mixin的使用场景

使用场景： Vue.mixin是一种混入（mix-in）技术，可以在多个组件之间共享组件选项，包括生命周期方法、data选项等等，使用Vue.mixin可以将一些通用的功能或逻辑封装起来，让多个组件可共同调用这些相同的逻辑，避免了冗余代码的出现。

在使用Vue.mixin时需要注意以下几点：

1. 全局注册的Mixin会影响到所有Vue组件，包括第三方组件，因此需要确保Mixin的逻辑可适用于所有组件，不会产生意想不到的副作用。
2. Mixin可能会覆盖组件本身的选项，因此需要仔细考虑Mixin的选项名称和组件本身选项名称的冲突。如果两者有重复，则Mixin的选项会覆盖组件本来的选项。一般来说，建议Mixin的选项命名要以特殊前缀开头以避免冲突。
3. Mixin中的函数和数据，都是共享的，因此需要注意变量名和数据结构的一致性，避免出现数据混乱的情况。同时也需要注意数据结构的引用和拷贝，如果Mixin中的数据是对象或数组，需要避免直接修改这些数据的引用，推荐使用Object.freeze()或类似方法锁定数据对象。
4. 注意在组件中使用parent或root来访问父组件或根组件的数据，因为Mixin在多个组件中都有使用，这些数据实际上是共享的，可能会导致数据不一致性。
5. 不要在Mixin中使用箭头函数，因为在Mixin中this指向的是Vue实例，而箭头函数中的this指向的是全局对象。

总之，在使用Vue.mixin时需要确保Mixin本身的合理性，合理考虑Mixin与组件本身选项的交互和数据传递，以避免意外情况的出现。同时要保持代码的可维护性和可读性，减少不必要的代码迁移和重构。

##### 何时使用keep-alive?

①缓存组件，不用重复渲染的时候。 如，前端多个静态tab页面切换。可以优化性能

##### 何时需要使用beforeDestory?

①解绑自定义事件 ②清除定时器 ③解绑自定义DOM事件

##### 什么是作用域插槽？

带数据的插槽，即让插槽内容能够访问子组件中才有的数据。

##### v-model自定义实现

我们可以通过分别绑定value属性和input事件来实现自定义的v-model功能：

```html
html 体验AI代码助手 代码解读复制代码<template>
  <div>
    <counter :value="count" @input="count = $event"></counter>
    <p>Count: {{ count }}</p>
  </div>
</template>

<script>
import Counter from './Counter';

export default {
  components: {
    Counter
  },
  data() {
    return {
      count: 0
    }
  }
}
</script>
```

在上述代码中，我们使用自定义计数器组件，并将count属性绑定到计数器组件的value属性上，通过@input事件监听计数器组件的值的变化，并将变化的值赋给父组件的count属性，从而实现了自定义v-model的功能。 不用:value和@input两个事件名字时:

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/20bb15466f5f4c2bb4277e8f58af3bec~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?) vue3: ![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/65691fefa9d048eebd1e785632ffcb9b~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

##### 单页应用和多页应用的优缺点？

单页应用和多页应用是两种常见的web应用程序的架构设计方案。它们各自有着自己的优缺点，适用于不同的场景。

单页应用（Single Page Application，SPA）：

优点：

- 用户体验好，局部刷新，不需要页面刷新，加载速度快，提升用户满意度。
- 可以提供类似原生应用的用户体验，例如前后端分离实现的页面无刷新化跳转，多级下拉菜单等。
- 前后端分离，API和UI开发分离，可以提高开发效率，工作分工更清晰。

缺点：

- **首次加载时间长**，因为要加载JavaScript/CSS文件等资源，可能会影响SEO优化。
- 复杂性高，需要前端框架进行实现，不利于初学者上手，调试以及维护也相应复杂。
- 安全性要求高，容易受到跨站点脚本攻击，需要更高的安全措施。

多页应用（Multiple Page Application，MPA）：

优点：

- 早期开发方式，对SEO优化友好，每个页面都有自己的URL地址。
- 容易上手，不需要掌握很多特殊技能，语言不限。
- 页面之间互相独立，不容易出现问题，容错率高。

缺点：

- 用户体验比较差，每次页面切换都需要重新加载页面，用户体验不佳。
- 开发效率低，每个页面都需要独立地开发和维护，工作量大。
- 需要后端参与渲染页面，前后端耦合程度高。

综上所述，单页应用适用于对用户体验有较高要求的场景，例如实时应用、互动游戏等。而多页应用适合于需要SEO的场景，如电商网站、新闻资讯等。

##### vuex为什么要分模块并加命名空间？

Vuex 分模块并加命名空间的主要目的是为了更好地管理应用的状态。在一个大型的 Vuex 应用中，如果所有的状态都集中在一个 store 中，会导致代码难以维护和扩展，因此需要将状态进行模块化管理，以优化应用的设计结构，并更好地组织和管理组件的状态。

同时，Vuex 命名空间的设计，则是为了解决模块之间命名冲突的问题。在一个大型的 Vuex 应用中，可能会存在多个模块都定义了同名的 state、mutation、action、getter，这时如果不加命名空间，容易出现命名冲突的问题，导致出现不可预期的错误。因此，Vuex 引入了命名空间的概念，可以保证每个模块具有独立的命名空间，避免了命名冲突，方便进行模块之间的数据通信和状态管理。

总之，Vuex 分模块并加命名空间可以让我们更好地管理和维护应用状态，并且可以更好地定义和隔离模块之间的状态和行为，使得应用的代码更易于组织和扩展。

##### vue SSR？

Vue SSR（Server-Side Rendering 服务端渲染）是一种将 Vue 应用程序渲染为 HTML 的技术，其核心思想是在服务器端渲染 Vue 应用程序组件并将其输出为 HTML，然后将 HTML 发送回浏览器进行渲染，从而提高 Vue 应用程序的性能和SEO表现。

Vue SSR 主要解决了两个问题：

1. 首屏渲染：当用户第一次访问应用时，由于需要下载并执行 JS 文件，Vue SPA（Single Page Application 单页应用）通常存在白屏时间，给用户带来不良体验。而使用 SSR 技术在服务器端就可以渲染组件并生成相应的 HTML，最终将完整的 HTML 响应给浏览器，提高首屏渲染效率。
2. SEO优化：在单页应用中，搜索引擎爬虫不能够直接读取到页面内容，这对于SEO（Search Engine Optimization 搜索引擎优化）十分不利。而使用 SSR 技术可以生成静态 HTML，方便搜索引擎抓取并提升 SEO 的效率。

Vue SSR 的原理是将 Vue 组件在服务器端渲染成 HTML，并通过浏览器请求这些静态 HTML 文件，**跳过了客户端渲染的过程**， 从而使用户能够更快地看到应用程序内容。具体实现可以分为以下几个步骤：

1. 获取请求的路由信息。
2. 根据路由信息创建一个 Vue 实例。
3. 调用 Vue 实例的 renderToString 方法，将组件渲染成 HTML 字符串。
4. 将渲染出来的 HTML 字符串返回给浏览器。
5. 浏览器解析 HTML、加载 JS 文件再进行客户端渲染。

需要注意的是，由于 SSR 需要在服务器上构建完整的应用程序，因此需要在服务器端运行一个 Node.js 框架来处理请求和构建 Vue 实例，并通过 Webpack 可以使 SSR 应用开发和部署更加方便。同时，很多浏览器 API 只能在浏览器环境中运行（例如：window 和 document 等对象），在服务器端无法使用，因此需要考虑这些因素，对组件的代码进行适当的修改和处理。

##### vue修饰符有哪些？

Vue修饰符是 Vue 提供的一种特殊语法，用于在指令后面添加一些特殊前缀，来表达指令的特殊含义。Vue 修饰符主要被用于指令的模板用法中，可以用来改变指令的行为方式，包括以下几个：

1. `.prevent`: 阻止默认事件的发生。
2. `.stop`: 阻止事件冒泡，即不再向上层元素传播事件。
3. `.capture`: 添加事件监听器时使用事件捕获模式。
4. `.self`: 只触发当前元素上的事件，不会触发它的子元素。
5. `.once`: 只触发一次事件，即使绑定了多个相同的事件监听器。
6. `.passive`: 指示浏览器不需要在执行操作时通知 Vue 是否阻止了事件的默认行为，可以提高页面的滚动性能。

除了上述常见的修饰符，Vue 还提供了一些用于键盘事件的快捷键修饰符，例如：

1. `.keyCode`: 只在特定的按键按下时才触发事件。
2. `.enter`: 按下回车键时触发事件。
3. `.tab`: 按下 Tab 键时触发事件。
4. `.delete`: 按下删除键时触发事件。

还有一些其他的键盘快捷键修饰符，通过这些修饰符可以方便地绑定和处理各种键盘事件。

##### 前端路由的实现原理

通过**监听URL的变化**，然后根据不同的URL路径显示不同的页面内容，而不是通过向服务器请求不同的HTML页面来实现页面切换。

hash 模式：

> 在URL中添加一个`#`符号，后面跟上对应的路径，例如`http://example.com/#/ path/to/page`。浏览器不会重新加载页面，而是通过监听`hashchange`事件来切换页面内容。可以使用`window.location.hash`来获取当前的URL路径，也可以通过设置`window.location.hash`来改变URL路径。可以对浏览历史记录栈进行修改，以及popState事件的监听到状态的更改。

history模式：

> 通过HTML5提供的`history` API来实现路由切换。可以使用`pushState`方法或`replaceState`方法来修改URL路径，然后通过监听`popstate`事件来响应URL的变化。例如`history.pushState({}, '', '/path/to/page')`。需要注意的是，使用History模式需要在服务器端进行配置，将所有的URL请求都重定向到主页面，避免在刷新或直接访问URL时出现404错误。

请用vnode描述一个DOM结构：

![image-20230402124929283]()![image-20230402124940467]()

##### 监听data变化的核心API是什么？

object.defineProperty

缺点：无法监听数组变化，无法监听对象的添加删除，深度监听，需要递归到底，一次性计算量大

##### Vue为何是异步渲染，$nextTick有何用？

Vue是异步渲染的原因主要是为了提高性能和用户体验。当组件中的数据发生变化时，Vue会根据新的数据重新生成虚拟DOM，并将新旧虚拟DOM进行比较，找出差异部分进行更新。由于比较虚拟DOM是一项计算密集型的操作，如果每次数据变化都同步更新虚拟DOM和页面，会导致页面性能下降，用户体验变差，尤其是在大规模数据变化时，会造成页面卡顿或甚至崩溃。

在Vue中，异步渲染的机制主要是通过`nextTick`方法来实现的，`nextTick`方法会将回调函数推迟到下一个事件循环中执行，从而实现异步渲染的效果。微任务

##### route和route和route和router的区别？

在Vue.js中，route和route和route和router都是与路由相关的对象，但是它们扮演的角色不同，有以下区别：

1. $route对象是当前路由跳转的对象，它包含了当前路由的信息，例如当前路由路径、参数、查询参数等。
2. router对象是VueRouter的路由实例，它中定义了路由的配置、路由跳转方法等。可以通过router对象是Vue Router的路由实例，它中定义了路由的配置、路由跳转方法等。可以通过router对象是VueRouter的路由实例，它中定义了路由的配置、路由跳转方法等。可以通过router对象监听路由变化或进行编程式导航。

简单来说，route主要是为了获取当前路由的信息，route主要是为了获取当前路由的信息，route主要是为了获取当前路由的信息，router主要为了管理路由，跳转到不同的路由，或者对路由做一些其它的操作。

##### Vue-router有几种模式？

①history： history 模式使用 HTML5 的 History API 来实现路由管理，该模式会将路由信息放在浏览器的历史记录中，即 URL 中的路径部分不包含 `#` 符号，而是正常的 URL 格式 `/path/to/route`。在这种模式下，当用户访问不同的路由时，URL 发生的变化会触发浏览器整页刷新，但可以通过设置服务器端配置来避免刷新问题。

②hash： 默认情况下，Vue-router 使用的是 hash 模式，在 URL 中使用 `#` 符号作为路由的标识，即 URL 中的路径部分是 `/#/path/to/route` 的形式。在这种模式下，当用户访问不同的路由时，URL 发生的变化都只会影响 `#` 符号后面的部分，而不会触发浏览器的整页刷新，因此可以实现单页应用（SPA）。

③abstract： abstract 模式不依赖于浏览器的历史记录或者 URL，而是将路由信息都保存在 JavaScript 对象中。这种模式主要用于测试或者非浏览器环境下的应用程序，例如 Electron 等。

##### Vue中如何扩展一个组件？

1、内容扩展:slot

2、逻辑扩展：mixins，extends，composition api

##### 子组件可以修改父组件中的数据吗？

子组件确实可以修改父组件中的数据，但是并不推荐这样做，官方开发文档中有一个**单项数据流**的原则

##### Vue中权限管理怎么做？

一般分为：按钮权限和页面权限；也分为前端配置和后端配置：

**页面权限**

前端配置：在Vue中，可以使用路由拦截器实现权限控制。通过**路由拦截器**，可以在页面跳转之前判断用户是否具有访问权限。 例如：

```javascript
javascript 体验AI代码助手 代码解读复制代码import Vue from 'vue'
import VueRouter from 'vue-router'
import store from '@/store'

Vue.use(VueRouter)

const router = new VueRouter({
  mode: 'history',
  routes: [
    {
      path: '/',
      name: 'home',
      component: () => import('@/views/Home.vue'),
      meta: {
        requiresAuth: true, // 需要登录才能访问
        requiresAdmin: true // 需要管理员权限才能访问
      }
    },
    {
      path: '/login',
      name: 'login',
      component: () => import('@/views/Login.vue')
    },
    {
      path: '/about',
      name: 'about',
      component: () => import('@/views/About.vue'),
      meta: {
        requiresAuth: true // 需要登录才能访问
      }
    }
  ]
})

router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // 判断是否需要认证
    if (!store.getters.isAuthenticated) {
      // 如果用户未登录，跳转至登录页面
      next('/login')
    } else {
      // 判断是否需要管理员权限
      if (to.matched.some(record => record.meta.requiresAdmin)) {
        if (store.getters.isAdmin) {
          next()
        } else {
          // 如果用户不是管理员，跳转至无权限页面
          next('/no-permission')
        }
      } else {
        next()
      }
    }
  } else {
    next()
  }
})

export default router
```

后端配置：会把所有页面路由信息存在数据库中，用户登录时，根据其角色查询可以得到他能访问的所有页面路由信息，然后返回给前端，前端通过addRoutes动态添加路由信息。 ![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/bcfcc85a09584cba83532503b58197df~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

**按钮权限**

通常会实现一个组件指令。如v-permission，将按钮要求角色通过传给v-permission指令，在指令的moutend钩子中可以判断当前用户角色和按钮是否存在交集，有则保留按钮，无则移除。

### 生命周期与钩子函数：

##### 描述vue组件生命周期（有父子组件的情况呢），每个生命周期都做了什么？

1、创建：beforecreate created beforemount mounted

2、更新：beforeupdate updated

3、销毁：beforedestroyed destroyed

父子组件：挂载：由父------->子；渲染：由子------->夫

##### vue父子组件生命周期器钩子函数顺序问题

1. 父组件 beforeCreate 钩子函数
2. 父组件 created 钩子函数
3. 父组件 beforeMount 钩子函数
4. 子组件 beforeCreate 钩子函数
5. 子组件 created 钩子函数
6. 子组件 beforeMount 钩子函数
7. 子组件 mounted 钩子函数
8. 父组件 mounted 钩子函数

**做了什么：**

beforecreate：创建一个空白vue实例，data和method尚未被初始化，不可使用

created：Vue实例初始化完成，完成响应式绑定data和method被初始化，未开始渲染模板

beforemount：编译模板，调用render生成vdom，未开始渲染DOM

mounted：完成渲染DOM，组件渲染完成，由“创建阶段”进入“运行阶段”

beforeupdate：data发生变化之后，但是尚未更新DOM

updated：data发生变化，并且完成DOM更新（不能在这一步修改data，否则会导致死循环）

beforedestroyed：组件进入销毁阶段，但是尚未进行销毁，任然可使用，可以用来接触一些自定义的事件，定时器自定义DOM节点等

destroyed：组件被销毁，所有子组件都被销毁

##### vue-router 的几种钩子函数？

在 Vue.js 中，Vue-Router 是官方推荐的路由管理器，主要用于通过 URL 地址来匹配对应的组件并将其渲染到页面中。Vue-Router 提供了一些生命周期钩子函数，可以在特定的时刻执行一些操作，常用的钩子函数有以下几种：

1. beforeRouteEnter: 这个钩子函数在路由进入前被调用，但这个时候该组件实例还没有被创建，所以无法直接访问组件实例，因此只能通过一个回调函数来访问到组件实例。
2. beforeRouteUpdate: 这个钩子函数在路由更新前被调用，因为路由更新可能会引起一些组件内部状态的变化，因此这个钩子函数可以用来处理这些状态变化。
3. beforeRouteLeave: 这个钩子函数在路由离开前被调用，主要作用是在组件离开前询问用户是否要保存未保存的数据，或者是在离开前做一些清理操作。
4. afterEach: 这个钩子函数在路由进入后被调用，可以用来添加页面访问统计、错误处理等公共逻辑。
5. beforeResolve: 这个钩子函数会在路由解析执行前被调用，可以在此修改或筛选后续要执行的路由。

这些钩子函数可以方便地对路由进行处理，帮助我们更好地管理页面的状态。

### 优化：

##### vue常见的性能优化

①合理使用v-show和v-if

②v-for加key，以及避免和v-if同时使用

③自定义事件、DOM事件及时销毁

④合理使用异步组件

⑤合理使用Keep-live

⑥前端通用优化，图片懒加载

##### 如何优化SPA首屏加载速度慢的问题？

优化SPA首屏加载速度慢的问题可以从以下几个方面入手：

1. 代码压缩与打包：使用Webpack等构建工具进行代码压缩，合并和打包，减少代码体积，提高加载速度。
2. 图片优化：使用合适的图片格式，采用lazy load和懒加载等方式，减少页面图片加载带来的时间成本。
3. 异步加载组件和数据：采用异步组件和Data Fetching，可以延迟一些用户不紧急需要的组件和数据的加载时间，从而加快首屏加载速度。
4. 缓存页面和数据：在浏览器端实现缓存机制，减少重复请求和加载，提升页面渲染速度。
5. 启用Gzip压缩：将服务器上的静态资源采用Gzip压缩后再进行传输，减少网络带宽占用，提升加载速度。
6. 代码懒加载：使用codesplitting的技术，根据需要进行懒加载，减少页面加载时的资源开销。
7. 采用服务器渲染：对于复杂的SPA应用，为了提高首屏渲染速度，可以采用服务器渲染的方式，通过提前将页面渲染出来返回给浏览器，减少浏览器端的渲染时间和开销。

通过以上方式的综合操作，可以极大地提升SPA应用的首屏加载速度。

##### Vdom真的很快吗？

vdom并不快，JS直接操作DOM才是最快的，但是“数据驱动视图”要有合适的技术方案，不能全部DOM重建，vdom就是目前最合适的技术方案。

##### Vue如何实现动态组件和异步组件？

动态组件：

`动态组件`：是Vue中一个特殊的Html元素：`<component>`，它拥有一个特殊的 `is` 属性，属性值可以是 `已注册组件的名称` 或 `一个组件的选项对象`，它是用于不同组件之间进行**动态切换**的。

使用标签和is属性可以实现动态组件，可以在同一位置动态切换不同的组件。具体使用方法如下：

```js
js

 体验AI代码助手
 代码解读
复制代码<component :is="componentName"></component>
```

其中，componentName是一个变量，它的值可以是一个组件的名称，例如：

data() { return { componentName: 'ComponentA' } }

这样，就会渲染ComponentA组件。

异步组件：

`异步组件`：简单来说是一个概念，一个可以让组件异步加载的方式；它一般会用于性能优化，比如减小首屏加载时间、加载资源大小。

```javascript
javascript 体验AI代码助手 代码解读复制代码    new Vue({
    // ...
        components: {
            'my-component': () => import('./my-async-component')
        }
    })

```

##### 何时使用异步组件？

①加载大组件 ②路由异步加载

##### vue中 动态组件和keep-alive的区别?

相同：Vue中动态组件和`<keep-alive>`都是用来优化组件性能的。

动态组件可以根据不同**的条件以不同的组件形式来渲染同一个组件位置**，它使得多个组件之间实现了灵活的切换。动态组件的优点是灵活，缺点是在不同组件之间频繁切换时，会导致组件的销毁和重建，使得页面状态丢失。动态组件不会默认缓存已经被销毁的组件。

而`<keep-alive>`的作用是缓存已经渲染的组件，来避免重复渲染已经存在的组件，优化性能。通过使用`<keep-alive>`可以使组件在切换时保持状态，不会因为被销毁而导致页面状态丢失。

`<keep-alive>`有两个重要的属性：

- `include`：被缓存的组件名称列表，只有该列表中的组件会被缓存。
- `exclude`：不被缓存的组件名称列表，该列表中的组件不会被缓存。

使用`<keep-alive>`缓存组件时，需要注意，被缓存的组件在缓存期间是不会被销毁的，因此如果组件占用的资源比较多，缓存过多的组件会导致内存占用过高，从而影响网页的性能。因此在使用`<keep-alive>`时，需要合理地配置`include`和`exclude`属性，以减小缓存的开销。

##### 如何减少DOM操作？

1. 减少读取DOM元素的次数：每次访问DOM都会产生一定的代价，尽量将元素的引用缓存起来，避免重复访问相同的DOM元素。
2. 批处理DOM元素操作：尽量将多个DOM操作合并成一次操作。例如，当需要修改一个列表的多个元素时，可以先将它们缓存起来，待操作完成后再进行一次性修改。
3. 使用事件委托：通过将事件绑定在父元素上，利用事件冒泡机制让父元素处理子元素的事件，可以减少事件绑定次数，提高页面性能。
4. 使用文档碎片：将需要操作的多个DOM节点先添加到文档碎片中，完成所有的操作后再将文档碎片一次性添加到页面中，可以减少对页面的多次渲染。
5. 避免频繁的DOM操作：频繁的DOM操作会导致页面不断地渲染和重排，影响页面性能和用户体验，尽量将DOM操作封装在一起，在完成一定的操作后再进行渲染。

##### 一次性给你大量的dom怎么优化？

1. 懒加载：只在需要时才加载DOM元素，而不是一次性将它们全部加载出来。
2. 分批加载：将大量DOM元素按照一定的规则进行分组，逐个进行加载，避免一次性加载所有DOM元素造成堵塞现象。
3. 虚拟滚动：只渲染屏幕内可见的DOM元素，滚动时实时更新渲染，避免一次性渲染大量DOM元素。
4. 使用列表组件：对于重复的、相似的DOM元素，可以使用列表组件如Vue的v-for来进行渲染，避免大量手工操作DOM元素。

##### vue如何兼容IE？

Vue.js 在默认情况下不支持 IE8 及其以下版本的浏览器，但是它可以通过一些方法来进行兼容：

1. 在 HTML 文件的头部添加 meta 标签：

```ini
ini

 体验AI代码助手
 代码解读
复制代码<meta http-equiv="X-UA-Compatible" content="IE=edge">
```

这样可以让 IE 浏览器使用最新的渲染模式来渲染页面。

1. 启用 babel-polyfill：

在 Vue 项目中安装 babel-polyfill，它可以在代码中自动加入一些特性的 shim 和 polyfills，从而支持更广泛的浏览器。例如：

```bash
bash

 体验AI代码助手
 代码解读
复制代码npm install babel-polyfill --save-dev
```

在 `src/main.js` 文件中引入：

```javascript
javascript

 体验AI代码助手
 代码解读
复制代码import 'babel-polyfill';
```

1. 使用 es5 版本的 Vue：

Vue 2.0 以上版本默认使用了 ES2015+ 的语法，如果要在 IE8 中运行，则需要使用 es5 版本的 Vue，可以通过在业务入口处，比如 main.js 中手动安装并指定 es5 版本的 Vue：

```javascript
javascript

 体验AI代码助手
 代码解读
复制代码import Vue from 'vue/dist/vue.esm.js';
```

1. 使用 Vue-cli 中的配置：

如果是使用 Vue-cli 工具创建的项目，可以通过修改 `.babelrc` 文件来配置，例如：

```json
json 体验AI代码助手 代码解读复制代码{
  "presets": [
    ["env", {
      "targets": {
        "browsers": ["last 2 versions", "ie >= 8"]
      }
    }]
  ]
}
```

这个配置指定编译目标浏览器为 "last 2 versions" 和 "IE 8 及以上"，可以实现对 IE8 的兼容。

总的来说，针对 IE 的兼容问题，可以结合不同的方法进行处理，具体方法可以根据项目需求来选择。



## VUE3

##### vue3比vue2有什么优势？

性能更好，打包体积更小，更好的ts支持，更好的代码组织，更好的逻辑抽离，更多的新功能

##### 描述Vu3生命周期

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/be25db0ffeae4655ae044d2dcf10e7f7~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

Options API的生命周期：

1. `beforeCreate`: 在实例初始化之后、数据观测(initState)和 event/watcher 事件配置之前被调用。 对于此时做的事情，如注册组件使用到的store或者service等单例的全局物件。 相比Vue2没有变化。
2. `created`: 一个新的 Vue 实例被创建后（包括组件实例），立即调用此函数。 在这里做一下初始的数据处理、异步请求等操作，当组件完成创建时就能展示这些数据。 相比Vue2没有变化。
3. `beforeMount`: 在挂载之前调用，相关的render函数首次被调用,在这里可以访问根节点，在执行mounted钩子前，dom渲染成功，相对Vue2改动不明显。
4. `onMounted`: 在挂载后调用，也就是所有相关的DOM都已入图，有了相关的DOM环境，可以在这里执行节点的DOM操作。在这之前执行beforeUpdate。
5. `beforeUpdate`: 在数据更新时同时在虚拟DOM重新渲染和打补丁之前调用。我们可以在这里访问先前的状态和dom，如果我们想要在更新之前保存状态的快照，这个钩子非常有用。相比Vue2改动不明显。
6. `onUpdated`:在数据更新完毕后，虚拟DOM重新渲染和打补丁也完成了，DOM已经更新完毕。这个钩子函数调用时，组件DOM已经被更新，可以执行操作，触发组件动画等操作
7. `beforeUnmount`:在卸载组件之前调用。在这里执行清除操作，如清除定时器、解绑全局事件等。
8. `onUnmounted`:在卸载组件之后调用，调用时，组件的DOM结构已经被拆卸，可以释放组件用过的资源等操作。

- `onActivated` – 被 `keep-alive` 缓存的组件激活时调用。
- `onDeactivated` – 被 `keep-alive` 缓存的组件停用时调用。
- `onErrorCaptured` – 当捕获一个来自子孙组件的错误时被调用。此钩子会收到三个参数：错误对象、发生错误的组件实例以及一个包含错误来源信息的字符串。此钩子可以返回 `false` 以阻止该错误继续向上传播。

Composition API的生命周期：

除了`beforecate`和`created`(它们被`setup`方法本身所取代)，我们可以在`setup`方法中访问的上面后面9个生命钩子选项:

##### 如何看待Composition API 和 Options API?

Composition API和Options API是Vue.js中的两种组件编写方式。

Options API是Vue.js早期版本中使用的编写方式，通过定义一个options对象进行组件的配置，包括props、data、methods、computed、watch等选项。这种方式的优点在于结构清晰、易于理解，在小型项目中比较实用。

Composition API是Vue.js 3.x版本中新引入的一种组件编写方式，它以函数的形式组织我们的代码，允许我们将相关部分组合起来，提高了代码的可维护性和重用性。Composition API还提供了模块化、类型推断等功能，可以更好地实现面向对象编程的思想。

Composition API 更好的代码组织，更好的逻辑服用；可维护性，更好的类型推导，可拓展性更好；

两种API各有优缺点，使用哪种API取决于具体的项目需求。对于小型项目，Options API更为简单方便；对于大型项目，Composition API可以更好地组织代码。

总之，Vue.js的Composition API和Options API是为了满足不同开发者的需求而存在的，我们应该根据具体的场景选择使用哪种API，以达到更好的开发效果和代码质量。

![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fee4ac484fe1401dad04dd61f4f52708~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

##### Vue3.0有什么更新

1. 性能优化：Vue.js 3.0使用了**Proxy**替代Object.defineProperty实现响应式，并且使用了静态提升技术来提高渲染性能。新增了编译时优化，在编译时进行模板静态分析，并生成更高效的渲染函数。
2. Composition API：Composition API是一个全新的组件逻辑复用方式，可以更好地组合和复用组件的逻辑。
3. TypeScript支持：Vue.js 3.0完全支持TypeScript，在编写Vue应用程序时可以更方便地利用TS的类型检查和自动补全功能。
4. 新的自定义渲染API：Vue.js 3.0的自定义渲染API允许开发者在细粒度上控制组件渲染行为，包括自定义渲染器、组件事件和生命周期等。
5. 改进的Vue CLI：Vue.js 3.0使用了改进的Vue CLI，可以更加灵活地配置项目，同时支持Vue.js2.x项目升级到Vue.js 3.0。
6. 移除一些API：Vue.js 3.0移除了一些不常用的API，如过渡相关API，部分修饰符等。

##### Proxy和Object.defineProperty的区别？

Proxy和Object.defineProperty都可以用来实现JavaScript对象的响应式，但是它们有一些区别：

1. 实现方式：Proxy是ES6新增的一种特性，使用了一种代理机制来实现响应式。而Object.defineProperty是在ES5中引入的，使用了getter和setter方法来实现。
2. 作用对象：Proxy可以代理**整个对象**，包括对象的所有属性、数组的所有元素以及类似数组对象的所有元素。而Object.defineProperty**只能代理对象上定义的属性**。
3. 监听属性：Proxy可以监听到新增属性和删除属性的操作，而Object.defineProperty**只能监听到已经**定义的属性的变化。
4. 性能：由于Proxy是ES6新增特性，其内部实现采用了更加高效的算法，相对于Object.defineProperty来说在性能方面有一定的优势。

综上所述，虽然Object.defineProperty在Vue.js 2.x中用来实现响应式，但是在Vue.js 3.0中已经采用了Proxy来替代，这是因为Proxy相对于Object.defineProperty拥有更优异的性能和更强大的能力。

##### Vue3升级了哪些重要功能？

- 新的API：Vue3使用createApp方法来创建应用程序实例，并有新的组件注册和调用方法。
- emits属性：：Vue 3的组件可以使用emits属性来声明事件。
- 生命周期
- 多个Fragment
- 移除.sync
- 异步组件的写法

```js
js

 体验AI代码助手
 代码解读
复制代码const Foo = defineAsyncComponent(() => import('./Foo.vue') )
```

##### vue2和vue3 核心 diff 算法区别？

Vue 2.x使用的是双向指针遍历的算法，也就是通过逐层比对新旧虚拟DOM树节点的方式来计算出更新需要做的最小操作集合。但这种算法的缺点是，由于遍历是从左到右、从上到下进行的，当发生节点删除或移动时，会导致其它节点位置的计算出现错误，因此会造成大量无效的重新渲染。

Vue 3.x使用了经过优化的单向遍历算法，也就是只扫描新虚拟DOM树上的节点，判断是否需要更新，跳过不需要更新的节点，进一步减少了不必要的操作。此外，在虚拟DOM创建后，Vue 3会缓存虚拟DOM节点的描述信息，以便于复用，这也会带来性能上的优势。同时，Vue 3还引入了静态提升技术，在编译时将一些静态的节点及其子节点预先处理成HTML字符串，大大提升了渲染性能。

因此，总体来说，Vue 3相对于Vue 2拥有更高效、更智能的diff算法，能够更好地避免不必要的操作，并提高了渲染性能。

##### Vue3为什么比Vue2快？

1. 响应式系统优化：Vue3引入了新的响应式系统，这个系统的设计让Vue3的渲染函数可以在编译时生成更少的代码，这也就意味着在运行时需要更少的代码来处理虚拟DOM。这个新系统的一个重要改进就是提供了一种基于Proxy实现的响应式机制，这种机制为开发人员提供更加高效的API，也减少了一些运行时代码。
2. 编译优化：Vue3的编译器对代码进行了优化，包括减少了部分注释、空白符和其他非必要字符的编译，同时也对编译后的代码进行了懒加载优化。
3. 更快的虚拟DOM：Vue3对虚拟DOM进行了优化，使用了跟React类似的Fiber算法，这样可以更加高效地更新DOM节点，提高性能。
4. Composition API：Vue3引入了Composition API，这种API通过提供逻辑组合和重用的方法来提升代码的可读性和重用性。这种API不仅可以让Vue3应用更好地组织和维护业务逻辑，还可以让开发人员更加轻松地实现优化。

##### Vue3如何实现响应式？

使用Proxy和Reflect API实现vue3响应式。

Reflect API则可以更加方便地实现对对象的监听和更新，可以用来访问、检查和修改对象的属性和方法，比如`Reflect.get`、`Reflect.set`、`Reflect.has`等。

Vue3会将响应式对象转换为一个Proxy对象，并利用Proxy对象的get和set拦截器来实现对属性的监听和更新。当访问响应式对象的属性时，get拦截器会被触发，此时会收集当前的依赖关系，并返回属性的值；当修改响应式对象的属性时，set拦截器会被触发，此时会触发更新操作，并通知相关的依赖进行更新。

优点：可监听属性的变化、新增与删除，监听数组的变化

##### vue3.0编译做了哪一些优化？

Vue 3.0作为Vue.js的一次重大升级，其编译器也进行了一些优化，主要包括以下几方面：

1. 静态树提升： Vue 3.0 通过重写编译器，实现对静态节点（即不改变的节点）进行编译优化，使用HoistStatic功能将静态节点移动到 render 函数外部进行缓存，从而服务端渲染和提高前端渲染的性能。
2. Patch Flag：在Vue 3.0中，编译的生成vnode会根据节点patch的标记，只对需要重新渲染的数据进行响应式更新，不需要更新的数据不会重新渲染，从而大大提高了渲染性能。
3. 静态属性提升：Vue3中对`不参与更新`的元素，会做静态提升，`只会被创建一次`，在渲染时直接复用。免去了重复的创建操作，优化内存。 没做静态提升之前，未参与更新的元素也在`render函数`内部，会重复`创建阶段`。
    做了静态提升后，未参与更新的元素，被`放置在render 函数外`，每次渲染的时候只要`取出`即可。同时该元素会被打上`静态标记值为-1`，特殊标志是`负整数`表示永远不会用于 `Diff`。
4. `事件监听缓存`：默认情况下绑定事件行为会被视为动态绑定（`没开启事件监听器缓存`），所以`每次`都会去追踪它的变化。`开启事件侦听器缓存`后，没有了静态标记。也就是说下次`diff算法`的时候`直接使用`。
5. 优化Render function：Vue 3.0的compile优化还包括：Render函数的换行和缩进、Render函数的条件折叠、Render函数的常量折叠等等。

总之，Vue 3.0通过多方面的编译优化，进一步提高了框架的性能和效率，使得Vue.js更加高效和易用。

##### watch和watchEffect的区别？

`watch` 和 `watchEffect` 都是监听器，`watchEffect` 是一个副作用函数。它们之间的区别有：

- `watch` ：既要指明监视的数据源，也要指明监视的回调。
- 而 `watchEffect` 可以自动监听数据源作为依赖。不用指明监视哪个数据，监视的回调中用到哪个数据，那就监视哪个数据。
- `watch` 可以访问`改变之前和之后`的值，`watchEffect` 只能获取`改变后`的值。
- `watch` 运行的时候`不会立即执行`，值改变后才会执行，而 `watchEffect` 运行后可`立即执行`。这一点可以通过 `watch` 的配置项 `immediate` 改变。
- `watchEffect`有点像 `computed` ：
  - 但 `computed` 注重的计算出来的值（回调函数的返回值）， 所以必须要写返回值。
  - 而 `watcheffect`注重的是过程（回调函数的函数体），所以不用写返回值。

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a343493e42514a9a8b78d8b8d43ca7a6~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

`watch`与 `vue2.x`中 `watch` 配置功能一致，但也有两个小坑

- 监视 `reactive` 定义的响应式数据时，`oldValue` 无法正确获取，强`制开启`了深度监视（deep配置失效）
- 监视 `reactive` 定义的响应式数据中`某个属性`时，`deep配置有效`。

```js
js 体验AI代码助手 代码解读复制代码let sum = ref(0)
let msg = ref('你好啊')
let person = reactive({
	name:'张三',
	age:18,
	job:{
		j1:{
			salary:20
		}
	}
})

//情况1：监视ref定义的响应式数据
watch(sum,(newValue, oldValue)=>{
	console.log("sum变化了", newValue, oldValue),(immediate:true)
})
//情况2：监视多个ref定义的响应式数据
watch([sum, msg],(newValue, oldValue)=>{
	console.log("sum或msg变化了", newValue, oldValue),(immediate:true)
})
//情况3：监视reactive定义的响应式数据
//若watch监视的是reactive定义的响应式数据，则无法正确获得oldValue，且强制开启了深度监视。
watch(person,(newValue, oldValue)=>{
	console.log("person变化了", newValue, oldValue),(immediate:true,deep:false) //此处的deep配置不再生效。
})
//情况4：监视reactive所定义的一个响应式数据中的某个属性
watch(()=>person.name,(newValue, oldValue)=>{
	console.log("person.name变化了", newValue, oldValue)
})
//情况5：监视reactive所定义的一个响应式数据中的某些属性
watch([()=>person.name, ()=>person.age],(newValue, oldValue)=>{
	console.log("person.name或person.age变化了", newValue, oldValue)
})
//特殊情况：
watch(()=>person.job,(newValue, oldValue)=>{
	console.log("person.job变化了", newValue, oldValue)
}, {deep:true})
```

##### 请介绍Vue3中的Teleport组件。

Vue 3 中新增了`teleport`（瞬移）组件，可以将组件的 DOM 插到指定的组件层，而不是默认的父组件层，可以用于在应用中创建模态框、悬浮提示框、通知框等组件。

`Teleport` 组件可以传递两个属性：

- `to` (必填)：指定组件需要挂载到的 DOM 节点的 ID，如果使用插槽的方式定义了目标容器也可以传入一个选择器字符串。
- `disabled` (可选)：一个标志位指示此节点是否应该被瞬移到目标中，一般情况下，这个 props 建议设为一个响应式变量来控制 caption 是否展示。

例子如下：

```vue
vue 体验AI代码助手 代码解读复制代码<template>
  <teleport to="#target">
    <div>这里是瞬移到target容器中的组件</div>
  </teleport>
  <div id="target"></div>
</template>
```

在上述示例中，`<teleport>` 组件往 `#target` 容器中，挂载了一个文本节点，效果等同于：

```vue
vue 体验AI代码助手 代码解读复制代码<template>
  <div id="target">
    <div>这里是瞬移到target容器中的组件</div>
  </div>
</template>
```

需要注意的是，虽然 DOM 插头被传送到另一个地方，但它的父组件仍然是当前组件，这一点必须牢记，否则会导致样式、交互等问题。

Teleport 组件不仅支持具体的 id/选择器，还可以为`to`属性绑定一个 Vue 组件实例，比如：

```vue
vue 体验AI代码助手 代码解读复制代码<template>
  <teleport :to="dialogRef">
    <div>这里是瞬移到Dialog组件里的组件</div>
  </teleport>
  <Dialog ref="dialogRef"></Dialog>
</template>
```

总之，`Teleport` 组件是 Vue3 中新增的一个非常有用的组件，可以方便地实现一些弹出框、提示框等组件的功能，提高了开发效率。

##### 如何理解reactive、ref 、toRef 和 toRefs？

- `ref`： 函数可以接收**原始数据类型**与**引用数据类型**。-   `ref`函数创建的响应式数据，在模板中可以直接被使用，在 `JS` 中需要通过 `.value` 的形式才能使用。
- `reactive`： 函数只能接收**引用数据类型**。
- `toRef`：针对一个响应式对象的属性创建一个ref，使得该属性具有响应式，两者之间保持引用关系。（入下所示，即让state中的age属性具有响应式）

![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/198bc4879a2f4756ac83e40605611e8c~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

- `toRefs`： 将一个**响应式对象**转为普通对象，对象的每一个属性都是对应的ref，两者保持引用关系

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/312caf8f9b604ec1a720dde64813b0bf~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

##### 谈谈pinia?

[Pinia](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fvuejs%2Fpinia) 是 `Vue` 官方团队成员专门开发的一个全新状态管理库，并且 `Vue` 的官方状态管理库已经更改为了 `Pinia`。在 [Vuex](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fvuejs%2Fvuex) 官方仓库中也介绍说可以把 `Pinia` 当成是不同名称的 `Vuex 5`，这也意味不会再出 `5` 版本了。

优点

- 更加轻量级，压缩后提交只有`1.6kb`。
- 完整的 `TS` 的支持，`Pinia` 源码完全由 `TS` 编码完成。
- 移除 `mutations`，只剩下 `state` 、 `actions` 、 `getters` 。
- 没有了像 `Vuex` 那样的模块镶嵌结构，它只有 `store` 概念，并支持多个 `store`，且都是互相独立隔离的。当然，你也可以手动从一个模块中导入另一个模块，来实现模块的镶嵌结构。
- 无需手动添加每个 `store`，它的模块默认情况下创建就自动注册。
- 支持服务端渲染（`SSR`）。
- 支持 `Vue DevTools`。
- 更友好的代码分割机制，[传送门](https://juejin.cn/post/7057439040911441957#heading-2)。

> `Pinia` 配套有个插件 [pinia-plugin-persist](https://link.juejin.cn/?target=https%3A%2F%2Fseb-l.github.io%2Fpinia-plugin-persist%2F)进行数据持久化，否则一刷新就会造成数据丢失

##### EventBus与mitt区别？

`Vue2` 中我们使用 `EventBus` 来实现跨组件之间的一些通信，它依赖于 `Vue` 自带的 `$on/$emit/$off` 等方法，这种方式使用非常简单方便，但如果使用不当也会带来难以维护的毁灭灾难。

而 `Vue3` 中移除了这些相关方法，这意味着 `EventBus` 这种方式我们使用不了， `Vue3` 推荐尽可能使用 `props/emits`、`provide/inject`、`vuex` 等其他方式来替代。

当然，如果 `Vue3` 内部的方式无法满足你，官方建议使用一些外部的辅助库，例如：[mitt](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fdevelopit%2Fmitt)。

优点

- 非常小，压缩后仅有 `200 bytes`。
- 完整 `TS` 支持，源码由 `TS` 编码。
- 跨框架，它并不是只能用在 `Vue` 中，`React`、`JQ` 等框架中也可以使用。
- 使用简单，仅有 `on`、`emit`、`off` 等少量实用API。

##### script setup 是干啥的？

`scrtpt setup` 是 `vue3` 的语法糖，简化了`组合式 API` 的写法，并且运行性能更好。使用 `script setup` 语法糖的特点：

- 属性和方法无需返回，可以直接使用。
- 引入`组件`的时候，会`自动注册`，无需通过 `components` 手动注册。
- 使用 `defineProps` 接收父组件传递的值。
- `useAttrs` 获取属性，`useSlots` 获取插槽，`defineEmits` 获取自定义事件。
- 默认`不会对外暴露`任何属性，如果有需要可使用 `defineExpose` 。

##### v-if 和 v-for 的优先级哪个高？

在 `vue2` 中 `v-for` 的优先级更高，但是在 `vue3` 中优先级改变了。`v-if` 的优先级更高。

##### setup中如何获得组件实例？

在 `setup` 函数中，你可以使用 `getCurrentInstance()` 方法来获取组件实例。`getCurrentInstance()` 方法返回一个对象，该对象包含了组件实例以及其他相关信息。

以下是一个示例：

```javascript
javascript 体验AI代码助手 代码解读复制代码import { getCurrentInstance } from 'vue';

export default {
  setup() {
    const instance = getCurrentInstance();

    // ...

    return {
      instance
    };
  }
};
```

在上面的示例中，我们使用 `getCurrentInstance()` 方法获取当前组件实例。然后，我们可以将该实例存储在一个常量中，并在 `setup` 函数的返回值中返回。

需要注意的是，`getCurrentInstance()` 方法只能在 `setup` 函数中使用，而不能在组件的生命周期方法（如 `created`、`mounted` 等方法）中使用。另外，需要注意的是，如果在 `setup` 函数返回之前访问了 `instance` 对象，那么它可能是 `undefined` ，因此我们需要对其进行处理。