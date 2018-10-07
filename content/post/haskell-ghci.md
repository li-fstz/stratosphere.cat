---
title: "Haskell GHCi 的使用"
date: 2018-10-05T00:06:33+08:00
---

&emsp;&emsp;为了配合教材，我选用了 [7.8.4 的 GHCi](https://www.haskell.org/ghc/download_ghc_7_8_4.html#windows64) ，下载直接添加 `ghc-7.8.4\bin` 到系统的 path 环境变量就可以使用了。
<!--more-->

### 命令
#### `:load`
&emsp;&emsp;简写为 `:l` ，载入已经编写好的 .hs 文件。
#### `:reload` 
&emsp;&emsp;简写为 `:r` ，重新载入之间载入的文件。
#### `:cd` 
&emsp;&emsp;改变 GHCi 当前的目录，使得载入文件的时候可以使用较短的相对路径。
#### `:edit` 
&emsp;&emsp;使用默认的文本编辑器编辑已经载入的文件。
#### `:!`
&emsp;&emsp;执行系统命令。
#### `:quit`
&emsp;&emsp;退出 GHCi。
#### `:?`
&emsp;&emsp;输出帮助信息。
#### `:type`
&emsp;&emsp;简写为 `:t` ，输出表达式的类型。
#### `:module`
&emsp;&emsp;简写为 `:m` ，导入一个库。