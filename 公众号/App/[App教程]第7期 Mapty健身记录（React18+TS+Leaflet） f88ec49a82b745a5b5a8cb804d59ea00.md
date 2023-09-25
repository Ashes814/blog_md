# [App教程]第7期 : Mapty健身记录（React18+TS+Leaflet）

源码:[https://github.com/Ashes814/workout-react-ts](https://github.com/Ashes814/workout-react-ts)

# 功能概述

- 展示地图
- 单击添加标记并弹窗，弹出运动记录
- 运动记录分为跑步和骑车两种模式
- 提供表单记录运动详情
- 设计UI形成运动列表
- 单击运动项目可以在地图上实现跳转

![20230720_181202.gif](%5BApp%E6%95%99%E7%A8%8B%5D%E7%AC%AC7%E6%9C%9F%20Mapty%E5%81%A5%E8%BA%AB%E8%AE%B0%E5%BD%95%EF%BC%88React18+TS+Leaflet%EF%BC%89%20f88ec49a82b745a5b5a8cb804d59ea00/20230720_181202.gif)

# 关键词

- `React18`
- `TypeScript`
- `Leaflet`

# 组件拆解

## `MapView`

- 创建地图容器
- 选择地图瓦片
- 存放MapMarker - 地图标记

```jsx
import React, { useContext } from "react";
import { MapContainer, TileLayer, Marker, Popup } from "react-leaflet";
import markerIconPng from "leaflet/dist/images/marker-icon.png";
import { Icon } from "leaflet";
import "./index.css";
import MapMarker from "./MapMarker";
import MapContext from "../../store/map-context";
import { WorkoutType } from "../../App";
import { LatLngExpression } from "leaflet";

export default function MapView() {
  const ctx = useContext(MapContext);
  return (
    // 创建地图容器
    <MapContainer
      className="map-container"
      // 设置初始化地图中心
      center={[31.5, 121.5]}
      // 设置缩放级别
      zoom={13}
      //是否使用滚轮缩放
      scrollWheelZoom={false}
    >
      {/* 瓦片 */}
      <TileLayer
        attribution='&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
        url="https://{s}.tile.openstreetmap.fr/hot/{z}/{x}/{y}.png"
      />
      {/* 渲染已有数据 */}
      {ctx.data.map((workout: WorkoutType) => {
        return (
          <Marker
            key={workout.id}
            position={workout.loc as LatLngExpression}
            // 定义标记图标
            icon={
              new Icon({
                iconUrl: markerIconPng,
                iconSize: [25, 41],
                iconAnchor: [12, 41],
              })
            }
          >
            {/* 定义弹窗内容 */}
            <Popup>
              {workout.type} at
              {`${workout.date.getMonth() + 1}/${workout.date.getDate()}`}.
            </Popup>
          </Marker>
        );
      })}

      {/* 添加新增数据标记 */}
      <MapMarker />
    </MapContainer>
  );
}
```

## `MapMarker`

- `MapView` 的子组件
- 实现单击事件

```jsx
import React, { useContext } from "react";
import { Marker, Popup, useMapEvents } from "react-leaflet";
import markerIconPng from "leaflet/dist/images/marker-icon.png";
import { Icon } from "leaflet";
import MapContext from "../../../store/map-context";

export default function MapMarker() {
  const ctx = useContext(MapContext);

  // 通过useMapEvents绑定在地图上的单击事件
  const map = useMapEvents({
    click: (e) => {
      // 设置单击点坐标
      ctx.setPosition([e.latlng.lat, e.latlng.lng]);
      // 弹出空白运动数据表格
      ctx.setShowBlank(true);
    },
  });

  // 将map对象返回给App组件
  ctx.setMap(map);

  return (
    <Marker
      position={ctx.position}
      icon={
        new Icon({
          iconUrl: markerIconPng,
          iconSize: [25, 41],
          iconAnchor: [12, 41],
        })
      }
    >
      <Popup>请输入健身信息...</Popup>
    </Marker>
  );
}
```

## `Workouts`

- 健身记录的容器
- 设计健身记录UI

## `Workout`

- 单个健身记录
- 实现单击事件
- 根据健身记录类型的不同展示不同的数据（running，cycling）

## `WorkoutBlank`

- 空健身记录
- 展示添加健身记录时的表单

# 数据传输useContext

- 顶层父组件App中

```jsx
<MapContext.Provider
      // 通过context传递数据
      value={{
        flyToMarker,
        map,
        setMap,
        showBlank,
        setShowBlank,
        data,
        position,
        setPosition,
        addWorkoutHandler,
      }}
    >
      <div className="App">
        <Workouts />
        <div id="map-container" className="map-container">
          <MapView />
        </div>
      </div>
    </MapContext.Provider>
```