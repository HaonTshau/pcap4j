How to add a protocol support  怎么添加一个协议
=============================

1. Write your packet class<br>
  Firstly, you need to write packet classes which represent packets used in the protocol you want to add.
  A packet class must implement [org.pcap4j.packet.Packet interface](https://github.com/kaitoy/pcap4j/blob/v1/pcap4j-core/src/main/java/org/pcap4j/packet/Packet.java).
  Actually, in most cases, you should extend [org.pcap4j.packet.AbstractPacket class](https://github.com/kaitoy/pcap4j/blob/v1/pcap4j-core/src/main/java/org/pcap4j/packet/AbstractPacket.java),
  which is an abstract class implementing many of Packet's methods so you can save time.

  And, it's recommended to write a static factory method in the packet class such as below:

  ```java
  public static YourPacket newPacket(byte[] rawData, int offset, int length) {
    return new YourPacket(rawData);
  }
  ```


  With this static factory method, you can use [the Properties-Based Packet Factory](https://github.com/kaitoy/pcap4j/blob/v1/www/PacketFactory.md#properties-based-packet-factory) for the packet class.

  To write a packet class, you need to write also a header class and a builder class.
  The header class must implements org.pcap4j.packet.Packet.Header or extends org.pcap4j.packet.AbstractPacket.AbstractHeader.
  The builder class must implements org.pcap4j.packet.Packet.Builder or extends org.pcap4j.packet.AbstractPacket.AbstractBuilder.

  The responsibilities of a packet class are the following:
  * Building its header object in the constructor (if it has a header).
  * Building its payload object in the constructor (if it has a payload).
  * Building its builder object in the getBuilder method.



1. 编写你的包类<br>
首先，您需要编写要添加的协议中使用的数据包的数据包类。
数据包类必须实现[org.pcap4j.packet.packet interface](https://github.com/HaonTshau/pcap4j/blob/v1/pcap4j-core/src/main/java/org/pcap4j/packet/Packet.java).
实际上，在大多数情况下，您应该扩展[org.pcap4j.packet.AbstractPacket class](https://github.com/HaonTshau/pcap4j/blob/v1/pcap4j-core/src/main/java/org/pcap4j/packet/AbstractPacket.java),这是一个抽象类，实现了包的许多方法，因此可以节省时间。
建议在packet类中编写一个静态工厂方法，如下所示：
  ```java
  public static YourPacket newPacket(byte[] rawData, int offset, int length) {
    return new YourPacket(rawData);
  }
  ```
通过这种静态工厂方法，您可以使用[the Properties-Based Packet Factory](https://github.com/kaitoy/pcap4j/blob/v1/www/PacketFactory.md#properties-based-packet-factory)，用于packet class。
要编写packet class，还需要编header类和builder类。header类必须实现org.pcap4j.packet.Packet.Header 或 extends org.pcap4j.packet.AbstractPacket.AbstractHeader。
builder类必须实现org.pcap4j.packet.Packet.Builder 或 extends org.pcap4j.packet.AbstractPacket.AbstractBuilder。
packet class的职责如下：

* 在构造函数中构建其header对象（如果它有头）。

* 在构造函数中构建其有效载荷对象（如果它有有效负载）。

* 在getBuilder方法中生成其builder对象。


2. Configure Packet Factory  配置Packet Factory<br> 
  Secondly, you need to configure Packet Factory so it properly instantiates packet objects from your packet classes.
  Assuming your packet class represents a protocol over TCP and the protocol uses port 1234,
  there are three ways to configure Packet Factory.
  
其次，您需要配置Packet Factory，以便它正确地从你的packet类实例化packet对象。

假设您的packet class表示TCP上的协议，并且该协议使用端口1234，有三种方法可以配置Packet Factory。
  

  2.1. Using [the Properties-Based Packet Factory](https://github.com/HaonTshau/pcap4j/blob/v1/www/PacketFactory.md#properties-based-packet-factory)<br>
  Add the following line to `jar:file:/path/to/pcap4j-packetfactory-propertiesbased.jar!/org/pcap4j/packet/factory/packet-factory.properties`:

  ```org.pcap4j.packet.Packet.classFor.org.pcap4j.packet.namednumber.TcpPort.1234 = org.pcap4j.packet.YourPacket```

  Or, if you don't want to modify packet-factory.properties in the jar, add the following system property before starting the first packet capture:

  ```org.pcap4j.packet.Packet.classFor.org.pcap4j.packet.namednumber.TcpPort.1234 = org.pcap4j.packet.YourPacket```

  Note system properties always take precedence over properties in packet-factory.properties.

  If you want to use your own properties file, set the system property `org.pcap4j.packet.factory.properties` to the path to your properties file.
  This makes the Properties-Based Packet Factory load the properties file using `java.lang.ClassLoader#getResourceAsStream` method and
  use it instead of packet-factory.properties in Properties-Based Packet Factory module.


  2.1. 使用[the Properties-Based Packet Factory](https://github.com/HaonTshau/pcap4j/blob/v1/www/PacketFactory.md#properties-based-packet-factory)<br>
       在`jar:file:/path/to/pcap4j-packetfactory-propertiesbased.jar!/org/pcap4j/packet/factory/packet-factory.properties`添加以下行：
```org.pcap4j.packet.Packet.classFor.org.pcap4j.packet.namednumber.TcpPort.1234 = org.pcap4j.packet.YourPacket```
或者，如果您不想修改jar包中的packet-factory.properties，在开始第一次数据包捕获之前添加以下系统属性：
```org.pcap4j.packet.Packet.classFor.org.pcap4j.packet.namednumber.TcpPort.1234 = org.pcap4j.packet.YourPacket```
注意，系统的properties优先于packet-factory.properties。
如何你想用自己的properties文件，请设置系统属性`org.pcap4j.packet.factory.properties`到你的properties文件路径，这使得基于属性的Packet Factory使用`java.lang.ClassLoader#getResourceAsStream`方法加载和使用properties文件，而不是使用Properties-Based中内置的properties



  2.2. Using [Static Packet Factory](https://github.com/HaonTshau/pcap4j/blob/v1/www/PacketFactory.md#static-packet-factory)<br>
  Modify the source [StaticTcpPortPacketFactory.java](https://github.com/HaonTshau/pcap4j/blob/v1/pcap4j-packetfactory-static/src/main/java/org/pcap4j/packet/factory/statik/StaticTcpPortPacketFactory.java)
  and add the following to its constructor:

  ```java
  instantiaters.put(
    TcpPort.getInstance((short)1234),
    new PacketInstantiater(byte[] rawData, int offset, int length) throws IllegalRawDataException {
      return YourPacket.newPacket(rawData, offset, length);
    }
  );
  ```
   Then, build the Static Packet Factory and use the new module.
   
  2.2. Using [Static Packet Factory](https://github.com/HaonTshau/pcap4j/blob/v1/www/PacketFactory.md#static-packet-factory)<br>
修改源代码[staticTcportPacketFactory.java](https://github.com/HaonTshau/pcap4j/blob/v1/pcap4j-packetfactory-static/src/main/java/org/pcap4j/packet/factory/statik/StaticTcpPortPacketFactory.java)
添加如下构造函数
  ```java
  instantiaters.put(
    TcpPort.getInstance((short)1234),
    new PacketInstantiater(byte[] rawData, int offset, int length) throws IllegalRawDataException {
      return YourPacket.newPacket(rawData, offset, length);
    }
  );
  ```
然后，构建Static Packet Factory并使用新模块


  2.3. Create your own Packet Factory module<br>
  To be written.
