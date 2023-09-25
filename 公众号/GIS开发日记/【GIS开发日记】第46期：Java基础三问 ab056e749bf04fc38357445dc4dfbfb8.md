# 【GIS开发日记】第46期：Java基础三问

> *分享GIS开发面经八股，GIS基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧*
> 

# 如何使用短路与符避免除0错误

```java
x != 0 && 1 / x
```

- 如果x为0，则第一个返回`false`，短路与不会进行第二个判断，也就避免了除0错误

# 如何通过较短的字符串构建字符串，不采用字符串拼接的方式，以节省时间和空间

- 利用`StringBuilder`

```java
StringBuilder builder = new StringBuilder();
builder.append('a');
builder.append("你太");
builder.append("美");
System.out.println(builder.toString());
```

# 如何使用Java标签跳出特定循环？

```java
Scanner scanner = new Scanner(System.in);
int n = 4;
readData: // 定义标签，用于break
while (n > 0) {
    for (int i = 0; i < 10; i++) {
        for (int j = 0; j < 10; j++) {
            if (j == 5) {
                System.out.println("jbreak");
                break;
            }

            if (i == 5) {
                System.out.println("ibreak");
                break readData;
            }
        }
    }
}
```