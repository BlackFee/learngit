[TOC]



# Linux

### Vim

命令模式：复制、粘贴、删除、查找，控制光标移动

输入模式：正常文本录入

末行模式:保存以及退出文档，以及设置编辑环境

> vim 文件名 ：进入命令模式，
>
> 按a、i、o等键进入输入模式，ESC退出到命令模式
>
> ：键进入末行模式，ESC退出到命令模式
>
> 末行与输入都是从命令模式进入

常用命令

表4-1                                                      命令模式中常用的命令

| 命令 | 作用                                                         |
| ---- | ------------------------------------------------------------ |
| dd   | 删除（剪切）光标所在整行                                     |
| 5dd  | 删除（剪切）从光标处开始的5行                                |
| yy   | 复制光标所在整行                                             |
| 5yy  | 复制从光标处开始的5行                                        |
| n    | 显示搜索命令定位到的下一个字符串                             |
| N    | 显示搜索命令定位到的上一个字符串                             |
| u    | 撤销上一步的操作                                             |
| p    | 将之前删除（dd）或复制（yy）过的数据粘贴到光标后面（光标下一行插入数据） |

表4-2                                                  末行模式中可用的命令

| 命令          | 作用                                 |
| ------------- | ------------------------------------ |
| :w            | 保存                                 |
| :q            | 退出                                 |
| :q!           | 强制退出（放弃对文档的修改内容）     |
| :wq!          | 强制保存退出                         |
| :set nu       | 显示行号                             |
| :set nonu     | 不显示行号                           |
| :命令         | 执行该命令                           |
| :整数         | 跳转到该行                           |
| :s/one/two    | 将当前光标所在行的第一个one替换成two |
| :s/one/two/g  | 将当前光标所在行的所有one替换成two   |
| :%s/one/two/g | 将全文中的所有one替换成two           |
| ?字符串       | 在文本中从下至上搜索该字符串         |
| /字符串       | 在文本中从上至下搜索该字符串         |

### vim练习

>1. 修改主机名称：vim /etc/hostname
>
>2. 配置网卡信息：
>
>   > ifconfig:确认网卡名称
>   >
>   > /etc/sysconfig/network-scripts
>   >
>   > ```shell
>   > [root@linuxprobe ~]# cd /etc/sysconfig/network-scripts/
>   > [root@linuxprobe network-scripts]# vim ifcfg-eno16777736
>   > TYPE=Ethernet    #设备类型
>   > BOOTPROTO=static  #地址分配模式
>   > NAME=eno16777736	#网卡名称
>   > ONBOOT=yes			#是否启动
>   > IPADDR=192.168.10.10	
>   > NETMASK=255.255.255.0
>   > GATEWAY=192.168.10.1
>   > DNS1=192.168.10.1
>   > ```

> 3. 搭建、配置yum仓库
>
>    > /etc/yum.repos.d/  :存放Yum软件仓库的配置文件
>    >
>    > 1、创建.repo配置文件，逐项写入如下参数：
>    >
>    > > **[rhel-media]** ：Yum软件仓库唯一标识符，避免与其他仓库冲突。
>    > >
>    > > **name=linuxprobe**：Yum软件仓库的名称描述，易于识别仓库用处。
>    > >
>    > > **baseurl=file:///media/cdrom**：提供的方式包括FTP（ftp://..）、HTTP（http://..）、本地（file:///..）。
>    > >
>    > > **enabled=1**：设置此源是否可用；1为可用，0为禁用。
>    > >
>    > > **gpgcheck=1**：设置此源是否校验文件；1为校验，0为不校验。
>    > >
>    > > **gpgkey=file:///media/cdrom/RPM-GPG-KEY-redhat-release**：若上面参数开启校验，那么请指定公钥文件地址.
>    > >
>    > > 实例：
>    > >
>    > > ```shell
>    > > [root@linuxprobe ~]# cd /etc/yum.repos.d/
>    > > [root@linuxprobe yum.repos.d]# vim rhel7.repo
>    > > [rhel7]
>    > > name=rhel7
>    > > baseurl=file:///media/cdrom
>    > > enabled=1
>    > > gpgcheck=0
>    > > ```
>    > >
>    > > 
>    >
>    > 2、按配置参数的路径挂载光盘，并把光盘挂载信息写入到/etc/fstab文件中。
>    >
>    > 3、使用“yum install httpd -y”命令检查Yum软件仓库是否已经可用。
>    >
>    > 待续。。。。。。。。。。。。。第四章

### 用户、文件权限

多任务、多用户操作系统

#### 用户

管理员、系统用户、普通用户  :UID不同 0  1-999  1000--

用户组 组ID  基本用户组 扩展用户组

useradd [选项] 用户名  :添加用户

groupadd [选项] 群组名 :添加用户组

usermod [选项] 用户名  :修改属性

passwd [选项] 用户名  :修改密码

userdel [选项] 用户名 :删除用户

#### 文件权限

文件类型 所有者 所有组 其他用户 权限rwx 421

![第5章 用户身份与文件权限。第5章 用户身份与文件权限。](https://www.linuxprobe.com/wp-content/uploads/2015/02/文件权限.png)

通过分析可知，该文件的类型为普通文件，所有者权限为可读、可写（rw-），所属组权限为可读（r--），除此以外的其他人也只有可读权限（r--），文件的磁盘占用大小是34298字节，最近一次的修改时间为4月2日的凌晨23分，文件的名称为install.log。

#### 特殊权限

SUID  SGID  SBIT

chmod chown  -R参数（目录内所有文件修改）

#### 隐藏属性

chattr:添加/删除隐藏属性  lsattr：显示隐藏属性

#### ACL

setfacl  getfacl

文件的权限最后一个点（**.**）变成了加号（**+**）,这就意味着该文件已经设置了ACL了

```shell
dr-xrwx---+
```

#### su命令 sudo服务

su -

### 存储结构

#### 物理设备命名

![第6章 存储结构与磁盘划分。第6章 存储结构与磁盘划分。](https://www.linuxprobe.com/wp-content/uploads/2015/02/硬盘命名规则.png)

首先，/dev/目录中保存的应当是硬件设备文件；其次，sd表示是存储设备；然后，a表示系统中同类接口中第一个被识别到的设备，最后，5表示这个设备是一个逻辑分区。一言以蔽之，“/dev/sda5”表示的就是“这是系统中第一块被识别到的硬件设备中分区编号为5的逻辑分区的设备文件”

![第6章 存储结构与磁盘划分。第6章 存储结构与磁盘划分。](https://www.linuxprobe.com/wp-content/uploads/2015/02/逻辑分区.png)



##### 挂载

用户在使用设备或者分区的数据时，需要先将其与本地已存在的文件进行关联，这个动作就叫挂载

mount 文件系统	挂载目录

**链接**

硬链接：相当于原始文件，原始文件删除之后，该链接还会指向该文件

软链接（符号链接）：类似快捷方式，源文件删除之后，该链接失效。

ln -s 目标	-s参数代表创建软链接，否则默认创建硬链接

#### 防火墙



策略：从上到下匹配规则。阻止/放行

规则链

> > 在进行路由选择前处理数据包（PREROUTING）；
> >
> > 处理流入的数据包（INPUT）；
> >
> > 处理流出的数据包（OUTPUT）；
> >
> > 处理转发的数据包（FORWARD）；
> >
> > 在进行路由选择后处理数据包（POSTROUTING）。

动作

> ACCEPT（允许流量通过）、REJECT（拒绝流量通过）、LOG（记录日志信息）、DROP（拒绝流量通过）
>
> DROP与reject的区别是drop丢弃不响应，而reject会响应
>
> > 设置两种策略后，发送方ping包结果:
> >
> > reject显示不可达，drop无法判断是访问拒绝还是主机不在线
> >
> > > ```shell
> > > [root@linuxprobe ~]# ping -c 4 192.168.10.10
> > > PING 192.168.10.10 (192.168.10.10) 56(84) bytes of data.
> > > From 192.168.10.10 icmp_seq=1 Destination Port Unreachable
> > > From 192.168.10.10 icmp_seq=2 Destination Port Unreachable
> > > From 192.168.10.10 icmp_seq=3 Destination Port Unreachable
> > > From 192.168.10.10 icmp_seq=4 Destination Port Unreachable
> > > --- 192.168.10.10 ping statistics ---
> > > 4 packets transmitted, 0 received, +4 errors, 100% packet loss, time 3002ms
> > > ```

> > > ```
> > > [root@linuxprobe ~]# ping -c 4 192.168.10.10
> > > PING 192.168.10.10 (192.168.10.10) 56(84) bytes of data.
> > > 
> > > --- 192.168.10.10 ping statistics ---
> > > 4 packets transmitted, 0 received, 100% packet loss, time 3000ms
> > > ```

Iptables ：需保存，否则重启后规则消失。  基于TCP/IP协议的流量过滤工具  

Firewalld:区域(zone)   基于TCP/IP协议的流量过滤工具

服务的访问控制列表：允许或禁止Linux系统**提供服务**的防火墙

## 常用命令



## 软件安装

`Tarball`  	`RPM` 	 `DPKG`

#### Tarball

##### 安装步骤

需要用到gcc和make软件

- 下载.tar.gz尾椎的压缩文件

  尾椎说明：tar代表以tar打包源代码文件

  gz代表gzip压缩技术。.tar.gz代表利用了tar与gzip功能，所以扩展名为`*.tar.gz`或缩写成`*.tgz`

  其他压缩格式`bzip2`  ` xz`对应文件名为`*tar.bz2` `*.tar.xz`

- 解压文件，文件内包含一下内容

  - 源代码文件
  - 检测程序文件（configure或者config等文件）
  - 简要说明与安装说明（install或README）

- 使用gcc软件编译源代码

  ```shell
  gcc -c  source1.c  		#会产生source1.o目标文件
  gcc -c  source2.c  		#会产生source2.o目标文件
  #.o文件为经过编译的目标文件
  ```

- 以gcc进行函数库、主、子程序的链接，以形成主要的二进制文件（可执行文件）

  ```shell
  gcc	-o main source1.o source2.0  	
  #将两个目标文件编译成最终的可执行文件
  gcc其他参数 
  —O		产生最佳化  
  —wall	产生详细的编译过程
  -lm 	加入libm.so函数库 m代表libm.so函数库
  -L/lib 	所加的函数库请到/lib文件里寻找
  ```

  之所以先要生成目标文件:

  - 第一是因为源代码文件并非只有一个
  - 第二是为了节省下次出现部分源代码修改而导致重新编译的时间，例如只有source1修改过，那么只需要重新编译source1文件即可

- 安装二进制文件与配置文件至自己主机上

##### make的使用

上述步骤中gcc的使用可以用make命令简化

1. ./configure

   生成makefile文件，文件格式如下

   ```shell
   目标(target):目标文件1  目标文件2
   <tab> gcc -o 欲建立的执行文件 目标文件1 目标文件2
   #可用#代表注释
   clean:
   	rm -f main source1.o source2.o
   ```

   使用命令时`make + 目标`就会执行目标clean

   ```shell
   ./configure --help #查看可用参数
   --prefix=/path  未来要安装的目录
   ```

   

2. make clean

   以clean这个目标来清楚目标文件，主要是清除可能存在的遗留目标文件

3. make 

   根据默认设置进行编译操作，主要是生成可执行文件

4. make install

   根据makefile中的install目标，将之前编译完成的内容安装到预定义的目录中

##### 软件删除

删除该安装目录

##### 更新

利用patch,更新源代码，重新编译

#### RPM

RedHat Package Manager

预先将安装的软件编译，并打包成RPM机制的文件

同时会记录安装需要的依赖性软件，安装时会查询主机是否满足条件，若满足才会予以安装

##### 注意

安装环境需要与打包时的环境一致

满足依赖性需求

反安装时需要小心底层软件（删除之后，是否影响系统）

##### SRPM

`*.src.rpm`扩展名

SRPM为源代码文件，还包括参数配置文件

RPM文件是已经在主机上编译过的安装文件

SRPM可以通过修改配置文件，重新编译生成适合我们Linux环境的RPM文件

##### 安装

软件管理程序：rpm

```shell
rpm -ivh
-i  安装的意思
-v	查看更详细的安装信息
-h	显示安装进度
--test 检查是否可以安装到当前环境中
--prefi 新路径
```

##### 升级

```shell
rpm -Uvh		后面的软件未安装，则会直接安装该软件
rpm -Fvh		后面的软件未安装，则该软件不会安装
```

##### 查询

其实查询的是/var/lib/rpm/目录下的数据库文件，同时也可以查询未安装的RPM文件

```shell
rpm -qa 列出所有已安装软件
rpm -q [软件名称]
-qc		列出配置文件
-qd		列出所有说明文件
-qi		列出软件详细信息
-ql		列出所有文件和目录所在完整文件名
-qR		列出依赖软件所含的文件
-qf     由后面接的文件名，列出该文件属于哪一个已安装的软件
```

##### 反安装

rpm -e 

rpm --rebuilddb			重建数据库

##### YUM

查询

```shell
yum serach 		#查找软件
search
list			#类似rpm -qa
info
provides  		#类似rpm -qf
```

安装/升级

```shell
yun install
yum update
```

删除

```shell
yum remove 
```

yum配置文件

`/etc/yum.repos.d/*.repo` 注意必须是`repo`尾椎的文件

该文件内存有yum服务器的地址

yum repolist all 

列出目前所使用的软件源

yum clean [packages|headers|all]

packages:将一下载的安装文件删除

all:将所有软件源数据都删除，包括软件本身。

**使用本机安装光盘**

假设光盘挂载在/mnt目录下，则可将repo文件改成如下格式

```shell
[mycdrom]
baseurl = file:///mnt
gpgcheck = 0
enabled = 0
```

安装：yum  --enablerepo=mycdrom  installl 软件名

#### dpkg

发行版代表：Debain/Ubuntu

软件管理机制：DPKG

使用命令:dpkg

在线升级功能：APT （apt-get）



dpkg --list 

查看已安装包的列表

sudo apt-get  --purge  remove softname

卸载softname并删除所有的配置文件(--purge代表删除配置文件)

dpkg --configure -a