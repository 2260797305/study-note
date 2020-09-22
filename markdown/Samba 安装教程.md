# Samba 安装教程

## 1、简易配置方法

### 1、安装samba：

对于一个全新的Ubuntu

```
sudo apt install samba
```

### 2、选择需要共享的目录

```
/home/yuki/work  ---yuki 为用户name
```

### 3、设置samba 用户

```
smbpasswd -a yuki:  设置yuki为samba 用户，并设置密码，可以与用户密码不一样。
```

### 4、修改配置文件

```
sudo gedit /etc/samba/smb.conf
```

在最后添加：

```
[work]
	comment = Share Folder require password
	browseable = yes
	path = /home/k-yuki/work
	valid users = k-yuki
	admin users  = k-yuki  
	public = yes
	writable = yes
	available = yes 
```

### 5、检查config 语法是否正确。

```
testparm
```

### 6、重启samba

```
sudo /etc/init.d/smbd restart  
```

### 7、windows 访问

​	在Windows 上：
​	win + R打开“运行”。输入Ubuntu ip： \\\192.168.31.150
​	打开的目录中会有一个work的文件夹。
​	打开 输入用户名： yuki，密码：之前设置的samba密码，不一定是用户密码。

### 8、可能问题及解决方法

如果出现访问被拒绝的情况，尝试以下方法。

**a:**

​	windows下： win + R ,输入“control userpasswords2”，打开“用户账户”；
选择高级选项卡，选择密码管理；
点击windows凭据，添加windows凭据；
添加地址：samba 地址，密码为samba密码。

**b**

​	Linux 端： 安装daemon

```
sudo apt-get install  daemon
```



## 2、无需密码登录方法

使用方法1时，有时候会出现始终登录失败，访问拒绝 ，密码账号错误等鬼名鬼眼的错误；建议使用以下方法；

在1 的基础上：

`sudo gedit /etc/samba/smb.conf`

**[global]** 下面加入：

```
security = user
map to guest = bad user 
```

最后配置共享目录处，注释掉 	`valid users` 与  `adminusers` 两项：

```
[linux]
        comment = Share Folder require password
        browseable = yes
        path = /home/k-yuki/linux
        #valid users = k-yuki
        #admin users  = k-yuki
        public = yes
        writable = yes
        available = yes
```

然后就可以无密码登录了；

# FUCK！！！！！！

## 1、安装

Samba是一套程序，其中最重要的两个是：

**smbd：**提供SMB / CIFS服务（文件共享和打印），也可以作为Windows域控制器。

**nmbd：**提供NetBIOS名称服务

安装命令：

```
sudo apt install samba
```

查看samba 版本：

```
sudo smbstatus  或者sudo smbd --version
```

要检查Samba服务是否正在运行，请运行以下命令。

```
systemctl status smbd
systemctl status nmbd
```

要启动这两个服务，请运行以下命令：

```
sudo systemctl start smbd
sudo systemctl start nmbd
```



### 2、编辑配置文件

只有一个配置文件需要处理：/etc/samba/smb.conf。

```
sudo vim /etc/samba/smb.conf
```

加入:  

```
usershare owner only = false
```

重启:  

```
sudo /etc/init.d/samba restart
```



