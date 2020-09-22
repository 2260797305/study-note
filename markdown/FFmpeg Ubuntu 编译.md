# FFmpeg Ubuntu 编译

## 1、glib, pcre 安装

```
sudo apt-get install build-essential
./configure --enable-utf8 --enable-unicode-properties
make
make install
```

## 2、cmake: not found

```
sudo apt-get install cmake
```

## 3、configure: error: Your intltool is too old.  You need intltool 0.35.0 or later.

```
sudo apt install libtool-bin
```

### 4、configure: error: Could not find bison

```
sudo apt-get install bison
```

### 5、configure: error: Could not find flex

```
sudo apt-get install flex
```

