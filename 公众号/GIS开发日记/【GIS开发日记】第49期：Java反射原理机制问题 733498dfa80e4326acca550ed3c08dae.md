# 【GIS开发日记】第49期：Java反射原理机制问题

> *分享GIS开发面经八股，GIS基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧*
> 

# 绘制Java反射机制原理图

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC49%E6%9C%9F%EF%BC%9AJava%E5%8F%8D%E5%B0%84%E5%8E%9F%E7%90%86%E6%9C%BA%E5%88%B6%E9%97%AE%E9%A2%98%20733498dfa80e4326acca550ed3c08dae/Untitled.png)

# 反射机制能够实现哪些功能

- 在运行时判断任意一个对象所属的类
- 在运行时构造任意一个类的对象
- 在运行时得到任意一个类所具有的成员变量和方法
- 在运行时调用任意一个对象的成员变量和方法
- 生成动态代理

# 辨析反射的优点与缺点

- 可以动态的创建和使用对象（也是框架的底层核心），使用灵活，没有反射机制，框架技术就失去底层支撑
- 使用反射基本是解释执行，对执行速度有影响

```java
package com.reflection;

import java.lang.reflect.Method;

/**
 * @author 欧欧
 * @version 1.0
 */
public class Reflection02  {
    public static void main(String[] args) throws Exception {
        m1();
        m2();
        m3();
    }
    // traditional method
    public static void m1() {
        Cat cat = new Cat();
        long start = System.currentTimeMillis();
        for (int i = 0; i < 900000000; i++) {
            cat.hi();
        }
        long end = System.currentTimeMillis();
        System.out.println("Traditonal Way: " + (end - start));
    }
    // reflection method
    public static void m2() throws Exception{
        Class cls = Class.forName("com.reflection.Cat");
        Object o = cls.newInstance();
        Method hi = cls.getMethod("hi");
        long start = System.currentTimeMillis();
        for (int i = 0; i < 900000000; i++) {
            hi.invoke(o);
        }
        long end = System.currentTimeMillis();
        System.out.println("Reflection Way: " + (end - start));
    }

    //reflection optimize
    public static void m3() throws Exception{
        Class cls = Class.forName("com.reflection.Cat");
        Object o = cls.newInstance();
        Method hi = cls.getMethod("hi");
        hi.setAccessible(true);
        long start = System.currentTimeMillis();
        for (int i = 0; i < 900000000; i++) {
            hi.invoke(o);
        }
        long end = System.currentTimeMillis();
        System.out.println("Reflection Way: " + (end - start));
    }
}
```

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC49%E6%9C%9F%EF%BC%9AJava%E5%8F%8D%E5%B0%84%E5%8E%9F%E7%90%86%E6%9C%BA%E5%88%B6%E9%97%AE%E9%A2%98%20733498dfa80e4326acca550ed3c08dae/Untitled%201.png)