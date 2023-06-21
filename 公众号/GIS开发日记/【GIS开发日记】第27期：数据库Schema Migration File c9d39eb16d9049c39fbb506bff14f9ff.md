# 【GIS开发日记】第27期：数据库Schema Migration File

> *分享GIS开发面经八股，GIS基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧*
> 

# 实际工程中如何解决以下两个问题

- 案例，需要将数据库`comments`表的`contents`字段重命名成`body`
- 第一：改变本地数据库DB的结构的同时改变客户端的相关数据库结构，保证不出问题
- 第二：当与他人协作时，将数据库结构与SQL代码绑定操作

## 设计Schema Migration File，

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC27%E6%9C%9F%EF%BC%9A%E6%95%B0%E6%8D%AE%E5%BA%93Schema%20Migration%20File%20c9d39eb16d9049c39fbb506bff14f9ff/Untitled.png)

- 对于问题1:更新远端数据库前自动运行所有的`Migrations`

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC27%E6%9C%9F%EF%BC%9A%E6%95%B0%E6%8D%AE%E5%BA%93Schema%20Migration%20File%20c9d39eb16d9049c39fbb506bff14f9ff/Untitled%201.png)

- 对于问题2: 发起`Review Request`即可

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC27%E6%9C%9F%EF%BC%9A%E6%95%B0%E6%8D%AE%E5%BA%93Schema%20Migration%20File%20c9d39eb16d9049c39fbb506bff14f9ff/Untitled%202.png)

## 利用`node-pg-migrate`实现一个Migration File

```bash
npm install node-pg-migrate pg
```

- `修改package.json`

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC27%E6%9C%9F%EF%BC%9A%E6%95%B0%E6%8D%AE%E5%BA%93Schema%20Migration%20File%20c9d39eb16d9049c39fbb506bff14f9ff/Untitled%203.png)

```bash
npm run migrate create table comments #create a migration file named as table comments
```

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC27%E6%9C%9F%EF%BC%9A%E6%95%B0%E6%8D%AE%E5%BA%93Schema%20Migration%20File%20c9d39eb16d9049c39fbb506bff14f9ff/Untitled%204.png)

- 创建SQL-示例

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC27%E6%9C%9F%EF%BC%9A%E6%95%B0%E6%8D%AE%E5%BA%93Schema%20Migration%20File%20c9d39eb16d9049c39fbb506bff14f9ff/Untitled%205.png)

- 运行

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC27%E6%9C%9F%EF%BC%9A%E6%95%B0%E6%8D%AE%E5%BA%93Schema%20Migration%20File%20c9d39eb16d9049c39fbb506bff14f9ff/Untitled%206.png)

- 需要撤回的话仅需将`up`改成`down`就可以撤销之前对数据库的修改