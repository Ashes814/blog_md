# 【GIS开发日记】第24期： JAVA包装流实例与序列化

> *分享GIS开发面经八股，GIS基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧*
> 

# 复制一张图片能否使用`BufferedReader`和`BufferedWriter`流？为什么？怎么修改？

- 不能使用，因为这两个包装流是按字符操作的，无法操作二进制文件如图片，音频，视频，pdf等
- 改用`BufferedInputStream`和`BufferedOutputStream`

```java

package com.file;

import java.io.*;

/**
 * @author 欧欧
 * @version 1.0
 */
public class BufferedInOutStream {
    public static void main(String[] args) {
        String srcFliePath = "/Users/zowcool/Desktop/Learning/hsp-JAVA-basic/chapter19/src/com/file/bag.png";
        String destFliePath = "/Users/zowcool/Desktop/Learning/hsp-JAVA-basic/chapter19/src/com/file/bag_copy.png";

        // 创建BufferedOutputStream对象和BufferedInputStream对象
        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;

        try {
            //使用对应的节点流创建Buffered包装流, FileInputStream是InputStream的子类
            bis = new BufferedInputStream(new FileInputStream(srcFliePath));
            bos = new BufferedOutputStream(new FileOutputStream(destFliePath));

            //循环读取文件，并写入的destFilePath
            byte[] buff = new byte[1024];
            int readLen = 0;

            while ((readLen = bis.read(buff)) != -1) {
                bos.write(buff, 0, readLen);

            }

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 关闭流，关闭外层的处理流即可，底层会去关闭节点流

            try {
                if (bis != null) {
                    bis.close();
                }
                if (bos != null) {
                    bos.close();
                }
            } catch (IOException e) {
                throw new RuntimeException(e);
            }

        }

    }
}
```

# 如何在保存数据时保存对应的数据类型，如`int 100` ，以及如何保存一个对象`Dog dog = new Dog(”wangcai”, 5)`?

- 本题即-如何将`基本数据类型`与`对象`进行`序列化`与`反序列化`的操作
- `序列化`：保存数据时，同时保存数据值与数据类型
- `反序列化`：恢复数据时，同时恢复数据值与数据类型
- 让某个对象实现序列化与反序列化，就要让他的类实现`Serializable`或`Externalizable`接口
- 推荐使用`Serializable`（`标记接口：`内部没有任何方法，起声明作用）

## 应用案例

- `ObjectOutputStream`（实现序列化）

```java
package com.file;

import java.io.FileOutputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;

/**
 * @author 欧欧
 * @version 1.0
 */
public class ObjectOutStream_ {
    public static void main(String[] args) {

        //序列化后，保存的文件格式，不是纯稳步
        String filePath = "/Users/zowcool/Desktop/Learning/hsp-JAVA-basic/chapter19/src/com/file/data.dat";

        try {
            ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(filePath));

            // 序列化数据到 filePath
            oos.write(100); // int -> Integer() (实现了 Serializable)
            oos.writeBoolean(true); // boolean -> Boolean (实现了Serializable)
            oos.writeChar('a'); // char -> Character (实现了Serializable)
            oos.writeDouble(9.5); // double -> Double (实现了Serializable)
            oos.writeUTF("古娜拉黑暗之神"); // String
            // 保存一个dog对象
            oos.writeObject(new Dog("泽塔", 10));
            oos.close();
            System.out.println("数据保存完毕（序列化形式）");
        } catch (Exception e) {
            throw new RuntimeException(e);
        }

    }
}

class Dog implements Serializable {
    private String name;
    private int age;

    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
    }

}
```

- `ObjectInputStream`（实现反序列化）

```java
package com.file;

import java.io.*;
import java.sql.SQLOutput;

/**
 * @author 欧欧
 * @version 1.0
 */
public class ObjectInStream_ {
    public static void main(String[] args) throws Exception {

        //指定反序列化的文件
        String filePath = "/Users/zowcool/Desktop/Learning/hsp-JAVA-basic/chapter19/src/com/file/data.dat";

        ObjectInputStream ois = new ObjectInputStream(new FileInputStream(filePath));

        //读取
        //1. 读取（反序列化）的顺序需要和你保存的数据（序列化）的顺序一致
        System.out.println(ois.readInt());
        System.out.println(ois.readBoolean());
        System.out.println(ois.readChar());
        System.out.println(ois.readDouble());
        System.out.println(ois.readUTF());

        Object dog = ois.readObject(); // 向上转型
        System.out.println(dog); //Object -> Dog

				// 使用Dog时需要向下转型
				// 需要将Dog类的定义放在可以引用的位置
        // 关闭外层流
        ois.close();

    }
}
```

## 注意事项⚠️

- 读写顺序要一致
- 序列化和反序列化的对象需要实现`Serializable`接口
- 序列化的类中建议添加`SerialVersionUID`，以提高版本兼容性
- 序列化属性时，默认将所有的属性序列化，除了`static`和`transient`修饰的成员
- 序列化对象时，要求其中的属性类型也实现序列化接口
- 被序列化的类，其子类也会被序列化，具有继承性