Packet Factory
==============

Packet Factory is used by the Pcap4J core module to create packet objects from captured packets (byte arrays).

Packet Factory被Pcap4J core模块用来从被捕捉到的包（字节数组）来创建包对象。



Packet Factory is pluggable. This pluggability is made by the [Packet Factory Binder](#packet-factory-binder).

Packet Factory是可扩展的。扩展能力来自[Packet Factory Binder](#packet-factory-binder).



Pcap4J has two Packet Factory modules, [Static Packet Factory](#static-packet-factory) and [Properties-Based Packet Factory](#properties-based-packet-factory).

Pcap4J有两种Packet Factory模块，[Static Packet Factory](#static-packet-factory) 和 [Properties-Based Packet Factory](#properties-based-packet-factory).



### Packet Factory Binder ###
Packet Factory Binder binds Packet Factory implementations to the Pcap4J core module.

Packet Factory Binder将Packet Factory实现绑定到Pcap4J核心模块。

Pcap4J core module's source includes [the interface of Packet Factory Binder](https://github.com/kaitoy/pcap4j/blob/v1/pcap4j-core/src/main/java/org/pcap4j/packet/factory/PacketFactoryBinder.java).

Pcap4J核心模块的源代码包括[Packet Factory Binder的接口](https://github.com/kaitoy/pcap4j/blob/v1/pcap4j-core/src/main/java/org/pcap4j/packet/factory/PacketFactoryBinder.java).

An implementation of Packet Factory Binder is included in a Packet Factory module which also has Packet Factory implementations.

Packet Factory Binder的实现被包括在Packet Factory模块中，该模块也具有Packet Factory实现。--- Packet Factory有Packet Factory Binder和Packet Factory的实现。

A Packet Factory implementation is used to find a packet class (e.g. [IpV4Packet](https://github.com/kaitoy/pcap4j/blob/v1/pcap4j-core/src/main/java/org/pcap4j/packet/IpV4Packet.java))
or a packet piece class (e.g. [IpV4Rfc1349Tos](https://github.com/kaitoy/pcap4j/blob/v1/pcap4j-core/src/main/java/org/pcap4j/packet/IpV4Rfc1349Tos.java))
by a classifier (e.g. [EtherType](https://github.com/kaitoy/pcap4j/blob/v1/pcap4j-core/src/main/java/org/pcap4j/packet/namednumber/EtherType.java))
and to instantiate its object.

Packet Factory的实现是用来发现一个包类(e.g. [IpV4Packet](https://github.com/kaitoy/pcap4j/blob/v1/pcap4j-core/src/main/java/org/pcap4j/packet/IpV4Packet.java))，
或一个类(e.g. [EtherType](https://github.com/kaitoy/pcap4j/blob/v1/pcap4j-core/src/main/java/org/pcap4j/packet/namednumber/EtherType.java))的细分类，并实例化一个对象。


### Static Packet Factory ###
Static Packet Factory is a Packet Factory module including Packet Factory implementations which find packet and packet piece classes in static way,
which means you can't replace these classes without code changes.
This Packet Factory doesn't use Java reflection and so relatively faster than [Properties-Based Packet Factory](#properties-based-packet-factory).

Static Packet Factory是一个Packet Factory模块，包含Packet Factory的实现，能以静态方式查找数据包，这意味着如果不更改代码，就无法替换这些类。
这个Packet Factory不使用Java反射，因此比[Properties-Based Packet Factory]（#Properties-Based Packet Factory）更快。


<img alt="Static Packet Factory" title="Static Packet Factory" src="https://github.com/kaitoy/pcap4j/raw/v1/www/images/staticPacketFactory.png" />

### Properties-Based Packet Factory ###
Properties-Based Packet Factory is a Packet Factory module including Packet Factory implementations which find packet and packet piece classes by Java properties.
This Packet Factory heavily uses Java reflection and so relatively slower than [Static Packet Factory](#static-packet-factory).

基于属性的Packet Factory是一个Packet Factory模块，包括通过Java属性查找数据包和数据包片段类的数据包工厂实现。
这个Packet Factory大量使用Java反射，因此比[静态包工厂]（#静态包工厂）相对较慢。


<img alt="Properties-Based Packet Factory" title="Properties-Based Packet Factory" src="https://github.com/kaitoy/pcap4j/raw/v1/www/images/propertiesBasedPacketFactory.png" />
