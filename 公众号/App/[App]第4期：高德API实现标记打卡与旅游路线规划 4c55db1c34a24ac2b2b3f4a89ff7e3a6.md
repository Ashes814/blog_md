# [App]第4期：高德API实现标记打卡与旅游路线规划

来源：[https://www.bilibili.com/video/BV1Ui4y1U7c6/](https://www.bilibili.com/video/BV1Ui4y1U7c6/?spm_id_from=333.337.search-card.all.click&vd_source=97a695383c4b98a4ed8b80d5fdecc716)

代码：[https://github.com/Ashes814/smart-campus](https://github.com/Ashes814/smart-campus)

# 0. 简介

- 高德API地图展示与基本控件

![Untitled](%5BApp%5D%E7%AC%AC4%E6%9C%9F%EF%BC%9A%E9%AB%98%E5%BE%B7API%E5%AE%9E%E7%8E%B0%E6%A0%87%E8%AE%B0%E6%89%93%E5%8D%A1%E4%B8%8E%E6%97%85%E6%B8%B8%E8%B7%AF%E7%BA%BF%E8%A7%84%E5%88%92%204c55db1c34a24ac2b2b3f4a89ff7e3a6/Untitled.png)

- 添加标记

![Untitled](%5BApp%5D%E7%AC%AC4%E6%9C%9F%EF%BC%9A%E9%AB%98%E5%BE%B7API%E5%AE%9E%E7%8E%B0%E6%A0%87%E8%AE%B0%E6%89%93%E5%8D%A1%E4%B8%8E%E6%97%85%E6%B8%B8%E8%B7%AF%E7%BA%BF%E8%A7%84%E5%88%92%204c55db1c34a24ac2b2b3f4a89ff7e3a6/Untitled%201.png)

- 点击一次表示打卡一次，为每个标记记录打卡次数并存入localStorage

![Untitled](%5BApp%5D%E7%AC%AC4%E6%9C%9F%EF%BC%9A%E9%AB%98%E5%BE%B7API%E5%AE%9E%E7%8E%B0%E6%A0%87%E8%AE%B0%E6%89%93%E5%8D%A1%E4%B8%8E%E6%97%85%E6%B8%B8%E8%B7%AF%E7%BA%BF%E8%A7%84%E5%88%92%204c55db1c34a24ac2b2b3f4a89ff7e3a6/Untitled%202.png)

- 旅行路线规划（驾车最短时间）

![Untitled](%5BApp%5D%E7%AC%AC4%E6%9C%9F%EF%BC%9A%E9%AB%98%E5%BE%B7API%E5%AE%9E%E7%8E%B0%E6%A0%87%E8%AE%B0%E6%89%93%E5%8D%A1%E4%B8%8E%E6%97%85%E6%B8%B8%E8%B7%AF%E7%BA%BF%E8%A7%84%E5%88%92%204c55db1c34a24ac2b2b3f4a89ff7e3a6/Untitled%203.png)

# 1.工具准备

- 高德官网申请API KEY和安全码
- 添加以下代码，否则在调用路径规划时会报`INVALID_USER_SCODE`

```jsx
window._AMapSecurityConfig = {
  securityJsCode: '你的安全码',
};
```

- `npm i @amap/amap-jsapi-loader --save`

# 2.建立地图容器

- `AMapLoader` 引入`AMap`，返回值是一个`Promise`
- 分别建立`AMap`和`map`对应的`state`，用于组件间传递
- `AMap.plugin`中的第一个参数必须添加`“AMap.GeoJSON”` 否则会找不到这个控件

![Untitled](%5BApp%5D%E7%AC%AC4%E6%9C%9F%EF%BC%9A%E9%AB%98%E5%BE%B7API%E5%AE%9E%E7%8E%B0%E6%A0%87%E8%AE%B0%E6%89%93%E5%8D%A1%E4%B8%8E%E6%97%85%E6%B8%B8%E8%B7%AF%E7%BA%BF%E8%A7%84%E5%88%92%204c55db1c34a24ac2b2b3f4a89ff7e3a6/Untitled%204.png)

# 3. 标记与打卡功能

- `localStorage`存取数据

![Untitled](%5BApp%5D%E7%AC%AC4%E6%9C%9F%EF%BC%9A%E9%AB%98%E5%BE%B7API%E5%AE%9E%E7%8E%B0%E6%A0%87%E8%AE%B0%E6%89%93%E5%8D%A1%E4%B8%8E%E6%97%85%E6%B8%B8%E8%B7%AF%E7%BA%BF%E8%A7%84%E5%88%92%204c55db1c34a24ac2b2b3f4a89ff7e3a6/Untitled%205.png)

- 从l`ocalStorage`读取GeoJSON并渲染标记

![Untitled](%5BApp%5D%E7%AC%AC4%E6%9C%9F%EF%BC%9A%E9%AB%98%E5%BE%B7API%E5%AE%9E%E7%8E%B0%E6%A0%87%E8%AE%B0%E6%89%93%E5%8D%A1%E4%B8%8E%E6%97%85%E6%B8%B8%E8%B7%AF%E7%BA%BF%E8%A7%84%E5%88%92%204c55db1c34a24ac2b2b3f4a89ff7e3a6/Untitled%206.png)

- 添加新的标记与打卡功能，将其保存至`localStroage`

![Untitled](%5BApp%5D%E7%AC%AC4%E6%9C%9F%EF%BC%9A%E9%AB%98%E5%BE%B7API%E5%AE%9E%E7%8E%B0%E6%A0%87%E8%AE%B0%E6%89%93%E5%8D%A1%E4%B8%8E%E6%97%85%E6%B8%B8%E8%B7%AF%E7%BA%BF%E8%A7%84%E5%88%92%204c55db1c34a24ac2b2b3f4a89ff7e3a6/Untitled%207.png)

# 4. 最短时间路径规划

- 调用`AMap.Driving` 做路径搜索，详见[官方文档](https://developer.amap.com/api/jsapi-v2/documentation#drivingsearch)
- 用`index` 来确认第一个添加的标记为起点，最后一个添加的标记为终点

![Untitled](%5BApp%5D%E7%AC%AC4%E6%9C%9F%EF%BC%9A%E9%AB%98%E5%BE%B7API%E5%AE%9E%E7%8E%B0%E6%A0%87%E8%AE%B0%E6%89%93%E5%8D%A1%E4%B8%8E%E6%97%85%E6%B8%B8%E8%B7%AF%E7%BA%BF%E8%A7%84%E5%88%92%204c55db1c34a24ac2b2b3f4a89ff7e3a6/Untitled%208.png)

- 路径动画实现

![Untitled](%5BApp%5D%E7%AC%AC4%E6%9C%9F%EF%BC%9A%E9%AB%98%E5%BE%B7API%E5%AE%9E%E7%8E%B0%E6%A0%87%E8%AE%B0%E6%89%93%E5%8D%A1%E4%B8%8E%E6%97%85%E6%B8%B8%E8%B7%AF%E7%BA%BF%E8%A7%84%E5%88%92%204c55db1c34a24ac2b2b3f4a89ff7e3a6/Untitled%209.png)