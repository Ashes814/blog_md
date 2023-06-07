# 【GIS开发日记】第10期：Cesium空间数据

> *分享GIS开发面经八股，GIS基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧*
> 

# Cesium空间数据有哪些？有哪些文件格式？

- Cesium栅格数据：影像图层，地形数据
- Cesium矢量数据（三维）：几何形体，三维模型，标签

## 几何形体

- `点要素`：`PointGraphics`
- `线要素`：`折线-PolylineGeometry（PolylineGraphics）`，`轮廓线-SimplePolylineGeometry`（没有宽度属性）
- `面要素`：轮廓线包围的几何面图形
- `体要素`

## GeoJSON

- `Cesium`主要采用`GeoJSON`与`KML`两种适合于网络传输的数据格式存储几何形体
- `GeoJSON`中，`geometry`表示地理数据，`type`表示要素类型，`coordinates`表示要素坐标数组`（WGS-84）`
- `Cesium.GeoJsonDataSource.load` 用于加载`GeoJSON`数据

## KML

- 谷歌开发，`OGC`接管，是一种基于`XML`用于保存地理信息的编码规范

## 三维模型-`GLTF`

- 特点是传输和解析高效，适用于网络传输

## 标签

- 用于标注信息，`LabelGraphics`，`BillboardGraphics`