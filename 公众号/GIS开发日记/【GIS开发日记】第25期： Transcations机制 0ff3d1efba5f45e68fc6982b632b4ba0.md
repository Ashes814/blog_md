# 【GIS开发日记】第25期： Transcations机制

> *分享GIS开发面经八股，GIS基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧*
> 

# 什么是Transactions（事务），有哪些性质？

- 假设你想设计一个数据库操作，Alyson转账50元给Gia，需要两步操作
- 第一步：从账户表（`accout`）中将`Alyson`的账户减去50元

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC25%E6%9C%9F%EF%BC%9A%20Transcations%E6%9C%BA%E5%88%B6%200ff3d1efba5f45e68fc6982b632b4ba0/Untitled.png)

- 第二步：从账户表（`accout`）中将`Gia`的账户加上50元

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC25%E6%9C%9F%EF%BC%9A%20Transcations%E6%9C%BA%E5%88%B6%200ff3d1efba5f45e68fc6982b632b4ba0/Untitled%201.png)

- 但你是个倒霉蛋😓，在第一步完成后，第二步开始前数据库管理系统发生了故障，你只减去了`Alyson` 的50元而没有给`Gia`加上
- `Transactions` 就可以解决这种问题
- 运行`BEGIN`，使得当前的数据库连接进入一个特殊状态

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC25%E6%9C%9F%EF%BC%9A%20Transcations%E6%9C%BA%E5%88%B6%200ff3d1efba5f45e68fc6982b632b4ba0/Untitled%202.png)

- 相当于复制了一份数据表仅仅给当前连接进行操作（并没有真正的复制，这是一个简单的理解）

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC25%E6%9C%9F%EF%BC%9A%20Transcations%E6%9C%BA%E5%88%B6%200ff3d1efba5f45e68fc6982b632b4ba0/Untitled%203.png)

- 在Connection1中改变的数据需要通过`COMMIT`将这个改变合并入Main Data Pool
- 在Connection1中通过`ROLLBACK`将这个改变删除，并移除这个工作区

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC25%E6%9C%9F%EF%BC%9A%20Transcations%E6%9C%BA%E5%88%B6%200ff3d1efba5f45e68fc6982b632b4ba0/Untitled%204.png)

- 当运行错误时，会自动`ROLLBACK`

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC25%E6%9C%9F%EF%BC%9A%20Transcations%E6%9C%BA%E5%88%B6%200ff3d1efba5f45e68fc6982b632b4ba0/Untitled%205.png)

- 这个新工作区使得操作不直接改变源数据表，不会造成执行到一半崩溃后产生的错误

## Transactions的性质

1. `Atomicity（原子性）`：事务是一个原子操作，即所有操作不可分割。如果一个操作失败，则整个事务失败并且`ROLLBACK`到最初状态
2. `Consistency（一致性）`：事务完成后，数据库状态必须是一致的，即满足所有的完整性约束条件
3. `Isolation（隔离性）`：由于并发执行事务可能会产生意想不到的结果，因此需要对事务之间的执行进行隔离。这可以通过使用锁机制来实现
4. `Durability（持久性）`：一旦事务`COMMIT`，则其所做的更改将永久保存在数据库中，并且对于任何后续操作都是可见的。即便在系统故障的情况下，也必须保证数据的持久性