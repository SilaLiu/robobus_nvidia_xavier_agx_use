

问题由来：印度robobus

描述：

工控机IP：192.168.4.88

Nvidia Master: 网卡a:192.168.4.103  网卡b：192.168.44.100

Nvidia Desktop:192.168.44.101

工控机与Nvidia主机都接入了交换机，Nvidia 从机没有接入；但是Nvidia 主机与从机之间是有内部网段进行通信的。入下图（1-1）所示：

![](https://tcs.teambition.net/storage/312zb89c757538307993e3b2e1c615a9f59b?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IjU3YWFjMjdmMGViOGFlNjYxZDc3YWVjZiIsImV4cCI6MTY5NzYzMDM4NywiaWF0IjoxNjk3NjI2Nzg3LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMnpiODljNzU3NTM4MzA3OTkzZTNiMmUxYzYxNWE5ZjU5YiJ9.tc4gCBR_a2Mg96cijjMMHhxQGHKhgFL1Kcq5PvV57CI&download=image.png "")

图（1-1）

问题：如何通过工控机192.168.4.88去访问Nvidia Desktop:192.168.44.101 ？

![](https://tcs.teambition.net/storage/312z1d2afe6b5b330218db53ef083cb8418d?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IjU3YWFjMjdmMGViOGFlNjYxZDc3YWVjZiIsImV4cCI6MTY5NzYyOTQ0OCwiaWF0IjoxNjk3NjI1ODQ4LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMnoxZDJhZmU2YjViMzMwMjE4ZGI1M2VmMDgzY2I4NDE4ZCJ9.E0f14gdbSCHhHAZBGzrfm117964QItGlZhWMMxJ1BzI&download=image.png "")

解决方法：

step1.在Nvidia Master上执行命令，启用IP转发功能

```text
sudo sysctl -w net.ipv4.ip_forward=1
```



![](https://tcs.teambition.net/storage/312z543eab9583a7c36f452c7988b7c0d677?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IjU3YWFjMjdmMGViOGFlNjYxZDc3YWVjZiIsImV4cCI6MTY5NzYyOTg4NywiaWF0IjoxNjk3NjI2Mjg3LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMno1NDNlYWI5NTgzYTdjMzZmNDUyYzc5ODhiN2MwZDY3NyJ9.L4VrkjvGWs0s5I8OfJ8X4p1CONnhEinbwDcy8I3FRVc&download=image.png "")



step2.在Nvidia Master上添加静态路由

```text
iptables -t nat -A POSTROUTING -s 192.168.4.88/32 -j SNAT -- to 192.168.44.100
```

![](https://tcs.teambition.net/storage/312z3f901b5177f7fb300e1915d98cc28b31?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IjU3YWFjMjdmMGViOGFlNjYxZDc3YWVjZiIsImV4cCI6MTY5NzYyOTkwOSwiaWF0IjoxNjk3NjI2MzA5LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMnozZjkwMWI1MTc3ZjdmYjMwMGUxOTE1ZDk4Y2MyOGIzMSJ9.ppPfJgpRgKQrWHXAOIVSsXGL62d5zmAYhIHUhrpbh4M&download=image.png "")



step3.配置工控机，使用Nvidia Master的IP作为默认网关

```text
sudo ip route add 192.168.44.101 via 192.168.4.103 dev enp4s0
```

![](https://tcs.teambition.net/storage/312z0b2d22d985b6456e99cec3e87a095de3?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IjU3YWFjMjdmMGViOGFlNjYxZDc3YWVjZiIsImV4cCI6MTY5NzYzMDE2OCwiaWF0IjoxNjk3NjI2NTY4LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMnowYjJkMjJkOTg1YjY0NTZlOTljZWMzZTg3YTA5NWRlMyJ9.eL_BdBUpisC8hmfkxO39sFI8_TuXFcR7b3scsbC2Mfk&download=image.png "")



step4.查看一下工控机下的IP路由列表

```text
ip route
```

![](https://tcs.teambition.net/storage/312z7bb7b0a31515923337baa9c6bb5e25ea?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IjU3YWFjMjdmMGViOGFlNjYxZDc3YWVjZiIsImV4cCI6MTY5NzYzMTE2MSwiaWF0IjoxNjk3NjI3NTYxLCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMno3YmI3YjBhMzE1MTU5MjMzMzdiYWE5YzZiYjVlMjVlYSJ9.08yYFJos6DcnacYNCjemuUdMXHvHpiWhZy7e7W1WN4k&download=image.png "")

如出现以上信息，说明路由转发功能已做好，ping 一下看看。

![](https://tcs.teambition.net/storage/312z174abfe83f240de6a9116c1c210238ff?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IjU3YWFjMjdmMGViOGFlNjYxZDc3YWVjZiIsImV4cCI6MTY5NzYzMTAxMCwiaWF0IjoxNjk3NjI3NDEwLCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMnoxNzRhYmZlODNmMjQwZGU2YTkxMTZjMWMyMTAyMzhmZiJ9.wvewOfmD1mekSZVbdslXyMmrZVPkCxoIZvc_rVQ2KxI&download=telegram-cloud-photo-size-5-6159097817201752436-x.jpg "")



设置前：

![](https://tcs.teambition.net/storage/312z9ec2824b3e08c5840cd4827ab5bdd717?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IjU3YWFjMjdmMGViOGFlNjYxZDc3YWVjZiIsImV4cCI6MTY5NzYzMDIwNywiaWF0IjoxNjk3NjI2NjA3LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMno5ZWMyODI0YjNlMDhjNTg0MGNkNDgyN2FiNWJkZDcxNyJ9.nFKTvXALR3R4upGMCg70oLXFOVLy36spn3dVp2N8Eug&download=image.png "")

设置后：

![](https://tcs.teambition.net/storage/312z47472775838e903aa34946d522734288?Signature=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBcHBJRCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9hcHBJZCI6IjU5Mzc3MGZmODM5NjMyMDAyZTAzNThmMSIsIl9vcmdhbml6YXRpb25JZCI6IjU3YWFjMjdmMGViOGFlNjYxZDc3YWVjZiIsImV4cCI6MTY5NzYzMDE4NywiaWF0IjoxNjk3NjI2NTg3LCJyZXNvdXJjZSI6Ii9zdG9yYWdlLzMxMno0NzQ3Mjc3NTgzOGU5MDNhYTM0OTQ2ZDUyMjczNDI4OCJ9.VT-cQ9CU-FNe2ZnCDVFE6WfPVpFlnaeqpU8gmFq4A68&download=image.png "")




iptables 是一个用于配置 Linux 内核防火墙规则的工具，但它也可以用于实现网络路由转发。路由转发是将数据包从一个网络接口接收并将其转发到另一个网络接口的过程，通常用于连接不同的网络或子网。以下是如何使用 iptables 来进行路由转发的基本步骤：

注意：在进行路由转发之前，请确保已启用 IP 转发。

启用 IP 转发：

在 /etc/sysctl.conf 中，找到以下行并确保它们设置为 1：

shell
Copy code
net.ipv4.ip_forward = 1
net.ipv6.conf.all.forwarding = 1
然后，运行以下命令以重新加载配置：

shell
Copy code
sysctl -p
使用 iptables 设置路由规则：

假设你有两个网络接口：eth0 和 eth1，并且你希望从 eth0 接口接收的数据包被转发到 eth1 接口。以下是一个示例 iptables 命令：

shell
Copy code
iptables -t nat -A POSTROUTING -o eth1 -j MASQUERADE
iptables -A FORWARD -i eth0 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT
iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT
这些规则的作用是：

第一行：对从 eth1 发出的数据包进行源地址转换，以便它们看起来像是从路由器发送的。
第二行：允许从 eth0 到 eth1 的数据包传递，只要它们与已建立的或相关的连接有关。
第三行：允许从 eth1 到 eth0 的数据包传递。
这些规则是基本的，你可能需要根据你的网络拓扑和需求进行更多的配置。

调整路由表：

使用 ip route 命令来设置路由表，以确保数据包能够正确路由到目标网络。

例如，要将所有流向 192.168.1.0/24 子网的数据包路由到 eth1 接口，可以运行：

shell
Copy code
ip route add 192.168.1.0/24 dev eth1
这些步骤涵盖了如何使用 iptables 进行基本的路由转发。根据你的网络配置和需求，你可能需要进行更多的规则和路由表设置。确保在配置 iptables 规则和路由表时小心谨慎，以确保网络安全和性能。

