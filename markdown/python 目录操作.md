```

```

# python 目录文件操作

## 目录操作

```
os.getcwd() #得到当前工作目录，即当前Python脚本工作的目录路径
```

```
os.listdir() #返回指定目录下的所有文件和目录名
```

```
os.remove() #.函数用来删除一个文件
```

```
os.removedirs（r“c：\python”） #删除多个目录
```

```
os.path.isfile() #检验给出的路径是否是一个文件
```

```
os.path.isdir() #检验给出的路径是否是一个目录
```

```
os.path.isabs() #判断是否是绝对路径
```

```
os.path.exists() #检验给出的路径是否真地存
```

```
os.path.split() #返回一个路径的目录名和文件名 返回队列， 0 为 目录名， 1 为文件名
```

```
os.path.splitext() #分离扩展名 返回队列， 0 为 basename， 1 扩展名
```

```
os.path.dirname() #仅获取路径名
```

```
os.path.basename() #获取文件名
```

```
os.system() #运行shell/bat命令
```

```
os.getenv() 与os.putenv() #读取和设置环境变量
```

```
os.rename(old,new) #重命名
```

```
os.makedirs（r“c：\python\test”） #创建多级目录
```

```
os.mkdir（“test”）#创建单个目录
```

```
os.stat（file）#获取文件属性
```

```
os.chmod（file）#修改文件权限与时间戳
```

```
os.exit（）#终止当前进程
```

```
os.path.getsize（filename）#获取文件大小
```

