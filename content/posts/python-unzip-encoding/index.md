---
title: "用Python的zipfile标准库解压非标准编码的zip压缩包"
date: 2024-07-19T11:33:34+08:00
draft: false
---

时至今日（2024年），依然有很多日本的zip压缩包采用Shift JIS编码。使用7-Zip等软件解压这些压缩包的时候，会发现文件名都是乱码。WinRAR支持指定压缩包的文本编码，但是我个人不怎么喜欢WinRAR。因此，我决定使用Python的标准库来解压非标准编码的zip压缩包。

对于3.11及3.11之后的Python版本，`ZipFile`提供了一个`metadata_encoding`的参数，通过指定这个参数就可以设置编码：
```python
import zipfile

with zipfile.ZipFile('foo.zip', metadata_encoding='cp932') as f:
    f.extractall()
```

如果是3.11之前的Python版本，对于非UTF-8编码的压缩包，`ZipFile`默认使用的是cp437编码，因此我们需要手动修改字符串编码，然后再解压：
```python
import zipfile
import os
import shutil

with zipfile.ZipFile('foo.zip') as f:
    for name in f.namelist():
        path = name.encode('cp437').decode('cp932')
        if path[-1] == '/':
            os.makedirs(path, exist_ok=True)
        else:
            src = f.open(name)
            os.makedirs(os.path.dirname(path), exist_ok=True)
            with open(path, 'wb') as dst:
                shutil.copyfileobj(src, dst)
```

顺带一提，根据[这个StackOverflow问题](https://stackoverflow.com/questions/13261347/correctly-decoding-zip-entry-file-names-cp437-utf-8-or)，zip标准规定：若UTF-8的flag没有被设置，那么文件名的编码就应该是cp437，但是实际实现很多都没有遵从标准。
