# Ubuntu 配置

## 1、ssh 配置

**检查ssh 服务是否已经启动**

```
$ ps -e | grep ssh

  7529 ?        00:00:00 sshd
  7852 pts/1    00:00:00 ssh
```

若输入指令后显示类似于上图所示，则说明SSH服务已启动

其中sshd表示ssh-server已启动，ssh表示ssh-client已启动



**安装ssh 服务**

```
sudo apt-get install openssh-client
sudo apt-get install openssh-server
```

**启动SSH服务**

```
sudo /etc/init.d/ssh start
```

启动后通过以下指令判断SSH服务是否正确启动：

```
ps -e | grep ssh
```

接下来可以使用远程登录到 Ubuntu 了；



## 2、网络配置

vmvare 下

桥接：和主机处于同一网段，同一地位下；同网段下的其他设备也可以连接到该虚拟机中；



## 3、国内源更新

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
```

在/etc/apt/sources.list文件前面添加如下条目

#添加阿里源

```
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
```

