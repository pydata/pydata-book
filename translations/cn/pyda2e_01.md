# 第一章 准备工作

## 1.1 本书的内容

本书讲的是利用 Python 进行数据控制、处理、整理、分析等方面的具体细节和基本要点。我的目标是介绍 Python 编程和用于数据处理的库和工具环境，掌握这些，可以让你成为一个数据分析专家。虽然本书的标题是“数据分析”，重点却是 Python 编程、库，以及用于数据分析的工具。这就是数据分析要用到的 Python 编程。

### 什么样的数据？

当书中出现“数据”时，究竟指的是什么呢？主要指的是结构化数据（structured data），这个故意含糊其辞的术语代指了所有通用格式的数据，例如：

* 表格型数据，其中各列可能是不同的类型（字符串、数值、日期等）。比如保存在关系型数据库中或以制表符/逗号为分隔符的文本文件中的那些数据。
* 多维数组（矩阵）。
* 通过关键列（对于 SQL 用户而言，就是主键和外键）相互联系的多个表。
* 间隔平均或不平均的时间序列。

这绝不是一个完整的列表。大部分数据集都能被转化为更加适合分析和建模的结构化形式，虽然有时这并不是很明显。如果不行的话，也可以将数据集的特征提取为某种结构化形式。例如，一组新闻文章可以被处理为一张词频表，而这张词频表就可以用于情感分析。

大部分电子表格软件（比如 Microsoft Excel，它可能是世界上使用最广泛的数据分析工具了）的用户不会对此类数据感到陌生。

## 1.2 为什么要使用 Python 进行数据分析

许许多多的人（包括我自己）都很容易爱上 Python 这门语言。自从 1991 年诞生以来，Python 现在已经成为最受欢迎的动态编程语言之一，其他还有 Perl、Ruby 等。由于拥有大量的 Web 框架（比如 Rails（Ruby）和 Django（Python）），自从 2005 年，使用 Python 和 Ruby 进行网站建设工作非常流行。这些语言常被称作脚本（scripting）语言，因为它们可以用于编写简短而粗糙的小程序（也就是脚本）。我个人并不喜欢“脚本语言”这个术语，因为它好像在说这些语言无法用于构建严谨的软件。在众多解释型语言中，由于各种历史和文化的原因，Python 发展出了一个巨大而活跃的科学计算（scientific computing）社区。在过去的 10 年，Python 从一个边缘或“自担风险”的科学计算语言，成为了数据科学、机器学习、学界和工业界软件开发最重要的语言之一。

在数据分析、交互式计算以及数据可视化方面，Python 将不可避免地与其他开源和商业的领域特定编程语言/工具进行对比，如 R、MATLAB、SAS、Stata 等。近年来，由于 Python 的库（例如 pandas 和 scikit-learn）不断改良，使其成为数据分析任务的一个优选方案。结合其在通用编程方面的强大实力，我们完全可以只使用 Python 这一种语言构建以数据为中心的应用。

### Python 作为胶水语言

Python 成为成功的科学计算工具的部分原因是，它能够轻松地集成 C、C++ 以及 Fortran 代码。大部分现代计算环境都利用了一些 Fortran 和 C 库来实现线性代数、优选、积分、快速傅里叶变换以及其他诸如此类的算法。许多企业和国家实验室也利用 Python 来“粘合”那些已经用了多年的遗留软件系统。

大多数软件都是由两部分代码组成的：少量需要占用大部分执行时间的代码，以及大量不经常执行的“胶水代码”。大部分情况下，胶水代码的执行时间是微不足道的。开发人员的精力几乎都是花在优化计算瓶颈上面，有时更是直接转用更低级的语言（比如 C）。

### 解决“两种语言”问题

很多组织通常都会用一种类似于领域特定的计算语言（如 SAS 和 R）对新想法做研究、原型构建和测试，然后再将这些想法移植到某个更大的生产系统中去（可能是用 Java、C\# 或 C++ 编写的）。人们逐渐意识到，Python 不仅适用于研究和原型构建，同时也适用于构建生产系统。为什么一种语言就够了，却要使用两个语言的开发环境呢？我相信越来越多的企业也会这样看，因为研究人员和工程技术人员使用同一种编程工具将会给企业带来非常显著的组织效益。

### 为什么不选 Python

虽然 Python 非常适合构建分析应用以及通用系统，但它对不少应用场景适用性较差。

由于 Python 是一种解释型编程语言，因此大部分 Python 代码都要比用编译型语言（比如 Java 和 C++）编写的代码运行慢得多。由于程序员的时间通常都比 CPU 时间值钱，因此许多人也愿意对此做一些取舍。但是，在那些延迟要求非常小或高资源利用率的应用中（例如高频交易系统），耗费时间使用诸如 C++ 这样更低级、更低生产率的语言进行编程也是值得的。

对于高并发、多线程的应用程序而言（尤其是拥有许多计算密集型线程的应用程序），Python 并不是一种理想的编程语言。这是因为 Python 有一个叫做全局解释器锁（Global Interpreter Lock，GIL）的组件，这是一种防止解释器同时执行多条 Python 字节码指令的机制。有关“为什么会存在 GIL”的技术性原因超出了本书的范围。虽然很多大数据处理应用程序为了能在较短的时间内完成数据集的处理工作都需要运行在计算机集群上，但是仍然有一些情况需要用单进程多线程系统来解决。

这并不是说 Python 不能执行真正的多线程并行代码。例如，Python 的 C 插件使用原生的 C 或 C++ 的多线程，可以并行运行而不被 GIL 影响，只要它们不频繁地与 Python 对象交互。

## 1.3 重要的 Python 库

考虑到那些还不太了解 Python 科学计算生态系统和库的读者，下面我先对各个库做一个简单的介绍。

NumPy NumPy（Numerical Python 的简称）是 Python 科学计算的基础包。本书大部分内容都基于 NumPy 以及构建于其上的库。它提供了以下功能（不限于此）：

* 快速高效的多维数组对象`ndarray`。
* 用于对数组执行元素级计算以及直接对数组执行数学运算的函数。
* 用于读写硬盘上基于数组的数据集的工具。
* 线性代数运算、傅里叶变换，以及随机数生成。 

成熟的 C API， 用于 Python 插件和原生 C、C++、Fortran 代码访问 NumPy 的数据结构和计算工具。

除了为 Python 提供快速的数组处理能力，NumPy 在数据分析方面还有另外一个主要作用，即作为在算法和库之间传递数据的容器。对于数值型数据，NumPy 数组在存储和处理数据时要比内置的 Python 数据结构高效得多。此外，由低级语言（比如 C 和 Fortran）编写的库可以直接操作 NumPy 数组中的数据，无需进行任何数据复制工作。因此，许多 Python 的数值计算工具要么使用 NumPy 数组作为主要的数据结构，要么可以与 NumPy 进行无缝交互操作。

### pandas

pandas 提供了快速便捷处理结构化数据的大量数据结构和函数。自从 2010 年出现以来，它助使 Python 成为强大而高效的数据分析环境。本书用得最多的 pandas 对象是`DataFrame`，它是一个面向列（column-oriented）的二维表结构，另一个是`Series`，一个一维的标签化数组对象。

pandas 兼具 NumPy 高性能的数组计算功能以及电子表格和关系型数据库（如 SQL）灵活的数据处理功能。它提供了复杂精细的索引功能，能更加便捷地完成重塑、切片和切块、聚合以及选取数据子集等操作。因为数据操作、准备、清洗是数据分析最重要的技能，pandas 是本书的重点。

作为背景，我是在 2008 年初开始开发 pandas 的，那时我任职于 AQR Capital Management，一家量化投资管理公司，我有许多工作需求都不能用任何单一的工具解决：

* 有标签轴的数据结构，支持自动或清晰的数据对齐。这可以防止由于数据不对齐，或处理来源不同的索引不同的数据，所造成的错误。
* 集成时间序列功能。
* 相同的数据结构用于处理时间序列数据和非时间序列数据。
* 保存元数据的算术运算和压缩。
* 灵活处理缺失数据。
* 合并和其它流行数据库（例如基于 SQL 的数据库）的关系操作。

我想只用一种工具就实现所有功能，并使用通用软件开发语言。Python 是一个不错的候选语言，但是此时没有集成的数据结构和工具来实现。我一开始就是想把 pandas 设计为一款适用于金融和商业分析的工具，pandas 专注于深度时间序列功能和工具，适用于时间索引化的数据。

对于使用 R 语言进行统计计算的用户，肯定不会对`DataFrame`这个名字感到陌生，因为它源自于 R 的`data.frame`对象。但与 Python 不同，`data.frame`是构建于 R 和它的标准库。因此，pandas 的许多功能不属于 R 或它的扩展包。

pandas 这个名字源于面板数据（panel data，这是多维结构化数据集在计量经济学中的术语）以及 Python 数据分析（Python data analysis）。

### matplotlib

matplotlib 是最流行的用于绘制图表和其它二维数据可视化的 Python 库。它最初由 John D.Hunter（JDH）创建，目前由一个庞大的开发团队维护。它非常适合创建出版物上用的图表。虽然还有其它的 Python 可视化库，matplotlib 却是使用最广泛的，并且它和其它生态工具配合也非常完美。我认为，可以使用它作为默认的可视化工具。

### IPython 和 Jupyter

IPython 项目起初是 Fernando Pérez 在 2001 年的一个用以加强和 Python 交互的子项目。在随后的 16 年中，它成为了 Python 数据栈最重要的工具之一。虽然 IPython 本身没有提供计算和数据分析的工具，它却可以大大提高交互式计算和软件开发的生产率。IPython 鼓励“执行-探索”的工作流，区别于其它编程软件的“编辑-编译-运行”的工作流。它还可以方便地访问系统的 shell 和文件系统。因为大部分的数据分析代码包括探索、试错和重复，IPython 可以使工作更快。

2014 年，Fernando 和 IPython 团队宣布了 Jupyter 项目，一个更宽泛的多语言交互计算工具的计划。IPython web 笔记本变成了 Jupyter 笔记本，现在支持 40 种编程语言。IPython 现在可以作为 Jupyter 使用 Python 的内核（一种编程语言模式）。

IPython 变成了 Jupyter 庞大开源项目（一个交互和探索式计算的高效环境）中的一个组件。它最老也是最简单的模式，现在是一个用于编写、测试、调试 Python 代码的强化 shell。你还可以使用通过 Jupyter 笔记本，一个支持多种语言的交互式网络代码“笔记本”，来使用 IPython。IPython shell 和 Jupyter 笔记本特别适合进行数据探索和可视化。

Jupyter 笔记本还可以编写 Markdown 和 HTML 内容，它提供了一种创建代码和文本的富文本方法。其它编程语言也在 Jupyter 中植入了内核，好让在 Jupyter 中可以使用 Python 以外的语言。

对我个人而言，我的大部分 Python 工作都要用到 IPython，包括运行、调试和测试代码。

在本书的 GitHub 页面，你可以找到包含各章节所有代码实例的 Jupyter 笔记本。

### SciPy

SciPy 是一组专门解决科学计算中各种标准问题域的包的集合，主要包括下面这些包：

* `scipy.integrate`：数值积分例程和微分方程求解器。
* `scipy.linalg`：扩展了由`numpy.linalg`提供的线性代数例程和矩阵分解功能。
* `scipy.optimize`：函数优化器（最小化器）以及根查找算法。
* `scipy.signal`：信号处理工具。
* `scipy.sparse`：稀疏矩阵和稀疏线性系统求解器。
* `scipy.special`：SPECFUN（这是一个实现了许多常用数学函数（如伽玛函数）的 Fortran 库）的包装器。
* `scipy.stats`：标准连续和离散概率分布（如密度函数、采样器、连续分布函数等）、各种统计检验方法，以及更好的描述统计法。

NumPy 和 SciPy 结合使用，便形成了一个相当完备和成熟的计算平台，可以处理多种传统的科学计算问题。

### scikit-learn

2010 年诞生以来，scikit-learn 成为了 Python 的通用机器学习工具包。仅仅七年，就汇聚了全世界超过 1500 名贡献者。它的子模块包括：

* 分类：SVM、近邻、随机森林、逻辑回归等等。
* 回归：Lasso、岭回归等等。
* 聚类：k-均值、谱聚类等等。
* 降维：PCA、特征选择、矩阵分解等等。
* 选型：网格搜索、交叉验证、度量。
* 预处理：特征提取、标准化。

与 pandas、statsmodels 和 IPython 一起，scikit-learn 对于 Python 成为高效数据科学编程语言起到了关键作用。虽然本书不会详细讲解 scikit-learn，我会简要介绍它的一些模型，以及用其它工具如何使用这些模型。

### statsmodels

statsmodels 是一个统计分析包，起源于斯坦福大学统计学教授 Jonathan Taylor，他设计了多种流行于 R 语言的回归分析模型。Skipper Seabold 和 Josef Perktold 在 2010 年正式创建了 statsmodels 项目，随后汇聚了大量的使用者和贡献者。受到 R 的公式系统的启发，Nathaniel Smith 发展出了 Patsy 项目，它提供了 statsmodels 的公式或模型的规范框架。

与 scikit-learn 比较，statsmodels 包含经典统计学和经济计量学的算法。包括如下子模块：

* 回归模型：线性回归，广义线性模型，健壮线性模型，线性混合效应模型等等。
* 方差分析（ANOVA）。
* 时间序列分析：AR，ARMA，ARIMA，VAR 和其它模型。
* 非参数方法： 核密度估计，核回归。
* 统计模型结果可视化。

statsmodels 更关注与统计推断，提供不确定估计和参数 p-值。相反的，scikit-learn 注重预测。

同 scikit-learn 一样，我也只是简要介绍 statsmodels，以及如何用 NumPy 和 pandas 使用它。

## 1.4 安装和设置

由于人们用 Python 所做的事情不同，所以没有一个普适的 Python 及其插件包的安装方案。由于许多读者的 Python 科学计算环境都不能完全满足本书的需要，所以接下来我将详细介绍各个操作系统上的安装方法。我推荐免费的 Anaconda 安装包。写作本书时，Anaconda 提供 Python 2.7 和 3.6 两个版本，以后可能发生变化。本书使用的是 Python 3.6，因此推荐选择 Python 3.6 或更高版本。

### Windows

要在 Windows 上运行，先下载 [Anaconda 安装包](https://www.anaconda.com/download/)。推荐跟随 Anaconda 下载页面的 Windows 安装指导，安装指导在写作本书和读者看到此文的的这段时间内可能发生变化。

现在，来确认设置是否正确。打开命令行窗口（`cmd.exe`），输入`python`以打开 Python 解释器。可以看到类似下面的 Anaconda 版本的输出：

```text
C:\Users\wesm>python
Python 3.5.2 |Anaconda 4.1.1 (64-bit)| (default, Jul  5 2016, 11:41:13)
[MSC v.1900 64 bit (AMD64)] on win32
>>>
```

要退出 shell，按`Ctrl-D`（Linux 或 macOS 上），`Ctrl-Z`（Windows 上），或输入命令`exit()`，再按`Enter`。

### Apple（OS X, macOS）

下载 OS X Anaconda 安装包，它的名字类似`Anaconda3-4.1.0-MacOSX-x86_64.pkg`。双击`.pkg`文件，运行安装包。安装包运行时，会自动将 Anaconda 执行路径添加到`.bash_profile`文件，它位于`/Users/$USER/.bash_profile`。

为了确认成功，在系统 shell 打开 IPython：

```text
$ ipython
```

要退出 shell，按`Ctrl-D`，或输入命令`exit()`，再按`Enter`。

### GNU/Linux

Linux 版本很多，这里给出 Debian、Ubantu、CentOS 和 Fedora 的安装方法。安装包是一个脚本文件，必须在 shell 中运行。取决于系统是 32 位还是 64 位，要么选择 x86（32 位）或 x86_64（64 位）安装包。随后你会得到一个文件，名字类似于`Anaconda3-4.1.0-Linux-x86_64.sh`。用 bash 进行安装：

```text
$ bash Anaconda3-4.1.0-Linux-x86_64.sh
```

> 笔记：某些 Linux 版本在包管理器中有满足需求的 Python 包，只需用类似`apt`的工具安装就行。这里讲的用 Anaconda 安装，适用于不同的 Linux 安装包，也很容易将包升级到最新版本。

接受许可之后，会向你询问在哪里放置 Anaconda 的文件。我推荐将文件安装到默认的`home`目录，例如`/home/$USER/anaconda`。

Anaconda 安装包可能会询问你是否将`bin/`目录添加到`$PATH`变量。如果在安装之后有任何问题，你可以修改文件`.bashrc`（或`.zshrc`，如果使用的是 zsh shell）为类似以下的内容：

```text
export PATH=/home/$USER/anaconda/bin:$PATH
```

做完之后，你可以开启一个新窗口，或再次用`~/.bashrc`执行`.bashrc`。

### 安装或升级 Python 包

在你阅读本书的时候，你可能想安装另外的不在 Anaconda 中的 Python 包。通常，可以用以下命令安装：

```text
conda install package_name
```

如果这个命令不行，也可以用`pip`包管理工具：

```text
pip install package_name
```

你可以用`conda update`命令升级包：

```text
conda update package_name
```

`pip`可以用`--upgrade`升级：

```text
pip install --upgrade package_name
```

本书中，你有许多机会尝试这些命令。

> 注意：当你使用`conda`和`pip`二者安装包时，千万不要用`pip`升级`conda`的包，这样会导致环境发生问题。当使用 Anaconda 或 Miniconda 时，最好首先使用`conda`进行升级。

Python 2 和 Python 3

第一版的 Python 3.x 出现于 2008 年。它有一系列的变化，与之前的 Python 2.x 代码有不兼容的地方。因为从 1991 年 Python 出现算起，已经过了 17 年，Python 3 的出现被视为吸取一些列教训的更优结果。

2012 年，因为许多包还没有完全支持 Python 3，许多科学和数据分析社区还是在使用 Python 2.x。因此，本书第一版使用的是 Python 2.7。现在，用户可以在 Python 2.x 和 Python 3.x 间自由选择，二者都有良好的支持。

但是，Python 2.x 在 2020 年就会到期（包括重要的安全补丁），因此再用 Python 2.7 就不是好的选择了。因此，本书使用了 Python 3.6，这一广泛使用、支持良好的稳定版本。我们已经称 Python 2.x 为“遗留版本”，简称 Python 3.x 为“Python”。我建议你也是如此。

本书基于 Python 3.6。你的 Python 版本也许高于 3.6，但是示例代码应该是向前兼容的。一些示例代码可能在 Python 2.7 上有所不同，或完全不兼容。

### 集成开发环境（IDEs）和文本编辑器

当被问到我的标准开发环境，我几乎总是回答“IPython 加文本编辑器”。我通常在编程时，反复在 IPython 或 Jupyter 笔记本中测试和调试每条代码。也可以交互式操作数据，和可视化验证数据操作中某一特殊集合。在 shell 中使用 pandas 和 NumPy 也很容易。

但是，当创建软件时，一些用户可能更想使用特点更为丰富的 IDE，而不仅仅是原始的 Emacs 或 Vim 的文本编辑器。以下是一些 IDE：

* PyDev（免费），基于 Eclipse 平台的 IDE；
* JetBrains 的 PyCharm（商业用户需要订阅，开源开发者免费）；
* Visual Studio（Windows 用户）的 Python Tools；
* Spyder（免费），Anaconda 附带的 IDE；
* Komodo IDE（商业）。

因为 Python 的流行，大多数文本编辑器，比如 Atom 和 Sublime Text 3，对 Python 的支持也非常好。

## 1.5 社区和会议

除了在网上搜索，各式各样的科学和数据相关的 Python 邮件列表是非常有帮助的，很容易获得回答。包括：

* pydata：一个 Google 群组列表，用以回答 Python 数据分析和 pandas 的问题；
* pystatsmodels： statsmodels 或 pandas 相关的问题；
* scikit-learn 和 Python 机器学习邮件列表，`scikit-learn@python.org`；
* numpy-discussion：和 NumPy 相关的问题；
* scipy-user：SciPy 和科学计算的问题；

因为这些邮件列表的 URLs 可以很容易搜索到，但因为可能发生变化，所以没有给出。

每年，世界各地会举办许多 Python 开发者大会。如果你想结识其他有相同兴趣的人，如果可能的话，我建议你去参加一个。许多会议会对无力支付入场费和差旅费的人提供财力帮助。下面是一些会议：

* PyCon 和 EuroPython：北美和欧洲的两大 Python 会议；
* SciPy 和 EuroSciPy：北美和欧洲两大面向科学计算的会议；
* PyData：世界范围内，一些列的地区性会议，专注数据科学和数据分析；
* 国际和地区的 PyCon 会议（[完整列表](http://pycon.org)）。

## 1.6 本书导航

如果之前从未使用过 Python，那你可能需要先看看本书的第二章和第三章，我简要介绍了 Python 的特点，IPython 和 Jupyter 笔记本。这些知识是为本书后面的内容做铺垫。如果你已经掌握 Python，可以选择跳过。

接下来，简单地介绍了 NumPy 的关键特性，附录 A 中是更高级的 NumPy 功能。然后，我介绍了 pandas，本书剩余的内容全部是使用 pandas、NumPy 和 matplotlib 处理数据分析的问题。我已经尽量让全书的结构循序渐进，但偶尔会有章节之间的交叉，有时用到的概念还没有介绍过。

尽管读者各自的工作任务不同，大体可以分为几类：

* 与外部世界交互

  阅读编写多种文件格式和数据存储；

* 数据准备

  清洗、修改、结合、标准化、重塑、切片、切割、转换数据，以进行分析；

* 转换数据

  对旧的数据集进行数学和统计操作，生成新的数据集（例如，通过各组变量聚类成大的表）；

* 建模和计算

  将数据绑定统计模型、机器学习算法、或其他计算工具；

* 展示

  创建交互式和静态的图表可视化和文本总结。

### 代码示例

本书大部分代码示例的输入形式和输出结果都会按照其在 IPython shell 或 Jupyter 笔记本中执行时的样子进行排版：

```text
In [5]: CODE EXAMPLE
Out[5]: OUTPUT
```

但你看到类似的示例代码，就是让你在`in`的部分输入代码，按`Enter`键执行（Jupyter 中是按`Shift-Enter`）。然后就可以在`out`看到输出。

### 示例数据

[各章的示例数据都存放在 GitHub 上](http://github.com/pydata/pydata-book)。下载这些数据的方法有二：使用 Git 版本控制命令行程序；直接从网站上下载该 GitHub 库的 zip 文件。如果遇到了问题，可以到[我的个人主页](http://wesmckinney.com/)，获取最新的指导。

为了让所有示例都能重现，我已经尽我所能使其包含所有必需的东西，但仍然可能会有一些错误或遗漏。如果出现这种情况的话，请给我发邮件：`wesmckinn@gmail.com`。[报告本书错误的最好方法是 O’Reilly 的 errata 页面](http://www.bit.ly/pyDataAnalysis_errata)。

### 引入惯例

Python 社区已经广泛采取了一些常用模块的命名惯例：

```python
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns
import statsmodels as sm
```

也就是说，当你看到`np.arange`时，就应该想到它引用的是 NumPy 中的`arange`函数。这样做的原因是：在 Python 软件开发过程中，不建议直接引入类似 NumPy 这种大型库的全部内容（`from numpy import *`）。

### 行话

由于你可能不太熟悉书中使用的一些有关编程和数据科学方面的常用术语，所以我在这里先给出其简单定义：

数据规整（Munge/Munging/Wrangling） 指的是将非结构化和（或）散乱数据处理为结构化或整洁形式的整个过程。这几个词已经悄悄成为当今数据黑客们的行话了。Munge 这个词跟 Lunge 押韵。

伪码（Pseudocode） 算法或过程的“代码式”描述，而这些代码本身并不是实际有效的源代码。

语法糖（Syntactic sugar） 这是一种编程语法，它并不会带来新的特性，但却能使代码更易读、更易写。

