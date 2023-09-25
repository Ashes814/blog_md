# 【GIS 开发日记】第 40 期： JS 日期

> _分享 GIS 开发面经八股，GIS 基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧_

# 如何获取 2023-07-01 格式的日期

```jsx
"use strict";
function formatDate(date) {
  const dt = date || new Date();
  const year = dt.getFullYear();
  let month = (dt.getMonth() + 1).toString(); //月份是从0开始计数的，所以需要加1
  let day = dt.getDate().toString();
  month = month.length === 1 ? "0" + month : month;
  day = day.length === 1 ? "0" + day : day;
  return `${year}-${month}-${day}`;
}
console.log(formatDate());
```

q

# 如何获取一个长度一致的随机数字字符串

```jsx
"use strict";
function randomStr(str) {
  let random = str || Math.random();
  random = random + "0000000000";
  random = random.slice(0, 10);
  return random;
}

console.log(randomStr());
```

# 写一个能遍历对象和数组的通用 forEach 函数

```jsx
"use strict";
function forEach(obj, fn) {
  if (typeof obj === "object") {
    for (let key in obj) {
      if (obj.hasOwnProperty(key)) {
        fn(key, obj[key]);
      }
    }
  } else if (obj instanceof Array) {
    obj.forEach(function (item, index) {
      fn(index, item);
    });
  }
}
```

# 数组中的常用方法有哪些？

- `forEach`： 遍历所有元素
- `every`：判断所有的元素是否都符合条件
- `some`：判断是否至少有一个元素符合条件
- `sort`：排序
- `map`：对数组中的元素重新组装，生成一个新的数组
- `filter`：过滤符合条件的元素
