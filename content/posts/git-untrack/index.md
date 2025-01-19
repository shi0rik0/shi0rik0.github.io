---
title: "Git取消追踪文件"
date: 2025-01-19T11:31:00+08:00
draft: false
---

Git 可以用`.gitignore`来忽略一些文件，但是如果是已经被追踪（track）的文件，将其添加到`.gitignore`并不会取消追踪。这时候就需要用到`git rm --cached`命令来手动取消追踪。但是有时候，误追踪的文件自己都不知道，有没有办法可以自动找出所有被追踪，但是原本应该被忽略的文件呢？

查阅 Git 文档之后，发现有两个命令可以实现这个功能：

1. `git ls-files`，这个命令可以列出所有被追踪的文件。
2. `git check-ignore --no-index`，这个命令可以用来判断某个文件是否被`.gitignore`忽略。这里必须加上`--no-index`参数，否则将不会输出已经被追踪的文件。

结合这两个命令，我们可以写一个脚本来找出所有被追踪，但是原本应该被忽略的文件：

```python
import subprocess


def is_path_ignored(path: str) -> bool:
    return subprocess.run(['git', 'check-ignore', '--no-index', path],
                          check=True,
                          capture_output=True).returncode == 0


tracked_files = subprocess.run(['git', 'ls-files'],
                               check=True,
                               capture_output=True,
                               text=True).stdout.splitlines()

for file in tracked_files:
    if is_path_ignored(file):
        print(file)
```
