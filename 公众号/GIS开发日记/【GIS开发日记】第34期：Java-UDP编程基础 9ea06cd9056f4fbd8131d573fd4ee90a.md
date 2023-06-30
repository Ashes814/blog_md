# 【GIS开发日记】第34期：Java-UDP编程基础

> *分享GIS开发面经八股，GIS基础（地图投影，空间分析，矢量栅格，地图算法），前端（HTML，CSS，JS/TS，React），开源地图库（Leaflet，Openlayers，Cesium），服务端（JAVA SE，GeoServer），数据库（MySQL，PostgreSQL，PostGIS，MongoDB）等知识技巧*
> 

# 简析Java中UDP网络编程原理

- 没有明确的服务端和客户端，演变成数据的发送端和接收端
- 接收数据和发送数据是通过`DatagramSocket`对象完成的
- 将数据封装到`DatagramPacket`对象/装包
- 当接收到`DatagramPacket`对象时，需要进行拆包，取出数据
- `DatagramPacket` 可以指定在哪个端口接收数据

![Untitled](%E3%80%90GIS%E5%BC%80%E5%8F%91%E6%97%A5%E8%AE%B0%E3%80%91%E7%AC%AC34%E6%9C%9F%EF%BC%9AJava-UDP%E7%BC%96%E7%A8%8B%E5%9F%BA%E7%A1%80%209ea06cd9056f4fbd8131d573fd4ee90a/Untitled.png)

- 基本流程
    - 核心的两个类/对象`DatagramSocket`和`DatagramPacket`
    - 建立发送端，接收端（没有服务端和客户端概念）
    - 发送数据前，建立数据包/报，`DatagramPacket`对象
    - 调用`DatagramSocket`的发送，接收方法
    - 关闭`DatagramPacket`

# Java中UDP网络编程应用案例分析

- 编写一个接收端A，和一个发送端B
- 接收端A在9999端口等待接收数据（receive）
- 发送端A向接收端B发送数据`“hello，明天吃火锅！”`
- 接收端B接收到发送端A发送的数据，回复`“好的，明天见”`，再退出
- 发送端接收回复的数据，再退出

```java
package com.netProgramming;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.net.SocketException;

/**
 * @author 欧欧
 * @version 1.0
 */
public class UDPReceiverA {
    public static void main(String[] args) throws IOException {
        //1. Create DatagramSocket, ready for receiving data from port 9999
        DatagramSocket socket = new DatagramSocket(9999);
        //2. Constructor DatagramPacket, (UDP - max size 64k)
        byte[] buf = new byte[1024];
        DatagramPacket datagramPacket = new DatagramPacket(buf, buf.length);
        //3. call receive
        System.out.println("Receiver A is waiting...");
        socket.receive(datagramPacket);

        //4. extract data from packet
        int length = datagramPacket.getLength(); // actual data length
        byte[] data = datagramPacket.getData();

        String s = new String(data, 0, length);
        System.out.println(s);

        byte[] reply = "好的, 明天见".getBytes();
        DatagramPacket replyPacket = new DatagramPacket(reply, reply.length, InetAddress.getByName("127.0.0.1"), 8888);
        socket.send(replyPacket);
        //5. Close
        socket.close();
        System.out.println("A exit");

    }
}
```

```java
package com.netProgramming;

import java.io.IOException;
import java.net.*;

/**
 * @author 欧欧
 * @version 1.0
 */
public class UDPSenderB {
    public static void main(String[] args) throws IOException {
        //1. create DatagramSocket
        DatagramSocket datagramSocket = new DatagramSocket(8888);

        //2. encapsulate data to DatagramPacket
        byte[] data = "hello, 明天吃火锅".getBytes();
        DatagramPacket datagramPacket = new DatagramPacket(data, data.length, InetAddress.getByName("127.0.0.1"), 9999);

        //3. send data
        datagramSocket.send(datagramPacket);

        // reeive data
        byte[] buf = new byte[1024];
        DatagramPacket receivePacket = new DatagramPacket(buf, buf.length);
        //3. call receive
        System.out.println("Receiver B is waiting...");
        datagramSocket.receive(receivePacket);

        //4. extract data from packet
        int length = receivePacket.getLength(); // actual data length
        byte[] receive = receivePacket.getData();

        String s = new String(receive, 0, length);
        System.out.println(s);

        //4. close
        datagramSocket.close();
        System.out.println("B exit");

    }
}
```