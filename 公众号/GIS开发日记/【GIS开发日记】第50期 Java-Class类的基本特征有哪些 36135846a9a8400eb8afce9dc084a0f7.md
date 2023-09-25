# 【GIS开发日记】第50期: Java-Class类的基本特征有哪些

> *分享GIS开发面经八股，GIS基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧*
> 

# Class类的基本特征

- `class`也是类，因此也继承`Object`类
- `class`类对象不是`new`出来的，而是系统创建的
- 对于某个类的`Class`对象，在内存中只有一份，因为类只加载一次
    
    ```java
    
    public static void main(String[] args) throws Exception {
            Class cls1 = Class.forName("com.reflection.Cat");
            Class cls2 = Class.forName("com.reflection.Cat");
            System.out.println(cls1.hashCode());
            System.out.println(cls2.hashCode());
        }
    ```
    
    ![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC50%E6%9C%9F%20Java-Class%E7%B1%BB%E7%9A%84%E5%9F%BA%E6%9C%AC%E7%89%B9%E5%BE%81%E6%9C%89%E5%93%AA%E4%BA%9B%2036135846a9a8400eb8afce9dc084a0f7/Untitled.png)
    
- 每个类的实例都会记得自己是由哪个`Class`实例所生成
- 通过`Class`对象可以完整地得到一个类的结构（通过一系列API）
- `Class`对象是存放在堆中的
- [类的字节码二进制数据是放在方法区的，有的地方称为类的元数据（方法代码，变量名，方法名，访问权限等等）](https://www.zhihu.com/question/38496907)

# 简析获取Class对象的六种方式

```java
public static void main(String[] args) throws Exception {
        //代码阶段，编译阶段
        Class cls1 = Class.forName("com.reflection.Cat"); // often used in properties
        System.out.println(cls1);
        //加载阶段
        System.out.println(Cat.class);// often used in parameter transition
        //运行阶段
        Cat cat = new Cat();
        System.out.println(cat.getClass());
        //类加载器
        //(1)先得到类加载器car
        ClassLoader classLoader = cat.getClass().getClassLoader();
        //(2)get Class object by class loader
        Class cls4 = classLoader.loadClass("com.reflection.Cat");
        System.out.println(cls4);
        //基本数据类型.class
        Class<Integer> integerClass = int.class;
        System.out.println(integerClass);
        //包装类.type
        Class<Integer> type = Integer.TYPE;
        System.out.println(type);
    }
```

# 绘制类加载的流程图

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC50%E6%9C%9F%20Java-Class%E7%B1%BB%E7%9A%84%E5%9F%BA%E6%9C%AC%E7%89%B9%E5%BE%81%E6%9C%89%E5%93%AA%E4%BA%9B%2036135846a9a8400eb8afce9dc084a0f7/Untitled%201.png)