# 前端手写题集

> 面试最后环节可选的编码题。难度从简单到中等，覆盖数据结构处理、DOM 操作、异步编程、函数式编程等方向。
>
> 面试官可根据候选人水平和剩余时间灵活选题，每题标注了难度和预估用时。

---

## 目录

- [1. 数据结构与转换](#1-数据结构与转换)
  - [1.1 扁平数组转树形结构](#11-扁平数组转树形结构)
  - [1.2 树形结构转扁平数组](#12-树形结构转扁平数组)
  - [1.3 对象深拷贝](#13-对象深拷贝)
  - [1.4 数组扁平化（flat）](#14-数组扁平化flat)
  - [1.5 数组去重](#15-数组去重)
- [2. 函数与闭包](#2-函数与闭包)
  - [2.1 防抖（debounce）](#21-防抖debounce)
  - [2.2 节流（throttle）](#22-节流throttle)
  - [2.3 柯里化（curry）](#23-柯里化curry)
  - [2.4 实现 compose 函数](#24-实现-compose-函数)
- [3. 异步编程](#3-异步编程)
  - [3.1 实现 Promise.all](#31-实现-promiseall)
  - [3.2 实现 Promise.race](#32-实现-promiserace)
  - [3.3 并发控制（限制并发数）](#33-并发控制限制并发数)
  - [3.4 实现 sleep 函数](#34-实现-sleep-函数)
- [4. 原型与类](#4-原型与类)
  - [4.1 实现 instanceof](#41-实现-instanceof)
  - [4.2 实现 new 操作符](#42-实现-new-操作符)
  - [4.3 实现 call / apply / bind](#43-实现-call--apply--bind)
- [5. DOM 与事件](#5-dom-与事件)
  - [5.1 事件委托](#51-事件委托)
  - [5.2 虚拟 DOM diff](#52-虚拟-dom-diff)
- [6. 算法（LeetCode 简单～中等偏简单）](#6-算法leetcode-简单中等偏简单)
  - [6.1 两数之和](#61-两数之和)
  - [6.2 有效的括号](#62-有效的括号)
  - [6.3 合并两个有序数组](#63-合并两个有序数组)
  - [6.4 反转链表](#64-反转链表)
  - [6.5 爬楼梯](#65-爬楼梯)
  - [6.6 买卖股票的最佳时机](#66-买卖股票的最佳时机)

---

## 1. 数据结构与转换

### 1.1 扁平数组转树形结构

**难度**：简单 | **预估用时**：5-10 分钟 | **高频指数**：⭐⭐⭐⭐⭐

**题目**：将扁平的部门列表转为树形结构。

```ts
interface TreeNode {
  id: number;
  name: string;
  parentId: number | null;
  children: TreeNode[];
}

const list = [
  { id: 1, name: '总部', parentId: null },
  { id: 2, name: '华东区', parentId: 1 },
  { id: 3, name: '华南区', parentId: 1 },
  { id: 4, name: '上海', parentId: 2 },
  { id: 5, name: '杭州', parentId: 2 },
  { id: 6, name: '广州', parentId: 3 },
];

// 输出：树形结构（每个节点有 children 字段）
// [
//   {
//     id: 1, name: '总部', parentId: null,
//     children: [
//       { id: 2, name: '华东区', parentId: 1, children: [
//         { id: 4, name: '上海', parentId: 2, children: [] },
//         { id: 5, name: '杭州', parentId: 2, children: [] }
//       ]},
//       { id: 3, name: '华南区', parentId: 1, children: [
//         { id: 6, name: '广州', parentId: 3, children: [] }
//       ]}
//     ]
//   }
// ]
```

**参考答案**：

```ts
// 方法一：Map + 一次遍历，O(n) 时间复杂度（推荐）
function listToTree(list: { id: number; name: string; parentId: number | null }[]): TreeNode[] {
  const map = new Map<number, TreeNode>();
  const roots: TreeNode[] = [];

  // 先为每个节点创建带 children 的对象并存入 Map
  for (const item of list) {
    map.set(item.id, { ...item, children: [] });
  }

  // 再遍历一次，把每个节点挂到其父节点的 children 下
  for (const item of list) {
    const node = map.get(item.id)!;
    if (item.parentId === null) {
      roots.push(node);
    } else {
      map.get(item.parentId)?.children.push(node);
    }
  }

  return roots;
}

// 方法二：递归（直观但 O(n²)）
function listToTreeRecursive(
  list: { id: number; name: string; parentId: number | null }[],
  parentId: number | null = null
): TreeNode[] {
  return list
    .filter(item => item.parentId === parentId)
    .map(item => ({
      ...item,
      children: listToTreeRecursive(list, item.id),
    }));
}
```

**考察点**：Map 的使用、引用关系理解、时间复杂度分析

**追问方向**：
- 方法一为什么是 O(n)？（Map 查找 O(1)，两次遍历 O(n)）
- 如果数据量很大（10万+），两种方法性能差多少？
- 如果数据是无序的（子节点可能出现在父节点前面），方法一还能正常工作吗？（可以，因为先全部建好 Map 再挂载）

---

### 1.2 树形结构转扁平数组

**难度**：简单 | **预估用时**：5 分钟 | **高频指数**：⭐⭐⭐

**题目**：将树形结构展开为扁平数组。

```ts
function treeToList(tree: TreeNode[]): Omit<TreeNode, 'children'>[] {
  const result: Omit<TreeNode, 'children'>[] = [];

  function traverse(nodes: TreeNode[]) {
    for (const node of nodes) {
      const { children, ...rest } = node;
      result.push(rest);
      if (children.length > 0) {
        traverse(children);
      }
    }
  }

  traverse(tree);
  return result;
}
```

---

### 1.3 对象深拷贝

**难度**：中等 | **预估用时**：10-15 分钟 | **高频指数**：⭐⭐⭐⭐⭐

**题目**：实现一个深拷贝函数，处理对象、数组、循环引用。

```ts
function deepClone<T>(obj: T, cache = new WeakMap()): T {
  // 基本类型直接返回
  if (obj === null || typeof obj !== 'object') return obj;

  // 处理循环引用
  if (cache.has(obj as object)) return cache.get(obj as object);

  // 处理 Date
  if (obj instanceof Date) return new Date(obj.getTime()) as T;

  // 处理 RegExp
  if (obj instanceof RegExp) return new RegExp(obj.source, obj.flags) as T;

  // 处理数组和对象
  const clone = (Array.isArray(obj) ? [] : {}) as T;
  cache.set(obj as object, clone);

  for (const key of Reflect.ownKeys(obj as object)) {
    (clone as any)[key] = deepClone((obj as any)[key], cache);
  }

  return clone;
}
```

**追问方向**：
- 为什么用 WeakMap 而不是 Map？（弱引用，不阻止 GC 回收）
- `structuredClone` 了解吗？它和手写深拷贝有什么区别？
- `JSON.parse(JSON.stringify())` 有什么局限？（不支持函数、undefined、循环引用、Date 变字符串、RegExp 变空对象）

---

### 1.4 数组扁平化（flat）

**难度**：简单 | **预估用时**：5 分钟 | **高频指数**：⭐⭐⭐

```ts
// 递归实现
function flat(arr: any[], depth = Infinity): any[] {
  return depth > 0
    ? arr.reduce((acc, val) =>
        acc.concat(Array.isArray(val) ? flat(val, depth - 1) : val), [])
    : arr.slice();
}

// 迭代实现（用栈）
function flatIterative(arr: any[]): any[] {
  const stack = [...arr];
  const result: any[] = [];
  while (stack.length) {
    const item = stack.pop()!;
    if (Array.isArray(item)) {
      stack.push(...item);
    } else {
      result.unshift(item);
    }
  }
  return result;
}
```

---

### 1.5 数组去重

**难度**：简单 | **预估用时**：3 分钟 | **高频指数**：⭐⭐⭐

```ts
// 方法一：Set
const unique = (arr: any[]) => [...new Set(arr)];

// 方法二：filter + indexOf
const unique2 = (arr: any[]) => arr.filter((item, index) => arr.indexOf(item) === index);

// 方法三：对象去重（处理引用类型需要序列化）
const unique3 = (arr: any[]) => {
  const map = new Map();
  return arr.filter(item => {
    const key = typeof item === 'object' ? JSON.stringify(item) : item;
    if (map.has(key)) return false;
    map.set(key, true);
    return true;
  });
};
```

---

## 2. 函数与闭包

### 2.1 防抖（debounce）

**难度**：简单 | **预估用时**：5 分钟 | **高频指数**：⭐⭐⭐⭐⭐

```ts
function debounce<T extends (...args: any[]) => any>(fn: T, delay: number) {
  let timer: ReturnType<typeof setTimeout> | null = null;

  return function (this: any, ...args: Parameters<T>) {
    if (timer) clearTimeout(timer);
    timer = setTimeout(() => {
      fn.apply(this, args);
      timer = null;
    }, delay);
  };
}
```

**追问**：如何实现 `leading` 模式（首次立即执行）？

---

### 2.2 节流（throttle）

**难度**：简单 | **预估用时**：5 分钟 | **高频指数**：⭐⭐⭐⭐⭐

```ts
function throttle<T extends (...args: any[]) => any>(fn: T, interval: number) {
  let lastTime = 0;

  return function (this: any, ...args: Parameters<T>) {
    const now = Date.now();
    if (now - lastTime >= interval) {
      lastTime = now;
      fn.apply(this, args);
    }
  };
}
```

---

### 2.3 柯里化（curry）

**难度**：中等 | **预估用时**：8 分钟 | **高频指数**：⭐⭐⭐

```ts
function curry(fn: Function) {
  return function curried(this: any, ...args: any[]): any {
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    }
    return (...moreArgs: any[]) => curried.apply(this, [...args, ...moreArgs]);
  };
}

// 使用
const add = (a: number, b: number, c: number) => a + b + c;
const curriedAdd = curry(add);
curriedAdd(1)(2)(3);   // 6
curriedAdd(1, 2)(3);   // 6
```

---

### 2.4 实现 compose 函数

**难度**：中等 | **预估用时**：5 分钟 | **高频指数**：⭐⭐⭐

```ts
function compose(...fns: Function[]) {
  return (input: any) => fns.reduceRight((acc, fn) => fn(acc), input);
}

// pipe（从左到右）
function pipe(...fns: Function[]) {
  return (input: any) => fns.reduce((acc, fn) => fn(acc), input);
}
```

---

## 3. 异步编程

### 3.1 实现 Promise.all

**难度**：中等 | **预估用时**：8-10 分钟 | **高频指数**：⭐⭐⭐⭐⭐

```ts
function promiseAll<T>(promises: Promise<T>[]): Promise<T[]> {
  return new Promise((resolve, reject) => {
    if (promises.length === 0) return resolve([]);

    const results: T[] = new Array(promises.length);
    let completed = 0;

    promises.forEach((promise, index) => {
      Promise.resolve(promise).then(
        (value) => {
          results[index] = value;
          completed++;
          if (completed === promises.length) {
            resolve(results);
          }
        },
        (reason) => {
          reject(reason);
        }
      );
    });
  });
}
```

**追问**：为什么用 `completed` 计数器而不是 `results.length`？（Promise 异步完成顺序不确定，results[index] 赋值可能跳跃，length 不可靠）

---

### 3.2 实现 Promise.race

**难度**：简单 | **预估用时**：3 分钟 | **高频指数**：⭐⭐⭐

```ts
function promiseRace<T>(promises: Promise<T>[]): Promise<T> {
  return new Promise((resolve, reject) => {
    promises.forEach(promise => {
      Promise.resolve(promise).then(resolve, reject);
    });
  });
}
```

---

### 3.3 并发控制（限制并发数）

**难度**：中等 | **预估用时**：10-15 分钟 | **高频指数**：⭐⭐⭐⭐

```ts
async function asyncPool<T>(
  limit: number,
  items: T[],
  fn: (item: T) => Promise<any>
): Promise<any[]> {
  const results: Promise<any>[] = [];
  const executing: Promise<any>[] = [];

  for (const item of items) {
    const p = fn(item);
    results.push(p);

    if (items.length >= limit) {
      const e = p.then(() => {
        executing.splice(executing.indexOf(e), 1);
      });
      executing.push(e);

      if (executing.length >= limit) {
        await Promise.race(executing);
      }
    }
  }

  return Promise.all(results);
}
```

---

### 3.4 实现 sleep 函数

**难度**：简单 | **预估用时**：2 分钟 | **高频指数**：⭐⭐

```ts
function sleep(ms: number): Promise<void> {
  return new Promise(resolve => setTimeout(resolve, ms));
}

// 使用
async function demo() {
  console.log('start');
  await sleep(1000);
  console.log('end'); // 1秒后
}
```

---

## 4. 原型与类

### 4.1 实现 instanceof

**难度**：简单 | **预估用时**：5 分钟 | **高频指数**：⭐⭐⭐⭐

```ts
function myInstanceof(left: any, right: Function): boolean {
  let proto = Object.getPrototypeOf(left);
  const prototype = right.prototype;

  while (proto !== null) {
    if (proto === prototype) return true;
    proto = Object.getPrototypeOf(proto);
  }

  return false;
}
```

---

### 4.2 实现 new 操作符

**难度**：中等 | **预估用时**：5 分钟 | **高频指数**：⭐⭐⭐⭐

```ts
function myNew(constructor: Function, ...args: any[]): any {
  // 1. 创建空对象，原型指向构造函数的 prototype
  const obj = Object.create(constructor.prototype);
  // 2. 执行构造函数，绑定 this
  const result = constructor.apply(obj, args);
  // 3. 如果构造函数返回对象则用返回值，否则用新对象
  return result instanceof Object ? result : obj;
}
```

---

### 4.3 实现 call / apply / bind

**难度**：中等 | **预估用时**：8 分钟 | **高频指数**：⭐⭐⭐⭐

```ts
// call
Function.prototype.myCall = function (context: any, ...args: any[]) {
  context = context ?? globalThis;
  const key = Symbol();
  context[key] = this;
  const result = context[key](...args);
  delete context[key];
  return result;
};

// apply
Function.prototype.myApply = function (context: any, args: any[] = []) {
  context = context ?? globalThis;
  const key = Symbol();
  context[key] = this;
  const result = context[key](...args);
  delete context[key];
  return result;
};

// bind
Function.prototype.myBind = function (context: any, ...outerArgs: any[]) {
  const fn = this;
  return function boundFn(this: any, ...innerArgs: any[]) {
    // 作为构造函数调用时，this 指向新实例
    if (this instanceof boundFn) {
      return new (fn as any)(...outerArgs, ...innerArgs);
    }
    return fn.apply(context, [...outerArgs, ...innerArgs]);
  };
};
```

---

## 5. DOM 与事件

### 5.1 事件委托

**难度**：简单 | **预估用时**：5 分钟 | **高频指数**：⭐⭐⭐

```ts
function delegate(
  parent: HTMLElement,
  selector: string,
  event: string,
  handler: (e: Event, target: HTMLElement) => void
) {
  parent.addEventListener(event, (e: Event) => {
    let target = e.target as HTMLElement;
    while (target && target !== parent) {
      if (target.matches(selector)) {
        handler(e, target);
        return;
      }
      target = target.parentElement!;
    }
  });
}
```

---

### 5.2 虚拟 DOM diff（简化版）

**难度**：中等偏难 | **预估用时**：15 分钟 | **高频指数**：⭐⭐⭐

```ts
interface VNode {
  tag: string;
  props: Record<string, any>;
  children: (VNode | string)[];
}

function diff(oldTree: VNode, newTree: VNode): any[] {
  const patches: any[] = [];

  // tag 不同，直接替换
  if (oldTree.tag !== newTree.tag) {
    patches.push({ type: 'REPLACE', node: newTree });
    return patches;
  }

  // 比较 props
  const propsPatches: any = {};
  for (const key in { ...oldTree.props, ...newTree.props }) {
    if (oldTree.props[key] !== newTree.props[key]) {
      propsPatches[key] = newTree.props[key] ?? null;
    }
  }
  if (Object.keys(propsPatches).length > 0) {
    patches.push({ type: 'PROPS', props: propsPatches });
  }

  // 比较 children（简化版，按索引对比）
  const maxLen = Math.max(oldTree.children.length, newTree.children.length);
  for (let i = 0; i < maxLen; i++) {
    if (!oldTree.children[i]) {
      patches.push({ type: 'ADD', node: newTree.children[i] });
    } else if (!newTree.children[i]) {
      patches.push({ type: 'REMOVE', index: i });
    } else if (typeof oldTree.children[i] === 'string' || typeof newTree.children[i] === 'string') {
      if (oldTree.children[i] !== newTree.children[i]) {
        patches.push({ type: 'TEXT', text: newTree.children[i] });
      }
    } else {
      patches.push(...diff(oldTree.children[i] as VNode, newTree.children[i] as VNode));
    }
  }

  return patches;
}
```

---

## 6. 算法（LeetCode 简单～中等偏简单）

### 6.1 两数之和

**难度**：简单 | LeetCode #1 | **高频指数**：⭐⭐⭐⭐⭐

```ts
function twoSum(nums: number[], target: number): number[] {
  const map = new Map<number, number>();
  for (let i = 0; i < nums.length; i++) {
    const complement = target - nums[i];
    if (map.has(complement)) {
      return [map.get(complement)!, i];
    }
    map.set(nums[i], i);
  }
  return [];
}
```

---

### 6.2 有效的括号

**难度**：简单 | LeetCode #20 | **高频指数**：⭐⭐⭐⭐

```ts
function isValid(s: string): boolean {
  const stack: string[] = [];
  const pairs: Record<string, string> = { ')': '(', ']': '[', '}': '{' };

  for (const ch of s) {
    if ('([{'.includes(ch)) {
      stack.push(ch);
    } else {
      if (stack.pop() !== pairs[ch]) return false;
    }
  }

  return stack.length === 0;
}
```

---

### 6.3 合并两个有序数组

**难度**：简单 | LeetCode #88 | **高频指数**：⭐⭐⭐

```ts
function merge(nums1: number[], m: number, nums2: number[], n: number): void {
  let i = m - 1, j = n - 1, k = m + n - 1;
  while (j >= 0) {
    if (i >= 0 && nums1[i] > nums2[j]) {
      nums1[k--] = nums1[i--];
    } else {
      nums1[k--] = nums2[j--];
    }
  }
}
```

---

### 6.4 反转链表

**难度**：简单 | LeetCode #206 | **高频指数**：⭐⭐⭐⭐⭐

```ts
class ListNode {
  val: number;
  next: ListNode | null;
  constructor(val: number, next: ListNode | null = null) {
    this.val = val;
    this.next = next;
  }
}

function reverseList(head: ListNode | null): ListNode | null {
  let prev: ListNode | null = null;
  let curr = head;

  while (curr) {
    const next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
  }

  return prev;
}
```

---

### 6.5 爬楼梯

**难度**：简单 | LeetCode #70 | **高频指数**：⭐⭐⭐⭐

```ts
function climbStairs(n: number): number {
  if (n <= 2) return n;
  let prev1 = 1, prev2 = 2;
  for (let i = 3; i <= n; i++) {
    const curr = prev1 + prev2;
    prev1 = prev2;
    prev2 = curr;
  }
  return prev2;
}
```

---

### 6.6 买卖股票的最佳时机

**难度**：简单 | LeetCode #121 | **高频指数**：⭐⭐⭐⭐

```ts
function maxProfit(prices: number[]): number {
  let minPrice = Infinity;
  let maxProfit = 0;

  for (const price of prices) {
    minPrice = Math.min(minPrice, price);
    maxProfit = Math.max(maxProfit, price - minPrice);
  }

  return maxProfit;
}
```
