## Linux 容易忘记的基本命令总结

//：注释

6：CentOS 6 或 Red Hat 6

7：CentOS 7

### 1、网络配置

目录： /etc/sysconfig/network-scripts/
文件： ifcfg-ens33（一般叫这个名）

#### 1.1 打开文件

```bash
vim /etc/sysconfig/network-scripts/ifcfg-ens33
```

#### 1.2 编辑内容

> 内容如下：
>
> TYPE=Ethernet
> PROXY_METHOD=none
> BROWSER_ONLY=no
> BOOTPROTO=dhcp
> DEFROUTE=yes
> IPV4_FAILURE_FATAL=no
> IPV6INIT=yes
> IPV6_AUTOCONF=yes
> IPV6_DEFROUTE=yes
> IPV6_FAILURE_FATAL=no
> IPV6_ADDR_GEN_MODE=stable-privacy
> NAME=ens33
> UUID=6b3715cd-c862-4a56-99a4-a041a1206af8
> DEVICE=ens33
> ONBOOT=yes
> #BOOTPROTO=static		//静态ip
> #IPADDR=			    //ip地址
> #NETMASK=255.255.255.0	//子网掩码
> #GATEWAY=				//网关
> #DNS1=119.29.29.29
> #DNS2=8.8.8.8      

#### 1.3 保存并退出

```bash
esc
:wq
```

#### 1.4. 重启网络

```bash
systemctl restart network
或者
service network restart （Linux更早版本）
```

### 2、查看IP地址

```bash
ifconfig
ip addr (如果上面那个指令报错)
```

### 3、修改的文件即时生效

```bash
# source 或者 .
例如：
source /etc/profile
. /etc/profile
```

### 4、防火墙状态查看及起停

4.1 查看防火墙状态

```bash
chkconfig iptables --list //6及之前
service iptables status //或者
irewall-cmd --state	//7
```

4.2 开启关闭（即时生效，重启后回复）

```bash
service iptables start //开启（6及之前）
service iptables stop //关闭（6及之前）
systemctl start firewalld.service //开启起（7）
systemctl stop firewalld.service //关闭（7）
```

4.3 开启关闭（永久生效，重启后不复原）

```bash
chkconfig iptables on //开启（6及之前）
chkconfig iptables off //关闭（6及之前）
systemctl enable firewalld.service //开启起（7）
systemctl disable firewalld.service //关闭（7）
```

### 5、端口号相关 netstat命令

> 常见参数
>
> -a (all)显示所有选项，默认不显示LISTEN相关
> -t (tcp)仅显示tcp相关选项
> -u (udp)仅显示udp相关选项
> -n 拒绝显示别名，能显示数字的全部转化成数字。
> -l 仅列出有在 Listen (监听) 的f服务状态
> -p 显示建立相关链接的程序名
> -r 显示路由信息，路由表
> -e 显示扩展信息，例如uid等
> -s 按各个协议进行统计
> -c 每隔一个固定时间，执行该netstat命令。
>
> 提示：LISTEN和LISTENING的状态只有用-a或者-l才能看到 

#### 5.1 查看当前所有已使用端口情况

```bash
netstat -nultp
```

#### 5.2查看固定端口相关信息

```bash
netstat -anp | grep 3306
```






















































































































































