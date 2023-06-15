# 【GIS开发日记】第20期：JS原型基本规则

> *分享GIS开发面经八股，GIS基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧*
> 

# 原型链的五条基本规则是什么？

- 所有的引用类型（数组，对象，函数），都是具有对象特性的，即可以自由扩展属性（除了`null`）
- 所有的引用类型（数组，对象，函数），都有一个`proto`属性(隐式原型)，这个属性的值是一个普通的对象
- 所有的函数，都有一个`prototype`属性(显式原型)，这个属性的值是一个普通的对象
- 所有的引用类型（数组，对象，函数），`proto` 属性值指向（完全相等）它的构造函数的`prototype`的属性值
- 当试图得到一个对象的某一个属性的时候，如果一个对象本身没有这个属性的话，就会去它的`proto`（也就是它的构造函数中去寻找这个属性）

## 原型的概念

- JS对象有一个特殊的隐藏属性`[[prototype]]` ，使用特殊的名字`__proto__`，当访问当前对象的某一属性不存在时，会查找它的`prototype`是否有这个属性
- `__proto__` 实际上是`[[prototype]]` 的`getter`和`setter`
- `__proto__` 过时，现在应该使用`Object.getPrototypeOf`和`Object.setPrototypeOf`
- 任何对象只能有一个`[[prototype]]`

# 下面这段代码的输出是什么？

```jsx
let user = {
  name: "John",
  surname: "Smith",

  set fullName(value) {
    [this.name, this.surname] = value.split(" ");
  },

  get fullName() {
    return `${this.name} ${this.surname}`;
  }
};

let admin = {
  __proto__: user,
  isAdmin: true
};

alert(admin.fullName); // (*)

// setter triggers!
admin.fullName = "Alice Cooper"; // (**)

alert(admin.fullName); //
alert(user.fullName); // 
```

- 对于`Accessor properties` 如`getter，setter` 的操作是执行在其本身的
- `this` 永远指向`.`前面的那个对象

```jsx

alert(admin.fullName); // John Smith (*)

// setter triggers!
admin.fullName = "Alice Cooper"; // (**)

alert(admin.fullName); // Alice Cooper, state of admin modified
alert(user.fullName); // John Smith, state of user protected
```

# 下面这段代码输出什么？

```jsx
let animal = {
  eats: true
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

// Object.keys only returns own keys
alert(Object.keys(rabbit)); // 

// for..in loops over both own and inherited keys
for(let prop in rabbit) alert(prop); // 
```

- `for…in` 循环中会遍历所有`prototype` 的属性，如果不想这样，可以使用`obj.hasOwnProperty`

```jsx
let animal = {
  eats: true
};

let rabbit = {
  jumps: true,
  __proto__: animal
};

// Object.keys only returns own keys
alert(Object.keys(rabbit)); // jumps

// for..in loops over both own and inherited keys
for(let prop in rabbit) alert(prop); // jumps, then eats
```