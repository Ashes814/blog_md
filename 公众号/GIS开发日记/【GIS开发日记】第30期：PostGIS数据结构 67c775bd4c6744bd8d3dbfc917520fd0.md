# 【GIS开发日记】第30期：PostGIS数据结构

> *分享GIS开发面经八股，GIS基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧*
> 

# 简介PostGIS中的Vector Geometry Model

- 符合OGC标准
- 基础数据结构是点`Point`- 2D (XY), 3D (XYZ)
- 折线`LineString` 是点的集合
- 面`Polygon`是闭合折线的集合
- 基本图形可以构成多点`MultiPoint`，多线`MultiLineString` ，多面`MultiLineString`
- 基本图形`Geometry` 和属性`Attributes` 构成要素`Features`

# SRID是什么，如何在PostGIS中查看

- SRID（`Spatial Reference ID`）是CRS（`Coordinate reference system`）的唯一标识，`Well Known Text`
- Coordinate System，Projection，Zone，Datum
- `SELECT * FROM spatial_ref_sys`

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC30%E6%9C%9F%EF%BC%9APostGIS%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%2067c775bd4c6744bd8d3dbfc917520fd0/Untitled.png)

# Geometry与Geography Data Type的区别

- 都是存储要素空间信息的方法

## Geometry

- Geometry基于平面地表
- Geometry可以是多种`CRS`的
- Geometry的数学计算相对简单
- Geometry具有很多操作函数
- Geometry符合OGC标准，GEOS库等
- Geometry的精度随着空间面积的增加而减小

## Geography

- Geography基于球面地表，更复杂，更精确
- Geography必须是GCS`（Geographic Coordinate Systems）`经纬度
- Geography的数学计算复杂
- Geography只支持部分函数
- Geography没有特定标准
- Geography精度高

## 选择使用哪个时考虑的因素

- 数据尺度
- 是否有能够使用的函数
- 性能
- 精度