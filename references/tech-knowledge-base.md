# 前端开发技术知识库

> 聚焦前端开发岗位高频面试问题，涵盖 HTML/CSS、JavaScript、TypeScript、React/Vue 框架、前端工程化、浏览器与网络、系统设计、Vibe Coding 与 AI 工具等核心方向。每个分类下列出面试最常问的问题及答案要点，面试官可按候选人背景和技术栈偏好灵活选题。

---

## 目录

- [1. HTML/CSS 基础](#1-htmlcss-基础)
  - [1.1 语义化与可访问性](#11-语义化与可访问性)
  - [1.2 CSS 布局](#12-css-布局)
  - [1.3 响应式设计](#13-响应式设计)
  - [1.4 CSS 工程化](#14-css-工程化)
- [2. JavaScript 核心](#2-javascript-核心)
  - [2.1 作用域与闭包](#21-作用域与闭包)
  - [2.2 异步编程](#22-异步编程)
  - [2.3 原型与继承](#23-原型与继承)
  - [2.4 ES6+ 特性](#24-es6-特性)
- [3. TypeScript](#3-typescript)
  - [3.1 类型系统基础](#31-类型系统基础)
  - [3.2 高级类型](#32-高级类型)
  - [3.3 工程实践](#33-工程实践)
- [4. 框架深度](#4-框架深度)
  - [4.1 React 生态](#41-react-生态)
  - [4.2 Vue 生态](#42-vue-生态)
- [5. 前端工程化](#5-前端工程化)
  - [5.1 构建工具](#51-构建工具)
  - [5.2 性能优化](#52-性能优化)
  - [5.3 测试](#53-测试)
  - [5.4 包管理与 Monorepo](#54-包管理与-monorepo)
- [6. 浏览器与网络](#6-浏览器与网络)
  - [6.1 浏览器渲染原理](#61-浏览器渲染原理)
  - [6.2 网络基础](#62-网络基础)
  - [6.3 前端安全](#63-前端安全)
- [7. 前端系统设计](#7-前端系统设计)
  - [7.1 组件库设计](#71-组件库设计)
  - [7.2 微前端架构](#72-微前端架构)
  - [7.3 前端监控](#73-前端监控)
  - [7.4 跨端开发](#74-跨端开发)
- [8. Vibe Coding 与 AI 工具](#8-vibe-coding-与-ai-工具)
  - [8.1 AI 编程工具使用](#81-ai-编程工具使用)
  - [8.2 开放性问题](#82-开放性问题)
- [9. 前端 × AI 产品实战](#9-前端--ai-产品实战)
  - [9.1 AI 场景中的通信与数据流](#91-ai-场景中的通信与数据流)
  - [9.2 AI 产品前端性能](#92-ai-产品前端性能)
  - [9.3 AI 平台前端架构](#93-ai-平台前端架构)

---

## 1. HTML/CSS 基础

### 1.1 语义化与可访问性

- **什么是 HTML 语义化？为什么重要？** — 使用正确的标签表达内容含义（如 `<article>`、`<nav>`、`<aside>`、`<header>`、`<footer>`），而非全部用 `<div>`。好处：提升 SEO（搜索引擎理解页面结构）、增强可访问性（屏幕阅读器依赖语义）、代码可维护性更高、团队协作更清晰
- **ARIA 是什么？什么时候需要用？** — Accessible Rich Internet Applications，W3C 规范，为动态内容和自定义控件补充语义信息。原则：能用原生 HTML 语义元素就不用 ARIA；自定义组件（如手写 Tab、Modal、Dropdown）必须补充 `role`、`aria-label`、`aria-expanded` 等属性。常见 role：`role="dialog"`、`role="tablist"`、`role="alert"`
- **`<img>` 标签的 alt 属性怎么写？空 alt 和没有 alt 的区别？** — alt 描述图片内容，供屏幕阅读器朗读和图片加载失败时展示。`alt=""` 表示装饰性图片，屏幕阅读器会跳过；没有 alt 属性则屏幕阅读器会尝试读取文件名，体验很差
- **如何做好前端 SEO？** — 语义化标签；合理的 heading 层级（h1 唯一）；meta 标签（title、description、og 标签）；结构化数据（JSON-LD）；SSR/SSG 提供完整 HTML；图片 alt 和 lazy loading；合理的 URL 结构和 sitemap

### 1.2 CSS 布局

- **CSS 中如何实现打字机效果的动画？** — 打字机效果可以通过控制容器宽度或利用 `steps()` 时间函数配合光标闪烁来实现。纯 CSS 方案：使用 `white-space: nowrap` 和 `overflow: hidden` 隐藏溢出内容；通过 `animation` 改变 `width` 从 0 到 100%；使用 `steps()` 函数让动画呈现断续的打字感，而不是平滑过渡；利用右边框 `border-right` 模拟闪烁的光标。常见坑：字符宽度不一致导致动画偏移（建议使用等宽字体 monospace）；无法动态适配多行文本。追问：如果是 AI 实时生成的流式文本，用 CSS 动画实现还是 JS 实现更好？——JS 更合适，因为文本长度不可预知且逐 Token 到达，CSS 动画需要预知最终宽度

- **Flexbox 和 Grid 的区别？各适合什么场景？** — Flexbox 是一维布局（行或列），适合组件内部布局（导航栏、按钮组、卡片内容排列）。Grid 是二维布局（行和列同时控制），适合页面整体布局（仪表盘、复杂表格式布局）。实际项目中两者经常嵌套使用
- **BFC 是什么？怎么触发？有什么用？** — Block Formatting Context（块级格式化上下文），是一个独立的渲染区域。触发条件：`overflow` 非 `visible`、`display: flow-root`、`float`、`position: absolute/fixed`、`display: flex/grid`。用途：清除浮动、阻止外边距折叠（margin collapse）、防止元素被浮动元素覆盖
- **层叠上下文（Stacking Context）是什么？z-index 为什么有时不生效？** — 层叠上下文是一个三维概念，决定元素在 Z 轴的堆叠顺序。创建条件：`position` 非 `static` 且设置了 `z-index`、`opacity < 1`、`transform`、`filter`、`will-change` 等。z-index 只在同一层叠上下文内比较，跨上下文无效
- **`position: sticky` 的工作原理和常见坑？** — 元素在滚动到特定位置前表现为 `relative`，到达阈值后表现为 `fixed`。常见坑：父元素 `overflow: hidden/auto` 会导致失效；需要指定 `top/bottom` 等阈值；父元素高度不足时无法"粘"住

### 1.3 响应式设计

- **移动端适配有哪些方案？各有什么优缺点？** — rem 方案：根据根元素 font-size 等比缩放，需要配合 `postcss-pxtorem`，适合等比还原设计稿；vw/vh 方案：直接用视口单位，无需 JS 计算，但部分极端尺寸下体验差；媒体查询断点方案：在不同屏幕尺寸下使用不同布局，灵活但维护成本高；容器查询（Container Query）：基于父容器尺寸响应，组件级响应式的未来方向
- **1px 问题是什么？怎么解决？** — 移动端高 DPR（设备像素比）屏幕上，CSS 的 1px 实际渲染为 2-3 个物理像素，看起来偏粗。解决方案：`transform: scaleY(0.5)` 配合伪元素；`border-image` 使用渐变；`box-shadow` 模拟；`-webkit-min-device-pixel-ratio` 媒体查询
- **视口（Viewport）相关的 meta 标签怎么配置？** — `<meta name="viewport" content="width=device-width, initial-scale=1.0">`。`width=device-width` 设置视口宽度为设备宽度；`initial-scale=1.0` 初始缩放比例；`user-scalable=no` 禁止用户缩放（慎用，影响可访问性）
- **什么是容器查询（Container Query）？解决什么问题？** — `@container` 允许组件根据父容器尺寸而非视口尺寸做出响应式调整。解决了组件在不同布局上下文中无法自适应的问题（如同一卡片组件在侧边栏和主内容区需要不同布局），使组件真正可复用

### 1.4 CSS 工程化

- **CSS Modules、CSS-in-JS、Tailwind CSS 的区别和选型？** — CSS Modules：文件级作用域，自动生成唯一类名，零运行时，适合中大型项目；CSS-in-JS（styled-components、Emotion）：JS 中写样式，动态样式方便，但有运行时开销（RSC 场景下有限制）；Tailwind CSS：原子化工具类，开发速度快，无命名负担，但 HTML 类名冗长，需要习惯。选型看团队和项目：Tailwind 适合快速迭代，CSS Modules 适合大型工程，CSS-in-JS 适合高度动态的 UI
- **CSS 预处理器（Sass/Less）还有必要用吗？** — 有价值但逐渐被替代。原生 CSS 已支持变量（Custom Properties）、嵌套（Nesting）、`@layer`。预处理器仍有优势的场景：Mixin 复用、函数计算、复杂主题系统。新项目可考虑 PostCSS + 原生 CSS 替代
- **什么是原子化 CSS？Tailwind 的原理？** — 每个类只做一件事（如 `p-4`、`text-center`、`bg-blue-500`），通过组合类名构建样式。Tailwind 在构建时通过 PurgeCSS（现为内置引擎）扫描模板文件，只保留用到的类，最终 CSS 体积很小。JIT（Just-In-Time）模式按需生成，开发体验接近即时
- **CSS `@layer` 有什么用？** — 控制样式的层叠优先级。可以将样式分为 `base`、`components`、`utilities` 等层，无论代码顺序如何，层的优先级始终一致。解决了大型项目中样式覆盖顺序不可控的问题，配合组件库使用效果显著

---

## 2. JavaScript 核心

### 2.1 作用域与闭包

- **JavaScript 有哪些作用域？** — 全局作用域、函数作用域、块级作用域（`let`/`const`，ES6+）。词法作用域（Lexical Scope）：作用域在代码编写时确定，而非运行时。`var` 是函数作用域，`let`/`const` 是块级作用域。暂时性死区（TDZ）：`let`/`const` 声明前不可访问
- **什么是闭包？有什么实际应用？** — 闭包是函数及其词法环境的组合，内部函数持有外部函数变量的引用。应用：数据私有化（模拟私有变量）、函数工厂、柯里化（Currying）、防抖/节流（debounce/throttle）、模块模式。React Hooks 本质上大量依赖闭包
- **闭包会导致内存泄漏吗？怎么避免？** — 闭包本身不一定泄漏，但如果闭包持有的引用不再需要却无法被 GC 回收，就会泄漏。常见场景：事件监听器未移除、定时器未清除、DOM 引用残留。避免方法：及时解除引用（`= null`）、组件卸载时清理副作用（React `useEffect` 的 cleanup）、使用 WeakMap/WeakRef
- **`this` 的指向规则？箭头函数的 this 有什么不同？** — 普通函数 `this` 取决于调用方式：默认绑定（全局/undefined）、隐式绑定（obj.fn()）、显式绑定（call/apply/bind）、new 绑定。箭头函数没有自己的 `this`，继承外层词法作用域的 `this`，且不能被 call/apply/bind 改变。在 React 类组件中常用箭头函数避免 this 绑定问题

### 2.2 异步编程

- **Event Loop 是什么？微任务和宏任务的区别？** — Event Loop 是 JS 的运行机制，单线程通过事件循环实现异步非阻塞。执行顺序：同步代码 → 微任务队列（Promise.then、MutationObserver、queueMicrotask）→ 宏任务队列（setTimeout、setInterval、requestAnimationFrame、I/O）。每轮事件循环先清空所有微任务，再执行一个宏任务
- **Promise 的核心 API 和错误处理？** — 三种状态：pending → fulfilled/rejected，不可逆。核心 API：`.then()`、`.catch()`、`.finally()`。组合方法：`Promise.all`（全部成功）、`Promise.allSettled`（全部完成不管成败）、`Promise.race`（最快的）、`Promise.any`（第一个成功的）。错误处理：`.catch()` 捕获链上所有错误；`unhandledrejection` 事件捕获未处理的 rejection
- **async/await 的原理？和 Promise 的关系？** — async 函数返回 Promise，await 暂停函数执行直到 Promise 完成。本质是 Generator + Promise 的语法糖。注意事项：`await` 只能在 async 函数中使用（顶层 await 需 ES Module）；并发请求用 `Promise.all` 而非逐个 `await`；错误处理用 try/catch 或 `.catch()`
- **requestAnimationFrame 和 setTimeout 的区别？** — `requestAnimationFrame`（rAF）在浏览器下一次重绘前执行，频率与屏幕刷新率同步（通常 60fps），适合动画和 DOM 操作。`setTimeout` 是宏任务，精度不高（最低 4ms），不与渲染同步，可能导致掉帧。性能优化中 DOM 批量操作应优先用 rAF

### 2.3 原型与继承

- **原型链是什么？`__proto__` 和 `prototype` 的区别？** — 每个对象都有 `__proto__`（内部属性 `[[Prototype]]`），指向其构造函数的 `prototype`。属性查找沿原型链向上：实例 → 构造函数.prototype → Object.prototype → null。`prototype` 是函数特有的属性，`__proto__` 是所有对象都有的
- **`new` 操作符做了什么？** — 四步：① 创建空对象；② 将空对象的 `__proto__` 指向构造函数的 `prototype`；③ 将构造函数的 `this` 指向新对象并执行；④ 如果构造函数返回对象则返回该对象，否则返回新创建的对象
- **ES6 class 和 ES5 构造函数的区别？** — class 是语法糖，本质仍基于原型。区别：class 声明不会提升；class 内部默认严格模式；class 的方法不可枚举；class 必须用 `new` 调用；`extends` 实现继承比 ES5 的组合继承（`call` + `Object.create`）更简洁
- **`instanceof` 的原理？有什么局限？** — 沿着对象的原型链查找是否存在构造函数的 `prototype`。局限：跨 iframe 失效（不同执行上下文的构造函数不同）；不能检测基本类型；可以被 `Symbol.hasInstance` 修改行为。替代方案：`Object.prototype.toString.call()` 更可靠

### 2.4 ES6+ 特性

- **如何判断一个对象是否是数组？至少列举三种方法？** — ① `Array.isArray(obj)`（最推荐，ES5+，跨 iframe 可靠）；② `Object.prototype.toString.call(obj) === '[object Array]'`（万能方法，跨 iframe 可靠）；③ `obj instanceof Array`（同一执行上下文下可用，跨 iframe 失效）；④ `obj.constructor === Array`（可被篡改，不推荐）；⑤ `Array.prototype.isPrototypeOf(obj)`（原型链检查）。面试重点：理解 `instanceof` 跨 iframe 失效的原因（不同全局执行上下文中 `Array` 构造函数不同），以及为什么 `Array.isArray` 是首选方案
- **手写防抖函数（Debounce）并解释应用场景？** — 防抖是指事件触发后延迟执行，如果在延迟时间内再次触发则重新计时。核心原理：利用闭包存储 timer 变量，每次触发先 `clearTimeout` 再 `setTimeout`。实现：`function debounce(fn, delay) { let timer = null; return function(...args) { if (timer) clearTimeout(timer); timer = setTimeout(() => { fn.apply(this, args); }, delay); }; }`。应用场景：AI 搜索建议（防止每输入一个字符就调用 LLM 接口）、窗口 resize、表单校验。进阶：支持 `immediate`（首次立即执行）、`cancel` 方法、返回 Promise。与节流（Throttle）的区别：节流是固定间隔内最多执行一次（如滚动事件），防抖是停止触发后才执行
- **解构赋值有哪些高级用法？** — 嵌套解构、默认值、重命名（`{ name: userName }`）、剩余参数（`...rest`）、动态键（`{ [key]: value }`）。函数参数解构配合默认值非常常用。注意：解构 `null`/`undefined` 会报错
- **Proxy 和 Reflect 是什么？有什么应用？** — Proxy 创建对象的代理，拦截属性读取、赋值、删除等操作。Reflect 提供与 Proxy handler 一一对应的静态方法。应用：响应式数据（Vue 3 的核心）、数据验证、日志记录、实现观察者模式、不可变数据。13 种可拦截操作（trap）：get、set、has、deleteProperty、apply 等
- **Symbol 的作用？** — 创建唯一的属性键，避免命名冲突。应用：定义对象私有属性（非真正私有，但不易被遍历到）；内置 Symbol 修改对象行为（`Symbol.iterator`、`Symbol.toPrimitive`、`Symbol.hasInstance`）；作为常量枚举值
- **Iterator 和 Generator 是什么？** — Iterator 是一种接口协议，提供 `next()` 方法按序访问元素。部署了 `Symbol.iterator` 的对象可用 `for...of` 遍历（Array、Map、Set、String 原生支持）。Generator 函数（`function*`）返回 Iterator，`yield` 暂停执行，`next()` 恢复。应用：惰性求值、异步流程控制（被 async/await 替代大部分场景）、无限序列

---

## 3. TypeScript

### 3.1 类型系统基础

- **TypeScript 相比 JavaScript 的核心价值？** — 静态类型检查：编译时发现类型错误而非运行时；更好的 IDE 支持（自动补全、重构、跳转定义）；代码即文档（类型注解是最好的文档）；大型项目可维护性显著提升。代价：学习曲线、编译步骤、类型体操可能过度复杂
- **联合类型和交叉类型的区别？** — 联合类型（`A | B`）：值可以是 A 或 B，使用时需要类型收窄（narrowing）。交叉类型（`A & B`）：值同时满足 A 和 B，常用于对象类型合并。注意：联合类型只能访问共有属性（除非收窄），交叉类型可以访问所有属性
- **泛型是什么？怎么用？** — 泛型是类型的参数，让函数/类/接口能处理多种类型且保持类型安全。常见用法：泛型函数（`function identity<T>(arg: T): T`）、泛型接口、泛型约束（`<T extends SomeType>`）、默认泛型（`<T = string>`）。React 中 `useState<T>`、`FC<Props>` 都是泛型应用
- **`type` 和 `interface` 的区别？什么时候用哪个？** — interface 可以 `extends` 继承、可以合并声明（Declaration Merging）、适合定义对象结构和类实现。type 可以用联合类型、交叉类型、映射类型、条件类型，表达能力更强。实践建议：对象结构优先用 interface；需要联合/交叉/复杂计算时用 type；团队保持统一即可

### 3.2 高级类型

- **条件类型怎么用？`infer` 关键字的作用？** — 条件类型：`T extends U ? X : Y`，根据类型关系选择不同结果。`infer` 在条件类型中声明待推断的类型变量。经典示例：`ReturnType<T>` 提取函数返回值类型：`type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never`。实际应用：提取 Promise 内部类型、提取数组元素类型
- **映射类型（Mapped Types）是什么？** — 基于已有类型生成新类型，遍历键并转换。内置工具类型大量使用：`Partial<T>`（全部可选）、`Required<T>`（全部必填）、`Readonly<T>`（全部只读）、`Pick<T, K>`（选取部分键）、`Omit<T, K>`（排除部分键）、`Record<K, V>`（构造对象类型）。自定义映射：`{ [K in keyof T]: NewType }`
- **模板字面量类型有什么用？** — 在类型层面拼接字符串：`` type EventName = `on${Capitalize<string>}` ``。应用：类型安全的事件名、CSS 属性名、路由路径。配合联合类型可以生成所有组合：`` type Color = 'red' | 'blue'; type Size = 'sm' | 'lg'; type ClassName = `${Color}-${Size}` `` 生成 `'red-sm' | 'red-lg' | 'blue-sm' | 'blue-lg'`
- **TypeScript 的类型收窄（Narrowing）有哪些方式？** — `typeof` 守卫（基本类型）、`instanceof` 守卫（类实例）、`in` 守卫（属性检查）、自定义类型守卫（`is` 关键字）、可辨识联合（Discriminated Union，用共有字段区分）、`satisfies` 运算符（TS 4.9+，检查类型但保留推断）。React 中 props 的条件渲染大量依赖类型收窄

### 3.3 工程实践

- **`any`、`unknown`、`never` 的区别？** — `any`：放弃类型检查，能赋值给任何类型也能接收任何类型，应尽量避免。`unknown`：类型安全的 `any`，能接收任何类型但使用前必须收窄。`never`：不可能存在的类型，用于穷尽检查（exhaustive check）和永远不会返回的函数（如 `throw`）。优先级：`never` > 具体类型 > `unknown` > `any`
- **strict 模式下有哪些关键检查？** — `strictNullChecks`（null/undefined 不能赋值给其他类型）、`strictFunctionTypes`（函数参数类型逆变检查）、`noImplicitAny`（禁止隐式 any）、`strictPropertyInitialization`（类属性必须初始化）。建议新项目始终开启 strict，老项目逐步迁移
- **如何为第三方库写类型声明？** — 查找 `@types/xxx` 包（DefinitelyTyped 社区维护）。没有时：创建 `.d.ts` 声明文件，用 `declare module 'xxx'` 导出类型。全局类型：`declare global { ... }`。最佳实践：先用 `any` 快速通过，再逐步细化类型；贡献类型定义回社区
- **TypeScript 项目中常见的类型体操有用吗？** — 适度有用，过度则有害。实际有用的场景：API 响应类型自动推断、表单验证类型安全、路由参数类型约束。过度的信号：类型比业务逻辑还复杂、团队成员看不懂、类型编译时间过长。原则：类型是为代码服务的，不是为炫技

---

## 4. 框架深度

> 面试官根据候选人选择的技术栈（React 或 Vue）选取对应小节提问。两个框架都熟悉的候选人可以交叉提问。

### 4.1 React 生态

- **React Hooks 的设计动机？解决了什么问题？** — 解决了类组件的三个痛点：组件间逻辑复用困难（HOC 和 Render Props 嵌套地狱）、复杂组件逻辑分散在生命周期中难以理解、class 的 this 绑定问题。Hooks 让函数组件拥有状态和副作用能力，逻辑复用通过自定义 Hook 实现
- **useState 的原理？为什么不能在条件语句中使用 Hook？** — React 通过 Fiber 节点上的链表按顺序存储每个 Hook 的状态。每次渲染时按调用顺序匹配链表节点，如果条件语句导致 Hook 调用顺序改变，状态就会错位。这是 Hooks 规则（Rules of Hooks）的根本原因
- **useEffect 的依赖数组怎么正确使用？常见的坑？** — 依赖数组决定 effect 何时重新执行。空数组 `[]` 只在挂载时执行一次（类似 componentDidMount）；不传则每次渲染都执行。常见坑：对象/数组引用变化导致无限循环（需 useMemo/useCallback 或拆解为基本类型）；闭包陷阱（effect 中捕获的是旧状态）；cleanup 函数不要遗忘（清除订阅、定时器）
- **React 的渲染机制：Virtual DOM → Reconciliation → Commit？** — ① Virtual DOM：用 JS 对象描述 UI 树；② Reconciliation（协调）：Diff 算法比较新旧 VDOM，找出最小变更（同层比较、key 优化列表 diff）；③ Commit：将变更应用到真实 DOM。Fiber 架构将渲染拆分为可中断的工作单元，支持时间切片（Time Slicing）和优先级调度
- **React Fiber 架构解决了什么问题？工作原理？** — Fiber 主要解决 React 在大量 DOM 更新时导致浏览器卡顿的问题，将同步更新变为可中断的异步更新。核心思想是"时间分片"（Time Slicing），将渲染工作拆分为多个小任务。工作原理：① 引入双缓存技术，在内存中构建新的 Fiber 树（workInProgress tree），完成后一次性替换当前树（current tree）；② 任务具有优先级（Priority），高优先级任务（如用户输入）可以打断低优先级任务（如数据请求后的渲染）；③ 分为 Reconciliation（协调阶段，可中断）和 Commit（提交阶段，不可中断）两个阶段。常见坑：误以为 Fiber 提升了单次计算的速度（实际上是提升了响应性）；在协调阶段执行了具有副作用的操作。追问：为什么 React 决定在 Commit 阶段不能中断？——因为 Commit 阶段操作真实 DOM，中断会导致 UI 不一致
- **React.memo、useMemo、useCallback 各有什么用？什么时候该用？** — `React.memo`：对组件进行浅比较，props 不变则跳过重渲染。`useMemo`：缓存计算结果，避免昂贵计算重复执行。`useCallback`：缓存函数引用，配合 memo 子组件避免不必要重渲染。注意：不要过早优化，只在性能分析确认有瓶颈时使用；React Compiler（React 19+）会自动处理部分场景
- **React 状态管理方案怎么选？** — Context：简单跨层级传递，不适合高频更新（会导致消费者全部重渲染）。Redux Toolkit：复杂全局状态，中间件生态丰富，适合大型应用。Zustand：轻量，API 简洁，基于发布订阅，支持选择性重渲染。Jotai/Recoil：原子化状态，适合细粒度响应式更新。选型看复杂度：简单用 Context + useReducer，中等用 Zustand，复杂用 Redux Toolkit
- **Next.js 的 App Router 和 Pages Router 有什么区别？RSC 是什么？** — App Router（Next.js 13+）：基于 React Server Components，默认服务端渲染，支持流式渲染、并行路由、嵌套布局。Pages Router：传统 CSR + SSR/SSG 模式。RSC（React Server Components）：组件在服务端执行，不发送 JS 到客户端，减少 bundle 体积。`'use client'` 标记客户端组件边界
- **React 19 有什么重要更新？** — React Compiler（自动 memo）：编译时自动优化，不再需要手动 useMemo/useCallback。Actions：`useActionState`、`useFormStatus` 简化表单处理。`use()` Hook：在渲染中读取 Promise 和 Context。文档元数据：`<title>`、`<meta>` 可直接在组件中使用。Server Actions：服务端函数直接在客户端调用

### 4.2 Vue 生态

- **Vue 3 的响应式原理？Proxy 比 defineProperty 好在哪？** — Vue 3 用 `Proxy` 代理整个对象，拦截所有属性操作。相比 Vue 2 的 `Object.defineProperty`：① 可以检测属性新增和删除（不需要 `Vue.set`）；② 可以监听数组索引变化和 length；③ 性能更好（惰性代理，按需递归）；④ 代码更简洁。核心流程：Proxy 拦截 get → 依赖收集（track）→ Proxy 拦截 set → 触发更新（trigger）
- **ref 和 reactive 的区别？什么时候用哪个？** — `ref`：包装任何类型的值，访问需要 `.value`，模板中自动解包。`reactive`：只能包装对象，直接访问属性，不能整体替换（会丢失响应性）。实践建议：基本类型用 `ref`，对象用 `reactive` 或 `ref` 都行；组合函数返回值推荐 `ref`（解构不丢失响应性）
- **Composition API vs Options API？** — Options API（data/methods/computed/watch）：按选项分类，简单直观，但逻辑分散。Composition API（setup + ref/reactive/computed/watch）：按功能组织，逻辑可提取为组合函数（composables），复用性强。Vue 3 推荐 Composition API，特别是 `<script setup>` 语法糖
- **Vue 的 computed 和 watch 的区别？** — `computed`：基于依赖缓存的派生值，只读（默认），有返回值，依赖不变则不重新计算。`watch`/`watchEffect`：监听变化执行副作用（API 调用、DOM 操作），无返回值。`watchEffect` 自动追踪依赖，`watch` 需要显式指定。原则：能用 computed 就不用 watch
- **Pinia 和 Vuex 的区别？** — Pinia 是 Vuex 的精神继承者（Vuex 5 提案演变而来）。区别：① 去掉 mutations（直接修改 state）；② 完整的 TypeScript 支持；③ 更轻量（约 1KB）；④ 支持 Composition API 风格；⑤ 无需嵌套模块（每个 store 独立）。Pinia 是 Vue 3 官方推荐的状态管理方案
- **Vue Router 的导航守卫有哪些？** — 全局守卫：`beforeEach`、`beforeResolve`、`afterEach`。路由独享守卫：`beforeEnter`。组件内守卫：`beforeRouteEnter`、`beforeRouteUpdate`、`beforeRouteLeave`。完整导航流程：beforeRouteLeave → beforeEach → beforeRouteUpdate → beforeEnter → beforeResolve → afterEach。实际应用：权限控制、页面标题设置、数据预加载
- **Nuxt 3 的核心特性？** — 基于 Vue 3 + Vite，开箱即用的全栈框架。核心特性：文件系统路由（`pages/` 目录自动生成路由）、自动导入（组件和组合函数无需手动 import）、服务端渲染（SSR/SSG/ISR 灵活切换）、Nitro 服务引擎（跨平台部署）、`useFetch`/`useAsyncData` 数据获取

---

## 5. 前端工程化

### 5.1 构建工具

- **Vite 为什么比 Webpack 快？** — 开发阶段：Vite 利用浏览器原生 ES Module，按需加载模块（不打包），冷启动极快。Webpack 必须先打包再启动。热更新（HMR）：Vite 只更新变更模块，Webpack 需要重新构建依赖链。生产构建：Vite 使用 Rollup（底层 esbuild 预构建依赖），Webpack 使用自己的打包逻辑。esbuild（Go 编写）做依赖预构建，速度比 JS 编写的打包器快 10-100 倍
- **Webpack 的核心概念？** — Entry（入口）、Output（输出）、Loader（文件转换：babel-loader、css-loader、ts-loader）、Plugin（扩展功能：HtmlWebpackPlugin、MiniCssExtractPlugin）、Module（模块化处理）、Chunk（代码块）。Webpack 5 新特性：Module Federation（模块联邦）、持久化缓存、Asset Modules（替代 file-loader/url-loader）
- **Tree Shaking 的原理？为什么有时不生效？** — 基于 ES Module 的静态分析，在编译时确定哪些导出未被使用并移除。不生效原因：使用了 CommonJS（`require`）导致无法静态分析；副作用代码未标记（需在 `package.json` 中设置 `"sideEffects": false`）；Babel 将 ESM 转为 CJS；re-export 全部导出（`export * from`）
- **esbuild 和 Turbopack 是什么？** — esbuild：用 Go 编写的极速 JS/TS 打包器和编译器，Vite 用它做依赖预构建和 TS/JSX 转换，但不支持低版本浏览器和部分 Babel 插件。Turbopack：Vercel 开发的 Rust 编写打包器，Next.js 的未来打包方案，增量计算（不重复打包未变更部分），目前处于迭代中

### 5.2 性能优化

- **Core Web Vitals 是什么？怎么优化？** — Google 定义的三个核心指标：LCP（Largest Contentful Paint，最大内容绘制，≤2.5s）衡量加载速度；INP（Interaction to Next Paint，交互响应，≤200ms）衡量交互性；CLS（Cumulative Layout Shift，累计布局偏移，≤0.1）衡量视觉稳定性。优化方向：LCP → 优化关键资源加载（预加载、CDN、图片优化）；INP → 减少主线程阻塞（代码分割、Web Worker）；CLS → 预留元素尺寸（图片/广告设置宽高）
- **懒加载有哪些实现方式？** — 路由懒加载：`React.lazy` + `Suspense`（React）、动态 `import()` + `defineAsyncComponent`（Vue）。组件懒加载：同上，配合 loading/error 状态。图片懒加载：`<img loading="lazy">`（原生）、Intersection Observer API（自定义）。第三方库按需引入：`import { Button } from 'antd'`（配合 Tree Shaking）
- **代码分割（Code Splitting）怎么做？** — 路由级分割：每个路由一个 chunk，最常用。组件级分割：大型组件动态导入。第三方库分割：`splitChunks`（Webpack）或 `manualChunks`（Vite/Rollup）将 vendor 单独打包。分割原则：首屏只加载必要代码；共享代码提取为公共 chunk；避免过度分割导致请求过多
- **前端缓存策略有哪些？** — 强缓存（`Cache-Control: max-age`）：不发请求，直接用本地缓存。协商缓存（`ETag`/`Last-Modified`）：发请求验证是否变更。Service Worker 缓存：离线可用，策略灵活（Cache First、Network First、Stale While Revalidate）。实践：HTML 使用协商缓存（随时更新）；JS/CSS/图片使用强缓存 + 文件名 hash（内容变更则 hash 变）
- **如何配置 Webpack 或 Vite 来优化首屏加载速度？** — Webpack 方面：① `splitChunks` 配置（将 vendor、公共模块、业务代码分离，`chunks: 'all'`）；② `MiniCssExtractPlugin` 提取 CSS 减少 JS 体积；③ `TerserPlugin` 压缩 + Tree Shaking；④ `externals` 将大型库（React、Lodash）排除为 CDN 引用；⑤ 持久化缓存（`cache: { type: 'filesystem' }`）加速二次构建。Vite 方面：① `build.rollupOptions.output.manualChunks` 手动分包；② `build.cssCodeSplit: true` CSS 按路由分割；③ 依赖预构建（`optimizeDeps`）减少开发时请求数。通用策略：路由级 Code Splitting（`React.lazy` / 动态 `import()`）；关键 CSS 内联（Critical CSS）；资源预加载（`<link rel="preload">`）；图片压缩 + WebP/AVIF 格式 + 懒加载；开启 Gzip/Brotli 压缩

### 5.3 测试

- **前端测试的分层策略？** — 测试金字塔：单元测试（最多，成本最低）→ 集成测试/组件测试 → E2E 测试（最少，成本最高）。单元测试：纯函数、工具函数、hooks（Vitest/Jest）。组件测试：渲染 + 交互验证（Testing Library）。E2E 测试：完整用户流程（Playwright/Cypress）。覆盖率不是唯一指标，测试质量比数量重要
- **Testing Library 的设计哲学？** — "The more your tests resemble the way your software is used, the more confidence they can give you." 核心原则：从用户视角测试（按角色、文本、标签查询，而非 class/id）；避免测试实现细节；优先 `getByRole` > `getByText` > `getByTestId`。React Testing Library 不鼓励 shallow rendering
- **如何测试自定义 Hook？** — 使用 `@testing-library/react` 的 `renderHook`：在测试中渲染使用 Hook 的组件，调用 `act` 触发状态更新，断言返回值。复杂 Hook（涉及异步、Context）需要包装 Provider。注意：不要直接调用 Hook 函数（违反 Rules of Hooks）
- **E2E 测试工具怎么选？Playwright vs Cypress？** — Playwright：多浏览器（Chromium、Firefox、WebKit）、多语言、并行执行、支持移动端模拟，社区增长快。Cypress：单浏览器（主要 Chrome），API 友好，实时调试体验好，文档完善。趋势：Playwright 逐渐成为主流选择，特别在 CI/CD 场景

### 5.4 包管理与 Monorepo

- **pnpm 相比 npm/yarn 的优势？** — 硬链接 + 符号链接：所有包统一存储在全局 store，项目内通过链接引用，磁盘占用极小。严格的依赖结构：非扁平化 `node_modules`（`.pnpm` 目录），防止幽灵依赖（phantom dependencies）。速度快：安装速度通常优于 npm/yarn。原生支持 workspace（Monorepo）
- **Monorepo 是什么？有什么优缺点？** — 多个项目/包放在同一个仓库管理。优点：代码共享方便（本地引用，无需发包）、统一版本管理、原子化提交、CI/CD 统一。缺点：仓库体积大、权限管理粗粒度、构建编排复杂。工具：pnpm workspace、Turborepo、Nx、Lerna。适合场景：组件库 + 文档站 + 示例项目、多个相关微服务的前端
- **Turborepo 解决什么问题？** — Monorepo 下的构建编排和缓存。核心能力：任务管道（Pipeline）定义构建依赖顺序；增量构建（只构建变更的包）；远程缓存（团队成员共享构建结果）；并行执行独立任务。配合 pnpm workspace 使用效果最佳
- **changesets 是什么？怎么管理版本发布？** — 语义化版本管理工具，适合 Monorepo 多包发布。流程：开发者提交 changeset（描述变更和版本类型 major/minor/patch）→ CI 汇总 changesets → 自动更新版本号和 CHANGELOG → 发布到 npm。解决了 Monorepo 中多包版本联动的复杂问题

---

## 6. 浏览器与网络

### 6.1 浏览器渲染原理

- **从输入 URL 到页面渲染，经历了什么？** — ① DNS 解析（域名 → IP）；② TCP 连接（三次握手，HTTPS 还有 TLS 握手）；③ 发送 HTTP 请求；④ 服务端处理并返回响应；⑤ 浏览器解析 HTML → 构建 DOM 树；⑥ 解析 CSS → 构建 CSSOM 树；⑦ DOM + CSSOM → 渲染树（Render Tree）；⑧ 布局（Layout）计算几何位置；⑨ 绘制（Paint）；⑩ 合成（Composite）图层合并输出
- **重排（Reflow）和重绘（Repaint）的区别？怎么减少？** — 重排：元素几何属性变化（位置、尺寸），触发重新布局，开销大。重绘：元素外观变化（颜色、背景），不影响布局，开销较小。重排一定触发重绘，反之不然。减少方式：批量修改 DOM（DocumentFragment）、使用 `transform`/`opacity` 做动画（只触发合成层，不重排重绘）、避免频繁读取布局属性（`offsetWidth` 等会强制同步布局）
- **合成层（Composite Layer）是什么？什么属性会创建？** — 合成层是 GPU 加速渲染的独立图层。创建条件：`transform: translateZ(0)` / `translate3d`、`will-change: transform`、`opacity` 动画、`position: fixed`、`<video>`/`<canvas>`。好处：变化只需重新合成，不触发重排重绘。注意：过多合成层反而浪费内存（层爆炸），需要谨慎使用
- **`<script>` 标签的 async 和 defer 区别？** — 默认：阻塞 HTML 解析，下载并执行完才继续。`async`：异步下载，下载完立即执行（可能在 HTML 解析中间），执行顺序不确定。`defer`：异步下载，在 HTML 解析完成后、DOMContentLoaded 前按顺序执行。实践：第三方脚本（统计、广告）用 `async`；依赖 DOM 的脚本用 `defer`；模块脚本（`type="module"`）默认 defer 行为

### 6.2 网络基础

- **HTTP/1.1、HTTP/2、HTTP/3 的区别？** — HTTP/1.1：文本协议，队头阻塞（Head-of-Line Blocking），同一域名最多 6 个 TCP 连接。HTTP/2：二进制分帧，多路复用（一个连接多个请求）、头部压缩（HPACK）、服务端推送。HTTP/3：基于 QUIC（UDP），解决了 TCP 层的队头阻塞，连接迁移（网络切换不中断），0-RTT 连接建立
- **HTTPS 的原理？TLS 握手过程？** — HTTPS = HTTP + TLS 加密。TLS 握手：① Client Hello（支持的加密套件）；② Server Hello + 证书；③ 客户端验证证书（CA 信任链）；④ 密钥交换（RSA 或 ECDHE）生成对称密钥；⑤ 后续通信使用对称加密。现代默认 TLS 1.3，握手只需 1-RTT
- **强缓存和协商缓存的区别？** — 强缓存：`Cache-Control: max-age=31536000`、`Expires`（已废弃），命中时不发请求（200 from cache）。协商缓存：`ETag`/`If-None-Match`、`Last-Modified`/`If-Modified-Since`，发请求验证（304 Not Modified 或 200 新内容）。最佳实践：HTML 用协商缓存；静态资源用强缓存 + 文件名 hash
- **CDN 的原理？怎么用？** — Content Delivery Network，将内容分发到全球边缘节点，用户访问就近节点。原理：DNS 解析到最近的 CDN 节点 → 节点有缓存则直接返回 → 无缓存则回源获取并缓存。前端使用：静态资源（JS/CSS/图片）放 CDN，HTML 不放（需要实时更新）；配合强缓存 + hash 文件名

### 6.3 前端安全

- **XSS 是什么？怎么防御？** — Cross-Site Scripting，攻击者注入恶意脚本。类型：存储型（数据库中）、反射型（URL 参数）、DOM 型（纯前端）。防御：输出编码（HTML Entity 转义）；CSP（Content-Security-Policy）限制脚本来源；`HttpOnly` Cookie 防止 JS 读取；React/Vue 默认转义模板输出（但 `dangerouslySetInnerHTML` / `v-html` 绕过了保护）
- **CSRF 是什么？怎么防御？** — Cross-Site Request Forgery，攻击者伪造用户请求。原理：用户在 A 站已登录，B 站携带 A 站 Cookie 发起请求。防御：CSRF Token（服务端生成随机令牌）；SameSite Cookie（`SameSite=Strict/Lax`）；检查 `Origin`/`Referer` 头；重要操作二次确认
- **CSP 是什么？怎么配置？** — Content Security Policy，通过 HTTP 头或 meta 标签限制页面可加载的资源来源。配置示例：`Content-Security-Policy: default-src 'self'; script-src 'self' cdn.example.com; style-src 'self' 'unsafe-inline'`。可以防止 XSS、数据注入、点击劫持。`report-uri` 可以收集违规报告
- **同源策略和跨域解决方案？** — 同源策略：协议、域名、端口完全一致才算同源，限制跨源的 DOM 访问、Cookie 读取和 AJAX 请求。跨域方案：CORS（服务端设置 `Access-Control-Allow-Origin`，预检请求 OPTIONS）；代理（开发用 devServer proxy，生产用 Nginx 反向代理）；JSONP（仅 GET，已过时）；`postMessage`（iframe 通信）

---

## 7. 前端系统设计

### 7.1 组件库设计

- **如何设计一个组件库的 API？** — 原则：一致性（类似功能相同 API 命名）、最小化（必要的 props 暴露，其余用 slot/render props 扩展）、可组合（小组件组合成大组件）。具体实践：受控/非受控模式支持（`value` + `onChange` vs 内部状态）；样式隔离（CSS 变量暴露主题接口）；TypeScript 类型导出；ref 转发（`forwardRef` / `defineExpose`）
- **组件库的主题系统怎么设计？** — CSS 变量方案（主流）：定义全局 CSS Custom Properties（`--color-primary`），组件内引用变量，切换主题只需修改变量值。Design Token 体系：将设计规范（颜色、间距、字号、圆角）抽象为 Token，统一管理。暗色模式：`prefers-color-scheme` 媒体查询 + CSS 变量切换。注意支持运行时切换而非只靠编译
- **组件库如何做到可访问性（a11y）？** — 遵循 WAI-ARIA 规范：正确使用 ARIA 属性（role、aria-label、aria-expanded）；键盘导航（Tab 顺序、Enter/Space 触发、Escape 关闭）；焦点管理（Modal 打开时陷入焦点、关闭后恢复焦点）；颜色对比度（WCAG AA 标准，4.5:1）；屏幕阅读器测试
- **组件库怎么打包发布？** — 打包格式：同时输出 ESM（Tree Shaking）和 CJS（兼容老项目）。样式：CSS 独立文件（按需引入）或 CSS-in-JS。`package.json` 关键字段：`main`（CJS）、`module`（ESM）、`types`（类型）、`exports`（条件导出）、`sideEffects: false`（Tree Shaking 友好）。版本管理用 changesets

### 7.2 微前端架构

- **微前端是什么？什么时候需要？** — 将前端应用拆分为多个独立部署、独立开发的子应用，由主应用（基座）统一加载和路由。适用场景：遗留系统渐进式迁移（新功能用新框架，老页面不动）；多团队协作大型应用；不同子应用技术栈不统一。不适合：小型项目、单一团队、技术栈统一
- **qiankun 的原理？** — 基于 single-spa。核心机制：① HTML Entry（加载子应用 HTML，解析并运行其中的 JS/CSS）；② JS 沙箱（Proxy 代理 window 对象，子应用修改全局变量不影响主应用）；③ CSS 隔离（Shadow DOM 或 `scoped` 添加前缀）；④ 应用间通信（全局状态 `initGlobalState`、CustomEvent）
- **Module Federation 是什么？** — Webpack 5 的模块联邦，允许多个独立构建的应用在运行时共享模块。核心概念：Host（消费者）和 Remote（提供者），`exposes` 暴露模块，`remotes` 引用远程模块。优势：真正的运行时代码共享（非 npm 包方式），独立部署和更新，依赖共享（避免重复加载 React 等公共库）
- **微前端的 CSS 隔离和 JS 沙箱怎么实现？** — CSS 隔离：Shadow DOM（严格隔离但部分库不兼容）、CSS Modules/Scoped（编译时加前缀）、动态添加样式 scope 属性。JS 沙箱：Proxy 沙箱（代理 window，快照恢复）、iframe 沙箱（天然隔离但通信复杂）、with + Proxy（轻量方案）。权衡：隔离越严格，子应用与主应用通信越困难

### 7.3 前端监控

- **前端错误监控怎么做？** — JS 错误：`window.onerror`（同步错误）、`window.addEventListener('unhandledrejection')`（Promise 错误）、`ErrorBoundary`（React 组件错误）。资源加载错误：`window.addEventListener('error', ..., true)`（捕获阶段）。上报策略：`navigator.sendBeacon`（页面卸载时上报）、`fetch` + 批量上报。Source Map 反解析定位原始代码位置
- **性能监控关注哪些指标？** — 加载指标：FCP（First Contentful Paint）、LCP、TTFB（Time To First Byte）。交互指标：INP、TBT（Total Blocking Time）、Long Tasks（>50ms）。稳定性指标：CLS。采集方式：Performance API（`performance.getEntries()`）、PerformanceObserver、Web Vitals 库。上报到自建平台或 Sentry/Datadog
- **用户行为追踪（埋点）有哪些方案？** — 代码埋点：手动在事件中上报，精准但维护成本高。无痕埋点（全埋点）：自动捕获所有点击/浏览事件，数据量大但分析灵活。可视化埋点：通过圈选页面元素生成配置，非技术人员可操作。上报方式：1×1 GIF（简单、跨域）、`sendBeacon`（可靠）、批量队列。注意隐私合规（GDPR、个保法）
- **Sentry 的工作原理？** — 客户端 SDK 全局捕获错误（重写 `window.onerror`、包装 `fetch`/`XMLHttpRequest`）→ 收集堆栈信息、面包屑（Breadcrumbs，用户操作轨迹）、设备/浏览器信息 → 上报到 Sentry 服务端 → Source Map 解析还原代码位置 → 聚合相同错误为 Issue → 告警通知。性能监控通过 `Performance` API 采集 Transactions 和 Spans

### 7.4 跨端开发

- **跨端开发中，JSBridge 的实现原理是什么？** — JSBridge 是 Web 与 Native（iOS/Android）的双向通信桥梁。原理：JS → Native：URL Scheme 拦截（`iframe.src = 'myapp://method?params'`）或注入全局对象（`window.webkit.messageHandlers`）；Native → JS：直接执行 `webView.evaluateJavaScript()`。通信协议：约定方法名 + 回调 ID + 参数的 JSON 格式。安全考虑：白名单校验、参数校验、防止恶意调用
- **React Native、Flutter 和 H5 跨端方案怎么选？** — React Native：JS 驱动原生组件，接近原生性能，React 生态复用；缺点是桥接开销、样式差异。Flutter：Dart 语言 + 自绘引擎（Skia），UI 一致性最高，性能接近原生；缺点是生态相对小、包体积大、Dart 学习成本。H5（WebView）：开发成本最低，跨平台能力最强；性能和体验最差。选型维度：性能要求、团队技术栈、迭代速度、原生能力依赖程度
- **在跨端框架中，一个组件从代码到屏幕显示的渲染链路是怎样的？** — React Native：JSX → React Reconciler 生成虚拟 DOM → Bridge 序列化为 JSON → Native 端反序列化为原生 View → 原生渲染引擎绘制。Flutter：Dart Widget → Element Tree → RenderObject Tree → Skia 引擎直接绘制到 Canvas。关键差异：RN 依赖原生组件（性能受桥接制约），Flutter 自绘（性能好但无法复用原生组件）
- **跨端应用中，如何实现一套代码适配手机、平板和折叠屏？** — 响应式布局策略：断点系统（根据屏幕宽度切换布局模式）；弹性布局（Flexbox + 百分比 + 最大/最小宽度约束）；组件级适配（同一组件在不同尺寸下显示不同变体）。折叠屏特殊处理：监听屏幕折叠状态变化、适配连续性模式（Continuity）、处理分屏场景。实践：用 CSS Container Query 或 JS `matchMedia` 监听断点
- **离线化方案和热更新机制怎么设计？** — 离线化：Service Worker 缓存静态资源 + IndexedDB 缓存数据；预加载关键页面；离线时显示缓存内容 + 网络恢复后同步。热更新（针对 RN/Hybrid）：JS Bundle 差量更新（CodePush/自建方案）；版本管理（强制更新 vs 静默更新）；回滚机制（新版本异常时自动回退）；安全校验（签名验证防篡改）

---

## 8. Vibe Coding 与 AI 工具

### 8.1 AI 编程工具使用

- **你用过哪些 AI 编程工具？各有什么特点？** — GitHub Copilot：VS Code 集成最成熟，行内补全体验好，Chat 模式支持代码解释和重构。Cursor：AI-native 编辑器，Composer 模式可跨文件编辑，Tab 补全 + Chat + Agent 三种模式。Claude Code：终端 Agent，擅长大范围代码修改和项目级任务，自主规划和执行能力强。Windsurf：类 Cursor 的 AI 编辑器，Cascade 模式自动执行多步操作。v0：Vercel 出品，自然语言生成 React 组件和页面，适合快速原型
- **AI 编程工具的核心原理是什么？** — 上下文感知：收集当前文件、打开的标签页、项目结构、Git 历史等信息构建上下文（Context Engineering）。代码补全：模型基于上下文预测下一段代码。Chat/Agent 模式：理解自然语言意图 → 检索相关代码 → 生成修改方案 → 执行修改。关键能力差异：上下文窗口大小、工具调用能力（读写文件、运行命令）、模型能力
- **如何写好 AI 编程的 Prompt？** — 明确任务目标（"实现一个带虚拟滚动的 Select 组件"而非"做个下拉框"）；提供上下文（现有代码、项目技术栈、设计规范）；指定约束（"使用 React + TypeScript"、"不要引入新依赖"）；分步引导（复杂任务拆为小步骤）；给出示例（输入/输出示例帮助模型理解期望格式）
- **AI 编程的局限性？** — 复杂业务逻辑理解不足（需要领域知识的场景）；容易生成"看似正确"的代码（需要人工 Review）；对运行时行为理解有限（性能、内存、并发问题）；长上下文管理困难（跨文件大范围修改时容易遗漏）；生成代码的安全性需要审查（可能引入 XSS、注入等漏洞）

### 8.2 开放性问题

> 以下问题没有标准答案，考察候选人对 AI 工具的真实使用深度、独立思考能力和行业认知。面试官可根据候选人的回答自由追问。涵盖 Vibe Coding 实践、AI 辅助开发、行业认知等方向。

**Vibe Coding 与 AI 辅助开发**

- **Vibe Coding 的工作流是什么？你实际用过吗？** — ① 描述需求（自然语言）→ ② AI 生成代码 → ③ 预览/运行 → ④ 反馈修正 → ⑤ 迭代直到满意。关键技巧：从简单开始逐步复杂；及时提交版本；对关键逻辑手动验证。追问：你有没有尝试过完全用 AI 从零完成一个项目？哪些环节顺利哪些卡住了？
- **Vibe Coding 可能产生什么风险？** — 代码质量不可控；安全漏洞（AI 不主动考虑安全性）；可维护性差（开发者可能不理解生成的代码）；技术债务累积。追问：你觉得 Vibe Coding 适合在什么场景下用？生产环境呢？
- **AI 工具在前端开发中最适合和最不适合哪些任务？** — 高效：组件骨架生成、样式实现、测试用例生成、代码重构、工具函数。效果差：复杂状态管理设计、性能优化（需实测）、复杂交互动画、架构决策、安全敏感代码。追问：能举一个 AI 帮你提效最明显的具体例子吗？有没有 AI 反而帮倒忙的经历？
- **如何用 AI 做前端代码审查？** — 提供完整上下文（PR diff + 相关文件）；明确审查重点。AI 擅长：语法错误、类型不匹配、边界条件、命名不一致、潜在 XSS。AI 不擅长：业务逻辑正确性、UX 判断、架构合理性。最佳方式：AI 初筛 + 人工终审
- **v0、bolt.new 这类 AI 前端生成工具你用过吗？怎么评价？** — v0：自然语言生成 React + Tailwind 组件，适合 UI 原型。bolt.new：浏览器内完整开发环境，AI 生成全栈应用。追问：你觉得这类工具会取代前端开发吗？它们的局限在哪？

**行业认知与个人思考**

- **你平时是怎么使用 AI 的？能举一个具体的例子吗？** — 考察实际使用深度而非工具罗列。好的回答包含：具体场景、遇到的问题和解决方式、效率提升的量化感受。追问：AI 帮你解决过最复杂的问题是什么？
- **你怎么看 AI 对前端开发的影响？** — 考察独立思考，而非复述套话。追问：哪些前端工作你觉得 3 年内会被 AI 大幅改变？哪些不会？作为前端开发者你在做什么准备？
- **你觉得 AI 时代前端开发者最重要的能力是什么？** — 好的回答通常涉及：需求理解和产品思维、审查和调试能力、系统设计能力、持续学习能力
- **如果一个初学者问你"要不要学前端，还是直接用 AI 写"，你怎么回答？** — 考察对"基础功"和"工具效率"的辩证理解。追问：你觉得学 JavaScript 原理还有必要吗？
- **你觉得前端面试还考八股文有意义吗？** — 考察思辨能力。追问：AI 时代面试应该怎么考？如果你是面试官，你会怎么评估一个前端候选人？
- **聊聊你在过去一年中遇到的最复杂的技术难题，你是如何定位并解决的？** — 开放性项目复盘题。考察问题定位能力、技术深度和解决问题的方法论。好的回答包含：问题背景、排查过程、尝试过的方案、最终解决方案和复盘总结
- **你对 WebAssembly (WASM) 在前端 AI 领域的应用前景怎么看？** — 考察技术视野。参考方向：WASM 可以在浏览器中运行推理模型（如 ONNX Runtime Web）、加速 Token 计算、离线 AI 能力。当前局限：模型体积大、初始化慢、GPU 访问有限（WebGPU 正在改善）。追问：你觉得未来浏览器端跑 AI 模型会成为主流吗？

---

## 9. 前端 × AI 产品实战

> 当候选人有 AI 产品（聊天应用、Agent 平台、AI 编辑器等）的前端开发经验时，可从本节选题。这些问题考察的是**用前端技术解决 AI 产品特有问题**的能力，而非 AI 算法本身。

### 9.1 AI 场景中的通信与数据流

- **在 AI 聊天场景中，SSE 和 WebSocket 有什么区别？怎么选？** — SSE（Server-Sent Events）：单向通信（服务端→客户端），基于 HTTP，自动重连，轻量。WebSocket：双向通信，独立协议（ws://），需要心跳维持。AI 聊天场景大多选 SSE：LLM 生成是单向流式输出，SSE 天然匹配；实现简单（原生 `EventSource` API）；兼容 HTTP 基础设施（负载均衡、CDN）。WebSocket 适合需要客户端持续发送消息的场景（如实时协作编辑、多人对话）
- **前端如何解析并渲染 AI 返回的 Markdown 流式数据？** — 挑战：数据是逐 Token 到达的，Markdown 语法可能跨多个 chunk（如代码块的 ``` 分隔符）。方案：增量解析（每次新 chunk 到达时追加并重新解析尾部）、使用流式 Markdown 解析器（如 marked 的 streaming 模式）、代码块高亮（highlight.js / Prism 在代码块完整后触发）。注意：避免每次全量重新渲染（性能灾难），应只更新最后一个正在生成的消息节点
- **如何实现一个前端并发请求控制器，限制同时进行的 AI 模型调用数量？** — 场景：批量翻译、多轮对话预加载等场景需要限制并发。实现：维护一个请求队列 + 当前活跃数计数器，新请求入队，活跃数 < 上限时取出执行，完成后递减并取下一个。核心代码用 Promise + 队列实现。进阶：支持优先级队列、取消机制（AbortController）、超时处理、重试策略
- **AI 交互中，前端如何处理长文本输入的 Token 计数与截断？** — 前端需要在用户输入时实时估算 Token 数量，避免超出模型上下文窗口。方案：使用 tiktoken（OpenAI 的分词库，有 JS 版本 js-tiktoken）做精确计算；或用近似公式（中文约 1-2 字/Token，英文约 4 字符/Token）做快速估算。UI 反馈：显示 Token 计数、接近上限时警告、自动截断策略（保留最近的对话轮次，摘要压缩早期历史）

### 9.2 AI 产品前端性能

- **AI 聊天界面的消息列表通常很长，如何优化渲染性能？** — 核心方案：虚拟滚动（Virtual Scrolling）——只渲染可视区域内的 DOM 节点。实现：react-virtuoso、@tanstack/virtual、vue-virtual-scroller。难点：AI 消息高度不固定（Markdown 渲染后高度不可预知），需要动态测量（ResizeObserver）+ 预估高度 + 滚动位置修正。其他优化：消息组件 memo 化、流式消息单独更新（不触发整个列表重渲染）、滚动到底部的自动跟随逻辑
- **AI 聊天场景下，长文本的流式渲染会导致页面频繁重排（Reflow），有哪些优化手段？** — 问题根源：每次 Token 到达 append 文本会触发 Layout 重计算。优化方案：① 使用 `requestAnimationFrame` 合并多个 Token 的 DOM 更新（减少重排次数）；② 流式消息区域用 `contain: content` 或 `will-change` 隔离为独立合成层（不影响其他元素）；③ 固定消息容器高度，只在当前消息内部重排；④ Markdown 增量渲染（只解析新增部分，不全量重新渲染）；⑤ 复杂元素（代码块、表格）延迟渲染，等流式完成后再触发高亮和格式化
- **CSS 中如何实现 AI 流式输出的打字机效果？** — 方案一：纯 CSS `@keyframes` + `steps()` + `overflow: hidden` + `white-space: nowrap`（适合固定文本）。方案二：JS 逐字添加文本节点 + CSS `animation` 光标闪烁（适合动态流式内容）。方案三：结合流式 SSE 数据，每个 Token 到达时 append 到 DOM，光标用 `::after` 伪元素 + `blink` 动画。注意：逐字渲染要避免频繁触发重排，用 `requestAnimationFrame` 批量更新
- **如何衡量 AI 聊天应用的前端性能？除了 Core Web Vitals 还有哪些特有指标？** — AI 特有指标：TTFT（Time To First Token，从发送请求到第一个 Token 渲染的时间）；TPS（Tokens Per Second，流式渲染速率，影响用户感知的"打字速度"）；消息渲染完成时间（从最后一个 Token 到达到 Markdown 渲染完毕）；输入响应延迟（用户发送消息到 UI 反馈的时间）。通用指标仍然重要：内存占用（长对话场景 DOM 节点堆积）、滚动流畅度（FPS）

### 9.3 AI 平台前端架构

- **如果让你从零设计一个 AI 平台（如 AI 聊天、Agent 编排）的前端架构，你会考虑哪些核心模块？** — 核心模块：① 对话引擎（消息管理、流式渲染、多轮上下文）；② 编辑器系统（Prompt 编辑器、代码编辑器 Monaco/CodeMirror）；③ 可视化引擎（Agent 流程图、DAG 编排，基于 ReactFlow/X6）；④ 实时通信层（SSE/WebSocket 管理、重连机制、消息队列）；⑤ 状态管理（对话状态、模型配置、用户设置）；⑥ 插件系统（工具面板、自定义组件扩展）
- **开发 AI Agent 编排工具时，如何实现高性能的连线引擎（如流程图）？** — 技术选型：Canvas（高性能、大量节点）vs SVG（交互友好、CSS 样式）vs 混合方案（节点用 DOM/SVG、连线用 Canvas）。主流库：ReactFlow（React 生态主流）、AntV X6（蚂蚁出品，功能丰富）、xyflow。性能关键：视口裁剪（只渲染可视区域的节点和边）；层级缓存（静态层用 Canvas 缓存，交互层单独渲染）；拖拽优化（`transform` 替代 `top/left`、`will-change` 提升合成层）
- **前端如何防护 Prompt Injection（提示词注入）？** — 前端能做的：用户输入过滤（检测常见注入模式如 "ignore previous instructions"，但不可靠，攻击者总能绕过）；输入长度限制；角色标记（明确区分系统 Prompt 和用户输入的边界）；敏感操作二次确认（AI 建议执行危险操作时弹窗确认）。关键认知：前端防护是辅助手段，核心防线在服务端（Prompt 隔离、沙箱执行、权限最小化）
- **谈谈 RAG（检索增强生成）在前端侧可以做的优化和实践？** — 前端可做的事：① 查询优化——对用户输入做预处理（去除冗余、提取关键词、历史上下文压缩）再发送给后端检索；② 结果展示——引用溯源 UI（高亮引用片段、展示来源文档链接、置信度标识）；③ 前端缓存——相似查询命中缓存，减少重复检索；④ 分级加载——先展示检索到的原文片段，再展示 LLM 生成的总结回答；⑤ 反馈闭环——用户标记"答案不准确"时记录反馈，用于优化检索策略
- **团队协作中，后端 AI 接口定义与前端需求不符（如字段结构复杂、流式协议不统一），你如何处理？** — 实际工作中非常常见的问题。应对方案：① 适配层（Adapter Pattern）——封装 API Client，将后端数据结构转换为前端友好的格式；② 制定前后端协议规范（流式数据格式、错误码定义、分页/流式切换策略）；③ Mock Server 先行——用 MSW 或自建 Mock 跑通前端流程，减少联调等待；④ 推动 API 设计评审，前端参与接口定义阶段
