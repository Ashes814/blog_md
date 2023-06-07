# 【GIS开发日记】第15期：JavaScript基础知识

> *分享GIS开发面经八股，GIS基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧*
> 

# [JavaScript中的值类型和引用类型的区别是什么？](https://juejin.cn/post/6844904198484541454)

- `值类型：` 每个变量都会存储各自的值，不会相互影响。主要指基本类型即`number`，`string`，`boolean`，`undefined`，`null`，`symbol` （栈内存中的数据就是所持有的值）
- `值类型`的数据是不可变的，在内存中占有固定大小的空间，它们都会被存储在栈（stack）中

```jsx
let a = 2;
let b = a;
b++;
console.log(a, b);  // 2, 3
```

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC15%E6%9C%9F%EF%BC%9AJavaScript%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%203f760bac4f764b10a4a6ca1d58f170bd/Untitled.png)

- `a`和`b`的值相互独立，改变`b`不会影响`a`
- 在调用函数时，传递给函数的参数如果是值类型，也是通过值复制的方式传递

---

- `引用类型：` 不同变量的指针执行了同一个对象（数组，对象，函数）

```jsx
const a = { name: "zhangsan", age: 20 };
const b = a;

b.name = "lisi";
console.log(a); // {name:"lisi", age:20}
console.log(b); // {name:"lisi", age:20}
```

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC15%E6%9C%9F%EF%BC%9AJavaScript%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%203f760bac4f764b10a4a6ca1d58f170bd/Untitled%201.png)

- 在调用函数时，传递给函数参数如果是引用类型，也是通过引用复制的方式传递

```jsx
const a = { name: "zhangsan", age: 20 };

function foo(b) {
  b.name = "lisi";
  console.log(b);// {name:"lisi", age:20}
}

foo(a);
console.log(a); // {name:"lisi", age:20}
```

```jsx
let a = { name: "zhangsan", age: 20 };
let b = a;
b = { value: 123 };

console.log(a); // {name:"zhangsan", age:20}
console.log(b); // {value:123}
```

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC15%E6%9C%9F%EF%BC%9AJavaScript%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%203f760bac4f764b10a4a6ca1d58f170bd/Untitled%202.png)

# 为什么会有栈内存和堆内存？

- 与`垃圾回收机制`有关 - 为了使程序运行时占用内存最小
- 当一个方法执行时，每个方法都会建立自己的内存栈，在这个方法内定义的变量将会被逐个放入这块栈内存里，当方法执行结束，这个方法的内存栈也会被销毁。因此，`所有在方法中定义的变量`都存放在`栈内存`中
- 但当在程序创建一个对象时，这个对象将被保存到运行时数据区中，以便反复利用（因为对象的创建成本通常较大），这个运行时数据区就是`堆内存`。堆内存中的对象不会随方法的结束而销毁，即使方法调用结束后，只要这个对象还可能被另一个变量所引用，则这个对象就不会被销毁；只有当一个对象没有被任何变量引用它时，系统的垃圾回收机制才会回收它。
- `const`保证的是栈内存中数据不变性，堆中内容可以改变