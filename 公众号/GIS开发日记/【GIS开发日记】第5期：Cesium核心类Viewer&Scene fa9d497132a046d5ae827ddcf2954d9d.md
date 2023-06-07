# 【GIS开发日记】第5期：Cesium核心类Viewer&Scene

> *分享GIS开发面经八股，GIS基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧*
> 

# `Viewer`类是什么，主要功能？

- `Viewer`是`Cesium`的核心类
- 展示三维要素内容主窗口
- 定义基础控件，图层初始状态等

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC5%E6%9C%9F%EF%BC%9ACesium%E6%A0%B8%E5%BF%83%E7%B1%BBViewer&Scene%20fa9d497132a046d5ae827ddcf2954d9d/Untitled.png)

# `Scene`类是什么，主要功能？

- 构建场景
1. 基础地理环境设置：地球参数，光照，雾，大气
2. 基础图层设置：地图图层，地形图层（Vierew类中也可以设置）
3. 场景数据
4. 场景交互函数：鼠标事件，相机事件等

# 解释`Entity`的作用？

- 由`Primitive`封装而来，使得开发者专注于数据的呈现，而不必担心底层的可视化机制
- `Primitive`则是更底层的图元，它通常表示一组几何形状（如点、线、面等）以及相关的渲染信息（如颜色、材质、纹理等）。`Primitive`通常不具备完整的实体属性和操作，而是单独渲染。
- 因此，在实际应用中，`Entity`通常作为高层次的可视化对象，而`Primitive`则作为`Entity`的组成部分或辅助元素。例如，在`Cesium`中，使用`Entities`来表示3D场景中的城市、车辆、航空器等，而使用`Primitives`来表示地形、建筑物、树木、雾等。
- 同时，`Cesium`也支持自定义`Entity`属性和`Primitive`渲染方式，开发者可以根据需求扩展或定制这两种元素的功能和表现形式。

# DataSourceCollection是什么？有什么作用？

- `Cesium`中加载矢量数据的主要方式之一
- 支持加载矢量数据集与外部文件的调用
- `CzmlDataSource`
- `KmlDataSource`
- `GeoJsonDataSource`