author: StudyingFather, ayalhw

虽然图形界面能做的事情越来越多，但有很多高阶操作仍然需要使用命令行来解决。

本页面将简要介绍命令行的一些使用方法。

## 一些常用命令[^1]

### 文件系统相关

先介绍文件系统里描述位置的两种方式，相对路径和绝对路径。

- 相对路径：用相对当前路径的位置关系来描述位置。例如当前路径为 `~/folder` ，则 `./a.cpp` 实际上指的就是 `~/folder/a.cpp` 这个文件。 **随着当前路径的变化，相对路径描述的位置也可能发生改变** 。
-   绝对路径：用完整的路径来描述位置。例如 `~/folder/a.cpp` 就是一个绝对路径的例子。 **绝对路径描述的位置不随当前路径的变化而改变** 。

    Windows/Linux 用 `.` 代表当前目录， `..` 代表当前目录的父目录。特别地，在 Linux 下，用 `~` 表示用户主目录（注意 `~` 由 shell 展开，因此在其他地方可能不可用）。

在 Windows/Linux 下，使用 `cd <目录>` 命令可以切换当前的目录。例如， `cd folder` 会切换到当前目录的 `folder` 子目录； `cd ..` 会切换到当前目录的父目录。

在 Windows 下，使用 `dir` 命令可以列出当前目录的文件列表。在 Linux 下，列出文件列表的命令是 `ls` 。

在 Windows 下，使用 `md <目录>` 命令创建一个新目录，使用 `rd <目录>` 命令删除一个目录。在 Linux 下，这两个命令分别是 `mkdir` 和 `rmdir` 。需要注意的是， **使用 `rd` 或是 `rmdir` 删除一个目录前，这个目录必须是空的** 。如果想要删除非空目录（和该目录下的所有文件）的话，Linux 下可以执行 `rm -r <目录>` 命令，Windows 下可以执行 `rd /s <目录>` 命令。

### 重定向机制

> 我编译了一个程序，它从标准输入读入，并输出到标准输出。然而输入文件和输出文件都很大，这时候能不能想办法把输入重定向到指定的输入文件，输出重定向到指定的输出文件呢？

使用如下命令即可做到。

```bash
command < input > output
```

例如， `./prog < 1.in > 1.out` 这个命令就将让 `prog` 这个程序从当前目录下的 `1.in` 中读入数据，并将程序输出覆盖写入到 `1.out` 。

???+ warning
     `1.out` 原本的内容会被覆盖，如果想要在原输出文件末尾追加写入，请使用 `>>` ，即 `./prog >> 1.out` 的方式做输出重定向

事实上，大多数 OJ 都采用了这样的重定向机制。选手提交的程序采用标准输入输出，通过重定向机制，就可以让选手的程序从给定的输入文件读入数据，输出到指定的输出文件，再进行文件比较就可以评测了。

### 执行程序

对于一个可执行程序或是批处理脚本，只需在命令行里直接输入它的文件名即可执行它。

当然，执行一个文件时，命令行并不会把所有目录下的文件都找一遍。环境变量 `PATH` 描述了命令行搜索路径的范围，命令行会在 `PATH` 中的路径寻找目标文件。

对于 Windows 系统， **当前目录也在命令行的默认搜索范围内** 。例如 Windows 系统中，输入 `hello` 命令就可以执行当前目录下的 `hello.exe` 。

在 Linux 系统中， **当前目录并不在命令行的默认搜索范围内** ，所以执行当前目录下的 `hello` 程序的命令就变成了 `./hello` 。

### 总结

上面介绍的用法只是命令行命令的一小部分，还有很多命令没有涉及到。在命令行里输入帮助命令 `help` ，可以查询所有命令以及它们的用途。

下面给出 Windows 系统和 Linux 系统的命令对照表，以供参考。

| 分类   | Windows 系统 | Linux 系统  |
| ---- | ---------- | --------- |
| 文件列表 |  `dir`     |  `ls`     |
| 切换目录 |  `cd`      |  `cd`     |
| 建立目录 |  `md`      |  `mkdir`  |
| 删除目录 |  `rd`      |  `rmdir`  |
| 比较文件 |  `fc`      |  `diff`   |
| 复制文件 |  `copy`    |  `cp`     |
| 移动文件 |  `move`    |  `mv`     |
| 文件改名 |  `ren`     |  `mv`     |
| 删除文件 |  `del`     |  `rm`     |

## 使用命令行编译/调试

### 命令行编译

在命令行下输入 `g++ a.cpp` 就可以编译 `a.cpp` 这个文件了（Windows 系统需提前把编译器所在目录加入到 `PATH` 中）。

编译过程中可以加入一些编译选项：

-  `-o <文件名>` ：指定编译器输出可执行文件的文件名。
-  `-g` ：在编译时添加调试信息（使用 gdb 调试时需要）。
-  `-Wall` ：显示所有编译警告信息。
-  `-O1` ， `-O2` ， `-O3` ：对编译的程序进行优化，数字越大表示采用的优化手段越多（开启优化会影响使用 gdb 调试）。
-  `-DDEBUG` ：在编译时定义 `DEBUG` 符号（符号可以随意更换，例如 `-DONLINE_JUDGE` 定义了 `ONLINE_JUDGE` 符号）。
-  `-UDEBUG` ：在编译时取消定义 `DEBUG` 符号。
-  `-lm` ， `-lgmp` : 链接某个库（此处是 math 和 gmp，具体使用的名字需查阅库文档，但一般与库名相同）。

???+ note
    在 Linux 下，如使用了标准 C 库里的 math 库（ `math.h` ），则需在编译时添加 `-lm` 参数。[^have-to-link-libm-in-gcc]

### 命令行调试

在命令行下，最常用的调试工具是 gdb。

执行 `gdb a` 就可以调试 `a` 程序。

以下是几个 gdb 调试的常用命令（大多数命令可以缩写，用命令开头的若干个字母就可以代表该命令）：

-  `list` （ `l` ）：列出程序源代码，如 `l main` 指定列出 `main` 函数附近的若干行代码。
-  `break` （ `b` ）：设置断点，如 `b main` 表示在 `main` 函数处设置断点。
-  `run` （ `r` ）：运行程序直到程序结束运行或遇到断点。
-  `continue` （ `c` ）：在程序遇到断点后继续执行，直到程序结束运行或到达下一个断点。
-  `next` （ `n` ）：执行当前行语句，如果当前行有函数调用，则将其视为一个整体执行。
-  `step` （ `s` ）：执行当前行语句，如果当前行有函数调用，则进入该函数内部。
-  `finish` （ `fin` ）：继续执行至当前函数返回。
-  `call` ：调用某个函数，例如： `call f(2)` （以参数 2 调用函数 f）。
-  `quit` （ `q` ）：退出 gdb。
-  `display` （ `disp` ）：指定程序暂停时显示的表达式。
-    `print` （ `p` ）：打印表达式的值。

     `display` 和 `print` 指令都支持控制输出格式，其方法是在命令后紧跟 `/` 与格式字符，例如 `p/d test` （按照十进制打印变量 test 的值），
    支持的格式字符有：

| 格式字符 | 对应格式          |
| ---- | ------------- |
| d    | 按十进制格式显示变量    |
| x    | 按十六进制格式显示变量   |
| a    | 按十六进制格式显示变量   |
| t    | 按二进制格式显示变量    |
| c    | 按字符格式显示变量     |
| f    | 按浮点数格式显示变量    |
| u    | 按十进制格式显示无符号整型 |
| o    | 按八进制格式显示变量    |

## 参考资料与注释

[^1]: 刘汝佳《算法竞赛入门经典（第 2 版）》附录 A 开发环境与方法

[^have-to-link-libm-in-gcc]:  [Why do you have to link the math library in C?](https://stackoverflow.com/questions/1033898/why-do-you-have-to-link-the-math-library-in-c) 
