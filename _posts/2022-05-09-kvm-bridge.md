---
layout: post
title: kvm设置网络桥接模式
subtitle: kvm设置
gh-repo: daattali/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [kvm, vm]
comments: true
---

# kvm设置网络桥接模式

kvm设置网络时，对于虚拟机的网络连接有两种常用方式，一种是NAT，一种是桥接。

NAT(网络地址转换)， Network Access Translation，使用NAT技术的局域网对外网的表现是一个IP地址A，但是在该局域网内部则使用同一个网段的不同IP，这些IP对外网访问，全部使用地址A，反过来外网对这些IP的访问，通过访问"A：端口"来映射。在创建虚拟机时，如果采用NAT方法，一般虚拟机会获得一个192.168.x.x的IP，该IP只有从host或者从该host上的其他虚拟机才能访问。

桥接方式则会让被创建的虚拟机获得与host一样的网段的IP, 因此表现得就像一台与host同在一个网络中的物理机一样。

创建KVM的xml文件中，采用NAT模式的设置一般如下:

```
<domain type="kvm">
  <devices>
	……
    <interface type="bridge">
      <source bridge="virbr0"/>
      <mac address="00:10:23:91:15:11"/>
      <model type="virtio"/>
    </interface>
    <hostdev mode='subsystem' type='pci' managed='yes'>
      <source>
        <address domain='0x0000' bus='0x18' slot='0x02' function='0x0'/>
      </source>
    </hostdev>
   <serial type="pty">
      <target port="0"/>
    </serial>
    <console type="pty">
      <target type="serial" port="0"/>
    </console>
  </devices>
</domain>

```

在interface属性下，type为"bridge",source bridge则是“virbr0”。mac address是v虚拟机中虚拟网卡(不是virbr0)中的mac,它只需要在这个局域网中保证唯一即可。

virbr0由kvm默认创建，它默认分配了一个IP 192.168.122.1，连接到该网桥上的虚拟网卡会被virbr0分配一个IP 。

## [理解 virbr0](https://www.cnblogs.com/zhaohongtian/p/6811317.html)

对虚拟机使用桥接连接方式，需要将xml中的“virbr0”修改为“br0”。

其次，修改host中的br0和网卡配置。以redhat举例，使用桥接方式之前，host上的br0和网卡地址是这样的：

```
br0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.1  netmask 255.255.255.0  broadcast 192.168.0.255
        inet6 fe80::20e:c6ff:fe67:74ec  prefixlen 64  scopeid 0x20<link>
        ether 00:0e:c6:67:74:ec  txqueuelen 1000  (Ethernet)
        RX packets 8396296  bytes 11827862282 (11.0 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 4642273  bytes 318891030 (304.1 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp0s20f0u1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.239.183.59  netmask 255.255.254.0  broadcast 10.239.183.255
        ether 00:0e:c6:67:74:ec  txqueuelen 1000  (Ethernet)
        RX packets 8398665  bytes 11845477389 (11.0 GiB)
        RX errors 0  dropped 425  overruns 0  frame 0
        TX packets 4645823  bytes 356244128 (339.7 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

在`/etc/sysconfig/network-scripts/`文件夹下，有`ifcfg-br0, ifcfg-enp0s20f0u1`两个配置文件，将它们修改为如下：

```
# ifcfg-br0
DEVICE="br0"
ONBOOT="yes"
TYPE="Bridge"
BOOTPROTO="dhcp"


# ifcfg-enp0s20f0u1`
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=dhcp
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
NAME=enp0s20f0u1
DEVICE=enp0s20f0u1
ONBOOT=yes
BRIDGE=br0 #主要是加上这句
```

并且执行`systemctl restart NetworkManager;nmcli connection up enp0s20f0u1`, 之后再把br0的192.168.0.1地址移除：

```
ip address del dev br0 192.168.0.1/24
```

再次ifconfig, 得到如下结果:

```
br0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.239.183.59  netmask 255.255.254.0  broadcast 10.239.183.255
        inet6 fe80::20e:c6ff:fe67:74ec  prefixlen 64  scopeid 0x20<link>
        ether 00:0e:c6:67:74:ec  txqueuelen 1000  (Ethernet)
        RX packets 8407070  bytes 11828720576 (11.0 GiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 4650038  bytes 319945070 (305.1 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

enp0s20f0u1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        ether 00:0e:c6:67:74:ec  txqueuelen 1000  (Ethernet)
        RX packets 8409506  bytes 11846391933 (11.0 GiB)
        RX errors 0  dropped 435  overruns 0  frame 0
        TX packets 4653593  bytes 357360918 (340.8 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

可以看到，ip地址从enp0s20f0u1移动到了br0上。这代表kvm可以成功设置桥接模式。

