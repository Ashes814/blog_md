# 【GIS开发日记】第17期：JS数据类型

> *分享GIS开发面经八股，GIS基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧*
> 

# `typeof`可以检测的数据类型有哪些？

- `八种基本数据类型：` `number，bigint，string，boolean，null，undefined，symbol`（七种原始类型），`object`（一种引用类型）
- `typeof`可以区分值类型，不能区分引用类型

```jsx
typeof undefined // "undefined"

typeof 0 // "number"

typeof 10n // "bigint"

typeof true // "boolean"

typeof "foo" // "string"

typeof Symbol("id") // "symbol"

typeof Math // "object"  

typeof null // "object"  这是官方承认的 typeof 的错误，这个问题来自于 JavaScript 语言的早期阶段，并为了兼容性而保留了下来。null 绝对不是一个 object。null 有自己的类型，它是一个特殊值。typeof 的行为在这里是错误的

typeof alert // "function" 
```

# [JS中`===`与`==`的区别？](https://www.freecodecamp.org/news/loose-vs-strict-equality-in-javascript/)

- `==`会进行强制类型转换之后再比较（loose equality），`===`不会进行强制类型转换（strict equality）
- `==` 并不会改变原始数据
- `===`先比较类型，类型相同再比较value

```jsx
const a = true;
const b = 1;
console.log(a == b); // true
console.log(a === b); // false
```

## JS的强制转换规则

- 如果有一个是`string`，其它转换成`string`
- 如果有一个是`number`，其它转换成`number`
- 如果有一个是`boolean`，则将其转换成`number`，true-1，false-0
- 如果有一个是`object`，另一个是`原始类型`，则`object`在比较前转换成`原始类型`
- 如果有一个是`null`或`undefined`，则另外一个必须也是`null`或`undefined` (即null==null或null==undefined)，否则false

```jsx
const a = true; // 强转成1
const b = 'true';

console.log(a == b) // false
```

## 使用场景

- 判断对象属性是否存在使用`==`

```jsx

let obj1 = {};
console.log(obj1.name == null) // true
console.log(obj1.name == undefined) // true
console.log(obj1.name === null) // false
console.log(obj1.name === undefined) // true
```

- 判断函数参数是否存在使用`==`,如果这个函数内部只会出现`undefined`，则`undefined==null` 也会成立

```jsx
function(a, b) {
	if (a == null) ...
}
```

- 其它情况一般都用`===`做判断

# JS中类型转换为false的有哪些？

- `null，undefined，NaN，’’，false，0`