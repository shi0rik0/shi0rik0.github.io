---
title: "关于LaTeX的各种算法伪代码宏包"
date: 2024-05-19T15:07:12+08:00
draft: false
---

在用 LaTeX 描述算法的时候，经常会用到 algorithm、algorithmic、algorithmicx、algpseudocode、algcompatible、algorithm2e 等各种宏包。这些宏包关系比较复杂，并且用法也存在差异，经常会引发混淆。本文就对这些宏包的作用进行简要的介绍。

首先，这些宏包可以分为两大类，algorithm2e 单独算作一类，而其他的是另一类。任何算法都可以分为算法标题和算法主体两个部分。对于 algorithm2e 宏包而言，算法标题和算法主体都通过 algorithm 环境来实现。而对于其他的宏包来说，算法标题通过 algorithm 环境实现，算法主体通过 algorithmic 环境实现。也就是说，如果你用的是 algorithm2e 宏包，那么你的算法代码大概就是下面这样：

```latex
\begin{algorithm}
\caption{An algorithm with caption}\label{alg:cap}
% 算法主体
\end{algorithm}
```

而如果用其他的宏包，那么就会变成下面这样：

```latex
\begin{algorithm}
\caption{An algorithm with caption}\label{alg:cap}
\begin{algorithmic}
% 算法主体
\end{algorithmic}
\end{algorithm}
```

所以说，如果你用的是 algorithm2e 宏包，那么只需要导入这一个宏包就行。algorithm2e 宏包里面包含了一个一步到位的超强 algorithm 环境。而如果你用的是其他宏包，就必须组合使用两个宏包，一个是 algorithm 宏包，它提供了一个专门用于显示算法标题的 algorithm 环境，另外一个则是 algorithmic、algorithmicx、algpseudocode 和 algcompatible 中的其中一个，它们都提供了用于显示算法主体的 algorithmic 环境。

接下来介绍一下 algorithmic、algorithmicx、algpseudocode 和 algcompatible 的区别。algorithmic 宏包是最早的，现在已经不推荐使用。algorithmicx 是 algorithmic 的改进版，和 algorithmic 不同，algorithmicx 没有提供预定义的语句，只提供了定义语句的命令，因此单纯用 algorithmicx 写伪代码会很麻烦。algpseudocode 和 algcompatible 则是在 algorithmicx 的基础上提供了一组定义好的语句。algpseudocode 和 algcompatible 的主要区别是 algcompatible 额外提供了对于 algorithmic 的兼容性，如果不需要兼容的话，还是更推荐使用 algpseudocode。导入 algpseudocode 或者 algcompatible 都会顺便把 algorithmicx 也导入进来，所以导入了 algpseudocode 或者 algcompatible 就不用再导入 algorithmicx 了。

最后是省流版：目前 LaTeX 的算法伪代码宏包主要就两种选择。第一种是`\usepackage{algorithm, algpseudocode}`，第二种是`\usepackage{algorithm2e}`。它们的语法是不一样的，选择一个自己喜欢的就行。
