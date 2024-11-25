---
title: "使用Docker搭建PySpark学习环境"
date: 2024-06-20T21:27:25+08:00
draft: false
---

首先安装 Docker，然后拉取 Spark 的 Docker 镜像：

```
docker pull apache/spark
```

然后创建一个 Docker 容器：

```
docker run -d -u root apache/spark tail -f /dev/null
```

（注：这里的-u root 的作用是把容器的默认用户设置成 root，如果不这么做，VSCode 就无法附加到容器上。）

接着就可以使用 VSCode 进入容器。进入后在终端里打开 spark-shell：

```
/opt/spark/bin/spark-shell
```

打开之后会看到类似下面的输出，里面包含了 Spark 的版本。这里我的版本是 3.5.1。

```
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 3.5.1
      /_/
```

然后安装对应版本的 PySpark：

```
pip install pyspark==3.5.1
```

**注意：这里的 PySpark 版本一定要和 Spark 版本一致。如果不一致，并不会立刻报错，而是会出现有些代码可以运行，有些代码不能运行的情况！**

安装完 PySpark 之后，就可以用 Python 编写 Spark 程序了。下面是一个简单的例子：

```python
from pyspark import SparkContext

sc = SparkContext.getOrCreate()

rdd = sc.parallelize(['apple', 'apple', 'banana', 'banana', 'banana']).map(lambda x: (x, 1)).reduceByKey(lambda x, y: x + y)
print(rdd.collect())
```

你应该会看到类似下面的输出：

```
[('banana', 3), ('apple', 2)]
```
