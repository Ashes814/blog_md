# 【GIS开发日记】第41期： DOM基础

> *分享GIS开发面经八股，GIS基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧*
> 

# DOM是什么？BOM又是什么？

- Document Object Model - 文件对象模型：将所有的页面内容表示为可编辑的对象
- `document`对象是页面的`entry point` 我们可以使用其改变或创建页面中的任何内容
- DOM不仅仅针对浏览器，服务端处理HTML页面时也可以使用DOM
- Browser Object Model - 浏览器对象模型：浏览器（主机环境）提供的对象，工作于document以外的环境
    - `navigator`对象：提供浏览器和操作系统的背景信息`navigator.userAgent` - 当前浏览器；`navigator.platform` - 当前平台（区分Win，Mac，Linux等）
    - `location`对象：读取当前URL或重定向

# 一个HTML文档在浏览器内部以什么表示？

- `DOM树`
- 标签成为元素节点
- 文本成为文本节点
- HTML中的所有元素均有对应的DOM节点，注释也有

# 比较DOM中6种主要的节点搜索方法

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC41%E6%9C%9F%EF%BC%9A%20DOM%E5%9F%BA%E7%A1%80%2097cda30bd1574dd7ae8bc41ef9ce1d61/Untitled.png)

# tagName和nodeName的区别有哪些？

- `tagName` 仅存在于`Element` Node
- `nodeName` 存在于所有Node中，在`Element` Node中与`tagName`一致