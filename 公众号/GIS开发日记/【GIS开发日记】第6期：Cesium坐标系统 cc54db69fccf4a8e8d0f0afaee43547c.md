# 【GIS开发日记】第6期：Cesium坐标系统

> *分享GIS开发面经八股，GIS基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧*
> 

# Cesium坐标系统主要有哪几个？

## 屏幕坐标系统（Cartesian2）

- 二维笛卡尔坐标系
- `new Cesium.Cartesian2(x, y)`
- 鼠标点击位置距离左上角的`x，y`值

## 笛卡尔空间直角坐标系（Cartesian3）

- 立体几何`x, y, z`轴组成的空间坐标系
- 计算机绘图不方便直接使用经纬度，所欲会将其转换成空间直角坐标

## WGS-84

- GPS的坐标系统
- 坐标原点为地球质心

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC6%E6%9C%9F%EF%BC%9ACesium%E5%9D%90%E6%A0%87%E7%B3%BB%E7%BB%9F%20cc54db69fccf4a8e8d0f0afaee43547c/Untitled.png)

# Cesium如何进行坐标转换？

## 经纬度坐标转换为笛卡尔空间直角坐标系

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC6%E6%9C%9F%EF%BC%9ACesium%E5%9D%90%E6%A0%87%E7%B3%BB%E7%BB%9F%20cc54db69fccf4a8e8d0f0afaee43547c/Untitled%201.png)

[https://www.notion.so](https://www.notion.so)

## 笛卡尔空间直角坐标系转换为经纬度坐标

[https://www.notion.so](https://www.notion.so)

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC6%E6%9C%9F%EF%BC%9ACesium%E5%9D%90%E6%A0%87%E7%B3%BB%E7%BB%9F%20cc54db69fccf4a8e8d0f0afaee43547c/Untitled%202.png)

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC6%E6%9C%9F%EF%BC%9ACesium%E5%9D%90%E6%A0%87%E7%B3%BB%E7%BB%9F%20cc54db69fccf4a8e8d0f0afaee43547c/Untitled%203.png)

## 屏幕坐标与笛卡尔空间坐标的转换

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC6%E6%9C%9F%EF%BC%9ACesium%E5%9D%90%E6%A0%87%E7%B3%BB%E7%BB%9F%20cc54db69fccf4a8e8d0f0afaee43547c/Untitled%204.png)