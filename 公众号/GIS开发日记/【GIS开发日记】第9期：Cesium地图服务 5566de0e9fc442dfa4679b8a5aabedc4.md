# 【GIS开发日记】第9期：Cesium地图服务

> *分享GIS开发面经八股，GIS基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧*
> 

# Cesium通过什么使用地图服务？如何自定义？

- Cesium默认使用Bing影像图，调用影像地图需要`imageProvider`函数

```jsx
// 加载MapBox
const layers = viewer.scene.imageryLayers;
const mapboxLayer = layers.addImageryProvider(
  new Cesium.MapboxImageryProvider({
    mapId: "mapbox.mapbox-streets-v8",
    accessToken:
      "your mapbox token",
  })
);
```

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC9%E6%9C%9F%EF%BC%9ACesium%E5%9C%B0%E5%9B%BE%E6%9C%8D%E5%8A%A1%205566de0e9fc442dfa4679b8a5aabedc4/Untitled.png)

- 可利用`GeoServer`发布自定义地图服务

# Cesium中的Primitive API与Entity API分别有哪些使用场景？

- `Primitive：`低层次，面向底层图形开发人员，效率高，灵活性强
- `Entity：`高层次，面向数据可视化，效率低，灵活性低