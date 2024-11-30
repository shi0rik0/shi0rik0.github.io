---
title: "Windows环境下Python 3 UTF-8编码的一些坑"
date: 2024-11-30T22:02:01+08:00
draft: false
---

众所周知，Python 3 相比 Python 2 最大的变化之一就是字符串强制使用 UTF-8 编码，但这并不意味着使用 Python 3 就不会有字符编码的问题。在 Python 的世界里，确实一切皆 UTF-8，但是一旦和操作系统交互，就可能会出现问题。

## open()的默认编码

在 Unix 系统上，open()函数的默认编码是 UTF-8，但是在 Windows 系统上，open()函数的默认编码是 ANSI（也就是根据 Windows 的地区设置而定，在中文 Windows 上一般是 GBK 编码）。因此，如果在 Windows 系统上要让 open()函数使用 UTF-8 编码，就需要显式指定编码：

```python
open('file.txt', encoding='utf-8')
```

## Python 的 UTF-8 模式

在[PEP 540](https://peps.python.org/pep-0540/)中，Python 引入了一个新的“UTF-8 模式”。这个模式可以通过以下两种方式启动：

1. 在环境变量中设置`PYTHONUTF8=1`。
2. 在启动 Python 解释器时，使用`-X utf8`参数，例如：

```bash
python -X utf8 script.py
```

启动 UTF-8 模式后，哪怕是在 Windows 系统上，open()函数也会默认使用 UTF-8 编码。

## sys.stdout 的编码

本节将讨论 Windows 命令行输出的编码问题，下文中的命令行指的都是 PowerShell。

在 Windows 环境下，如果启动 Python 解释器的时候没有将 stdout 重定向，那么 sys.stdout 的编码是 UTF-8，否则就会是 ANSI。下面的例子就证明了这一点：

```python
# test.py
import sys
print(sys.stdout.encoding)
```

```powershell
PS> python test.py
utf-8
PS> python test.py > out.txt
PS> gc out.txt
gbk
```

要让 sys.stdout 的编码始终是 UTF-8，要么启用上文所提到的 UTF-8 模式，要么就要在代码中显式指定编码：

```python
import sys
sys.stdout.reconfigure(encoding='utf-8')
```

不过，真正麻烦的地方才刚刚开始。在重定向 stdout 之后，不仅要让 Python 输出 UTF-8，还要让命令行知道输出的是 UTF-8，否则依然会出现乱码。这可以通过指定`Out-File`命令的`-Encoding`参数来实现，但这里又有一个奇怪的地方，就是要指定`-Encoding`为`default`（也就是 ANSI）才不会乱码。具体的原因我也不太清楚。

```powershell
python test.py | Out-File -Encoding default out.txt
```

如果觉得每次都要这样写很麻烦，也可以通过指定`$PSDefaultParameterValues`来让`Out-File`的`-Encoding`参数默认为`default`：

```powershell
$PSDefaultParameterValues['Out-File:Encoding'] = 'default'
python test.py | Out-File out.txt
```

可以通过编辑`$PROFILE`文件来让这个设置永久生效：

```powershell
if (-not (Test-Path $PROFILE)) {
    New-Item $PROFILE -ItemType File -Force
}
Add-Content -Path $PROFILE -Value "`$PSDefaultParameterValues['Out-File:Encoding'] = 'default'"
```

这里还有一个坑，就是 PowerShell 7 开始，这个`default`参数被改名成了`ansi`，所以要注意版本。不过目前 Windows 11 自带的 PowerShell 还是 5.x 版本。

这里提供一个我写的 PowerShell 函数`My-Out-File`，可以自动根据版本来设置`-Encoding`：

```powershell
function My-Out-File {
    param (
        [string]$Path,

        [Parameter(ValueFromPipeline = $true)]
        [string]$InputString
    )

    $version = $PSVersionTable.PSVersion.Major

    if ($version -eq 5) {
        Write-Output $InputString | Out-File -FilePath $Path -Encoding default
    } elseif ($version -ge 7) {
        Write-Output $InputString | Out-File -FilePath $Path -Encoding ansi
    } else {
        Write-Host "Unsupported PowerShell version: $version"
    }
}
```
