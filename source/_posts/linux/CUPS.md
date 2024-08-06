---
title: Linux Cups 实战
date: 2024-08-06 17:24:31
tags:
---

本文以Rocky Linux 8.9 (Green Obsidian)为例<br>
使用的身份均为root，并且没有启用防火墙。<br>
<br>


1. 安装cups

```shell
sudo yum install cups
sudo yum install foomatic-filters
``` 

2. 配置 cups

```shell
sudo nano /etc/cups/cupsd.conf
```

**配置监听的ip地址和端口<br>**

将Listen localhost:631修改为服务器分配的IP地址，如果想设置全部IP都监听，则修改为Listen 0.0.0.0:631<br>

其中631是CUPS默认的监听端口，如果有使用IPv6的需求，则需要额外添加Listen [::]:631，这里的::表示全部IPv6地址，请根据实际情况修改。

```shell
# Only listen for connections from the local machine.
Listen 10.32.7.38:631
Listen localhost:631
Listen /run/cups/cups.sock
```

**配置 cups 允许访问的IP地址<br>**

格式是Allow from ip/subnetmask，如允许192.168.1.0、子网掩码是255.255.255.0的网段时，则Allow from 192.168.1.0/24，IPv6的地址需要额外加上[]，如Allow
from [FDEE::]/7。

```shell
# Restrict access to the server...
<Location />
  Allow from 10.32.96.241/24
  Order allow,deny
</Location>
```

如果允许全部主机访问，则直接添加Allow all即可，如下

```shell
# Restrict access to the server...
<Location />
  Allow all
  Order allow,deny
</Location>
```

**配置允许访问管理页面的ip地址范围**
格式与上述一致

```shell
# Restrict access to the admin pages...
<Location /admin>
  Allow from 10.32.105.40/24
  Order allow,deny
</Location>
```

3. 启动cups

```shell
systemctl start cups
```

4. 添加打印机以得实针式打印机为例

- 打印机管理页面
  ![](管理打印机.png)
- 添加打印机
  ![](添加打印机.png)
  ![](添加打印机2.png)
  ![](添加打印机3.png)
  配置驱动，手动添加PDD驱动，配置完成后，点击Add Printer
  ![](添加打印机4.png)
- 设置打印机首选项
  ![](打印机首选项.png)

### 问题汇总

1. 添加打印机后,cups4j 无法获取到打印机信息

```text
 查看添加打印机时是否共享该打印机,如果没有共享,cups4j是拿不到该打印机的
```