# 十二、Python 建模库介绍

> 原文：[`wesmckinney.com/book/modeling`](https://wesmckinney.com/book/modeling)
>
> 译者：[飞龙](https://github.com/wizardforcel)
>
> 协议：[CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)


> 此开放访问网络版本的《Python 数据分析第三版》现已作为[印刷版和数字版](https://amzn.to/3DyLaJc)的伴侣提供。如果您发现任何勘误，请[在此处报告](https://oreilly.com/catalog/0636920519829/errata)。请注意，由 Quarto 生成的本站点的某些方面与 O'Reilly 的印刷版和电子书版本的格式不同。
> 
> 如果您发现本书的在线版本有用，请考虑[订购纸质版](https://amzn.to/3DyLaJc)或[无 DRM 的电子书](https://www.ebooks.com/en-us/book/210644288/python-for-data-analysis/wes-mckinney/?affId=WES398681F)以支持作者。本网站的内容不得复制或再生产。代码示例采用 MIT 许可，可在 GitHub 或 Gitee 上找到。

在本书中，我专注于为在 Python 中进行数据分析提供编程基础。由于数据分析师和科学家经常报告花费大量时间进行数据整理和准备，因此本书的结构反映了掌握这些技术的重要性。

您用于开发模型的库将取决于应用程序。许多统计问题可以通过简单的技术解决，如普通最小二乘回归，而其他问题可能需要更高级的机器学习方法。幸运的是，Python 已经成为实现分析方法的首选语言之一，因此在完成本书后，您可以探索许多工具。

在本章中，我将回顾一些 pandas 的特性，这些特性在您在 pandas 中进行数据整理和模型拟合和评分之间来回切换时可能会有所帮助。然后，我将简要介绍两个流行的建模工具包，[statsmodels](http://statsmodels.org)和[scikit-learn](http://scikit-learn.org)。由于这两个项目都足够庞大，值得有自己的专门书籍，因此我没有尝试全面介绍，而是建议您查阅这两个项目的在线文档，以及一些其他基于 Python 的数据科学、统计学和机器学习书籍。

## 12.1 pandas 与模型代码之间的接口

模型开发的常见工作流程是使用 pandas 进行数据加载和清理，然后切换到建模库来构建模型本身。模型开发过程中的一个重要部分被称为*特征工程*，在机器学习中。这可以描述从原始数据集中提取信息的任何数据转换或分析，这些信息在建模环境中可能有用。我们在本书中探讨的数据聚合和 GroupBy 工具经常在特征工程环境中使用。

虽然“好”的特征工程的细节超出了本书的范围，但我将展示一些方法，使在 pandas 中进行数据操作和建模之间的切换尽可能轻松。

pandas 与其他分析库之间的接触点通常是 NumPy 数组。要将 DataFrame 转换为 NumPy 数组，请使用`to_numpy`方法：

```py
In [12]: data = pd.DataFrame({
 ....:     'x0': [1, 2, 3, 4, 5],
 ....:     'x1': [0.01, -0.01, 0.25, -4.1, 0.],
 ....:     'y': [-1.5, 0., 3.6, 1.3, -2.]})

In [13]: data
Out[13]: 
 x0    x1    y
0   1  0.01 -1.5
1   2 -0.01  0.0
2   3  0.25  3.6
3   4 -4.10  1.3
4   5  0.00 -2.0

In [14]: data.columns
Out[14]: Index(['x0', 'x1', 'y'], dtype='object')

In [15]: data.to_numpy()
Out[15]: 
array([[ 1.  ,  0.01, -1.5 ],
 [ 2.  , -0.01,  0.  ],
 [ 3.  ,  0.25,  3.6 ],
 [ 4.  , -4.1 ,  1.3 ],
 [ 5.  ,  0.  , -2.  ]])
```

回到 DataFrame，正如您可能从前几章中记得的那样，您可以传递一个二维的 ndarray，其中包含可选的列名：

```py
In [16]: df2 = pd.DataFrame(data.to_numpy(), columns=['one', 'two', 'three'])

In [17]: df2
Out[17]: 
 one   two  three
0  1.0  0.01   -1.5
1  2.0 -0.01    0.0
2  3.0  0.25    3.6
3  4.0 -4.10    1.3
4  5.0  0.00   -2.0
```

`to_numpy`方法旨在在数据是同质的情况下使用，例如所有的数值类型。如果您有异构数据，结果将是一个 Python 对象的 ndarray：

```py
In [18]: df3 = data.copy()

In [19]: df3['strings'] = ['a', 'b', 'c', 'd', 'e']

In [20]: df3
Out[20]: 
 x0    x1    y strings
0   1  0.01 -1.5       a
1   2 -0.01  0.0       b
2   3  0.25  3.6       c
3   4 -4.10  1.3       d
4   5  0.00 -2.0       e

In [21]: df3.to_numpy()
Out[21]: 
array([[1, 0.01, -1.5, 'a'],
 [2, -0.01, 0.0, 'b'],
 [3, 0.25, 3.6, 'c'],
 [4, -4.1, 1.3, 'd'],
 [5, 0.0, -2.0, 'e']], dtype=object)
```

对于某些模型，您可能希望仅使用部分列。我建议使用`loc`索引和`to_numpy`：

```py
In [22]: model_cols = ['x0', 'x1']

In [23]: data.loc[:, model_cols].to_numpy()
Out[23]: 
array([[ 1.  ,  0.01],
 [ 2.  , -0.01],
 [ 3.  ,  0.25],
 [ 4.  , -4.1 ],
 [ 5.  ,  0.  ]])
```

一些库原生支持 pandas，并自动完成一些工作：从 DataFrame 转换为 NumPy，并将模型参数名称附加到输出表或 Series 的列上。在其他情况下，您将不得不手动执行这种“元数据管理”。

在 Ch 7.5：分类数据中，我们看过 pandas 的`Categorical`类型和`pandas.get_dummies`函数。假设我们的示例数据集中有一个非数字列：

```py
In [24]: data['category'] = pd.Categorical(['a', 'b', 'a', 'a', 'b'],
 ....:                                   categories=['a', 'b'])

In [25]: data
Out[25]: 
 x0    x1    y category
0   1  0.01 -1.5        a
1   2 -0.01  0.0        b
2   3  0.25  3.6        a
3   4 -4.10  1.3        a
4   5  0.00 -2.0        b
```

如果我们想用虚拟变量替换`'category'`列，我们创建虚拟变量，删除`'category'`列，然后将结果连接：

```py
In [26]: dummies = pd.get_dummies(data.category, prefix='category',
 ....:                          dtype=float)

In [27]: data_with_dummies = data.drop('category', axis=1).join(dummies)

In [28]: data_with_dummies
Out[28]: 
 x0    x1    y  category_a  category_b
0   1  0.01 -1.5         1.0         0.0
1   2 -0.01  0.0         0.0         1.0
2   3  0.25  3.6         1.0         0.0
3   4 -4.10  1.3         1.0         0.0
4   5  0.00 -2.0         0.0         1.0
```

使用虚拟变量拟合某些统计模型时存在一些微妙之处。当您拥有不仅仅是简单数字列时，使用 Patsy（下一节的主题）可能更简单且更不容易出错。

## 12.2 使用 Patsy 创建模型描述

[Patsy](https://patsy.readthedocs.io/)是一个用于描述统计模型（尤其是线性模型）的 Python 库，它使用基于字符串的“公式语法”，受到 R 和 S 统计编程语言使用的公式语法的启发（但并非完全相同）。在安装 statsmodels 时会自动安装它：

```py
conda install statsmodels
```

Patsy 在为 statsmodels 指定线性模型方面得到很好的支持，因此我将重点介绍一些主要功能，以帮助您快速上手。Patsy 的*公式*是一种特殊的字符串语法，看起来像：

```py
y ~ x0 + x1
```

语法`a + b`并不意味着将`a`加到`b`，而是这些是为模型创建的*设计矩阵*中的*项*。`patsy.dmatrices`函数接受一个公式字符串以及一个数据集（可以是 DataFrame 或数组字典），并为线性模型生成设计矩阵：

```py
In [29]: data = pd.DataFrame({
 ....:     'x0': [1, 2, 3, 4, 5],
 ....:     'x1': [0.01, -0.01, 0.25, -4.1, 0.],
 ....:     'y': [-1.5, 0., 3.6, 1.3, -2.]})

In [30]: data
Out[30]: 
 x0    x1    y
0   1  0.01 -1.5
1   2 -0.01  0.0
2   3  0.25  3.6
3   4 -4.10  1.3
4   5  0.00 -2.0

In [31]: import patsy

In [32]: y, X = patsy.dmatrices('y ~ x0 + x1', data)
```

现在我们有：

```py
In [33]: y
Out[33]: 
DesignMatrix with shape (5, 1)
 y
 -1.5
 0.0
 3.6
 1.3
 -2.0
 Terms:
 'y' (column 0)

In [34]: X
Out[34]: 
DesignMatrix with shape (5, 3)
 Intercept  x0     x1
 1   1   0.01
 1   2  -0.01
 1   3   0.25
 1   4  -4.10
 1   5   0.00
 Terms:
 'Intercept' (column 0)
 'x0' (column 1)
 'x1' (column 2)
```

这些 Patsy `DesignMatrix`实例是带有附加元数据的 NumPy ndarrays：

```py
In [35]: np.asarray(y)
Out[35]: 
array([[-1.5],
 [ 0. ],
 [ 3.6],
 [ 1.3],
 [-2. ]])

In [36]: np.asarray(X)
Out[36]: 
array([[ 1.  ,  1.  ,  0.01],
 [ 1.  ,  2.  , -0.01],
 [ 1.  ,  3.  ,  0.25],
 [ 1.  ,  4.  , -4.1 ],
 [ 1.  ,  5.  ,  0.  ]])
```

您可能会想知道`Intercept`项是从哪里来的。这是线性模型（如普通最小二乘回归）的一个约定。您可以通过在模型中添加`+ 0`项来抑制截距：

```py
In [37]: patsy.dmatrices('y ~ x0 + x1 + 0', data)[1]
Out[37]: 
DesignMatrix with shape (5, 2)
 x0     x1
 1   0.01
 2  -0.01
 3   0.25
 4  -4.10
 5   0.00
 Terms:
 'x0' (column 0)
 'x1' (column 1)
```

Patsy 对象可以直接传递到像`numpy.linalg.lstsq`这样的算法中，该算法执行普通最小二乘回归：

```py
In [38]: coef, resid, _, _ = np.linalg.lstsq(X, y, rcond=None)
```

模型元数据保留在`design_info`属性中，因此您可以重新附加模型列名称到拟合系数以获得一个 Series，例如：

```py
In [39]: coef
Out[39]: 
array([[ 0.3129],
 [-0.0791],
 [-0.2655]])

In [40]: coef = pd.Series(coef.squeeze(), index=X.design_info.column_names)

In [41]: coef
Out[41]: 
Intercept    0.312910
x0          -0.079106
x1          -0.265464
dtype: float64
```

### Patsy 公式中的数据转换

您可以将 Python 代码混合到您的 Patsy 公式中；在评估公式时，库将尝试在封闭范围中找到您使用的函数：

```py
In [42]: y, X = patsy.dmatrices('y ~ x0 + np.log(np.abs(x1) + 1)', data)

In [43]: X
Out[43]: 
DesignMatrix with shape (5, 3)
 Intercept  x0  np.log(np.abs(x1) + 1)
 1   1                 0.00995
 1   2                 0.00995
 1   3                 0.22314
 1   4                 1.62924
 1   5                 0.00000
 Terms:
 'Intercept' (column 0)
 'x0' (column 1)
 'np.log(np.abs(x1) + 1)' (column 2)
```

一些常用的变量转换包括*标准化*（均值为 0，方差为 1）和*中心化*（减去均值）。Patsy 具有内置函数用于此目的：

```py
In [44]: y, X = patsy.dmatrices('y ~ standardize(x0) + center(x1)', data)

In [45]: X
Out[45]: 
DesignMatrix with shape (5, 3)
 Intercept  standardize(x0)  center(x1)
 1         -1.41421        0.78
 1         -0.70711        0.76
 1          0.00000        1.02
 1          0.70711       -3.33
 1          1.41421        0.77
 Terms:
 'Intercept' (column 0)
 'standardize(x0)' (column 1)
 'center(x1)' (column 2)
```

作为建模过程的一部分，您可以在一个数据集上拟合模型，然后基于另一个数据集评估模型。这可能是一个*保留*部分或稍后观察到的新数据。当应用诸如中心化和标准化之类的转换时，您在使用模型基于新数据形成预测时应当小心。这些被称为*有状态*转换，因为在转换新数据时必须使用原始数据集的统计数据，如均值或标准差。

`patsy.build_design_matrices`函数可以使用原始*样本内*数据的保存信息对新的*样本外*数据应用转换：

```py
In [46]: new_data = pd.DataFrame({
 ....:     'x0': [6, 7, 8, 9],
 ....:     'x1': [3.1, -0.5, 0, 2.3],
 ....:     'y': [1, 2, 3, 4]})

In [47]: new_X = patsy.build_design_matrices([X.design_info], new_data)

In [48]: new_X
Out[48]: 
[DesignMatrix with shape (4, 3)
 Intercept  standardize(x0)  center(x1)
 1          2.12132        3.87
 1          2.82843        0.27
 1          3.53553        0.77
 1          4.24264        3.07
 Terms:
 'Intercept' (column 0)
 'standardize(x0)' (column 1)
 'center(x1)' (column 2)]
```

因为 Patsy 公式中加号（`+`）并不表示加法，所以当您想按名称从数据集中添加列时，您必须将它们包装在特殊的`I`函数中：

```py
In [49]: y, X = patsy.dmatrices('y ~ I(x0 + x1)', data)

In [50]: X
Out[50]: 
DesignMatrix with shape (5, 2)
 Intercept  I(x0 + x1)
 1        1.01
 1        1.99
 1        3.25
 1       -0.10
 1        5.00
 Terms:
 'Intercept' (column 0)
 'I(x0 + x1)' (column 1)
```

Patsy 在`patsy.builtins`模块中还有几个内置转换。请查看在线文档以获取更多信息。

分类数据有一类特殊的转换，接下来我会解释。

### 分类数据和 Patsy

非数字数据可以以多种不同的方式转换为模型设计矩阵。本书不涉及这个主题的完整处理，最好是在统计课程中学习。

当您在 Patsy 公式中使用非数字术语时，默认情况下它们会被转换为虚拟变量。如果有一个截距，将会有一个级别被排除以避免共线性：

```py
In [51]: data = pd.DataFrame({
 ....:     'key1': ['a', 'a', 'b', 'b', 'a', 'b', 'a', 'b'],
 ....:     'key2': [0, 1, 0, 1, 0, 1, 0, 0],
 ....:     'v1': [1, 2, 3, 4, 5, 6, 7, 8],
 ....:     'v2': [-1, 0, 2.5, -0.5, 4.0, -1.2, 0.2, -1.7]
 ....: })

In [52]: y, X = patsy.dmatrices('v2 ~ key1', data)

In [53]: X
Out[53]: 
DesignMatrix with shape (8, 2)
 Intercept  key1[T.b]
 1          0
 1          0
 1          1
 1          1
 1          0
 1          1
 1          0
 1          1
 Terms:
 'Intercept' (column 0)
 'key1' (column 1)
```

如果从模型中省略截距，那么每个类别值的列将包含在模型设计矩阵中：

```py
In [54]: y, X = patsy.dmatrices('v2 ~ key1 + 0', data)

In [55]: X
Out[55]: 
DesignMatrix with shape (8, 2)
 key1[a]  key1[b]
 1        0
 1        0
 0        1
 0        1
 1        0
 0        1
 1        0
 0        1
 Terms:
 'key1' (columns 0:2)
```

数值列可以使用`C`函数解释为分类列：

```py
In [56]: y, X = patsy.dmatrices('v2 ~ C(key2)', data)

In [57]: X
Out[57]: 
DesignMatrix with shape (8, 2)
 Intercept  C(key2)[T.1]
 1             0
 1             1
 1             0
 1             1
 1             0
 1             1
 1             0
 1             0
 Terms:
 'Intercept' (column 0)
 'C(key2)' (column 1)
```

当您在模型中使用多个分类项时，情况可能会更加复杂，因为您可以包括形式为`key1:key2`的交互项，例如在方差分析（ANOVA）模型中使用：

```py
In [58]: data['key2'] = data['key2'].map({0: 'zero', 1: 'one'})

In [59]: data
Out[59]: 
 key1  key2  v1   v2
0    a  zero   1 -1.0
1    a   one   2  0.0
2    b  zero   3  2.5
3    b   one   4 -0.5
4    a  zero   5  4.0
5    b   one   6 -1.2
6    a  zero   7  0.2
7    b  zero   8 -1.7

In [60]: y, X = patsy.dmatrices('v2 ~ key1 + key2', data)

In [61]: X
Out[61]: 
DesignMatrix with shape (8, 3)
 Intercept  key1[T.b]  key2[T.zero]
 1          0             1
 1          0             0
 1          1             1
 1          1             0
 1          0             1
 1          1             0
 1          0             1
 1          1             1
 Terms:
 'Intercept' (column 0)
 'key1' (column 1)
 'key2' (column 2)

In [62]: y, X = patsy.dmatrices('v2 ~ key1 + key2 + key1:key2', data)

In [63]: X
Out[63]: 
DesignMatrix with shape (8, 4)
 Intercept  key1[T.b]  key2[T.zero]  key1[T.b]:key2[T.zero]
 1          0             1                       0
 1          0             0                       0
 1          1             1                       1
 1          1             0                       0
 1          0             1                       0
 1          1             0                       0
 1          0             1                       0
 1          1             1                       1
 Terms:
 'Intercept' (column 0)
 'key1' (column 1)
 'key2' (column 2)
 'key1:key2' (column 3)
```

Patsy 提供了其他转换分类数据的方法，包括具有特定顺序的项的转换。有关更多信息，请参阅在线文档。

## 12.3 statsmodels 简介

[statsmodels](http://www.statsmodels.org)是一个用于拟合许多种统计模型、执行统计检验以及数据探索和可视化的 Python 库。statsmodels 包含更多“经典”的频率统计方法，而贝叶斯方法和机器学习模型则在其他库中找到。

在 statsmodels 中找到的一些模型类型包括：

+   线性模型、广义线性模型和鲁棒线性模型

+   线性混合效应模型

+   方差分析（ANOVA）方法

+   时间序列过程和状态空间模型

+   广义矩估计法

在接下来的几页中，我们将使用 statsmodels 中的一些基本工具，并探索如何使用 Patsy 公式和 pandas DataFrame 对象的建模接口。如果您之前在 Patsy 讨论中没有安装 statsmodels，现在可以使用以下命令进行安装：

```py
conda install statsmodels
```

### 估计线性模型

statsmodels 中有几种线性回归模型，从更基本的（例如普通最小二乘法）到更复杂的（例如迭代重新加权最小二乘法）。

statsmodels 中的线性模型有两种不同的主要接口：基于数组和基于公式。可以通过以下 API 模块导入来访问这些接口：

```py
import statsmodels.api as sm
import statsmodels.formula.api as smf
```

为了展示如何使用这些方法，我们从一些随机数据生成一个线性模型。在 Jupyter 中运行以下代码：

```py
# To make the example reproducible
rng = np.random.default_rng(seed=12345)

def dnorm(mean, variance, size=1):
 if isinstance(size, int):
 size = size,
 return mean + np.sqrt(variance) * rng.standard_normal(*size)

N = 100
X = np.c_[dnorm(0, 0.4, size=N),
 dnorm(0, 0.6, size=N),
 dnorm(0, 0.2, size=N)]
eps = dnorm(0, 0.1, size=N)
beta = [0.1, 0.3, 0.5]

y = np.dot(X, beta) + eps
```

在这里，我写下了具有已知参数`beta`的“真实”模型。在这种情况下，`dnorm`是一个用于生成具有特定均值和方差的正态分布数据的辅助函数。现在我们有：

```py
In [66]: X[:5]
Out[66]: 
array([[-0.9005, -0.1894, -1.0279],
 [ 0.7993, -1.546 , -0.3274],
 [-0.5507, -0.1203,  0.3294],
 [-0.1639,  0.824 ,  0.2083],
 [-0.0477, -0.2131, -0.0482]])

In [67]: y[:5]
Out[67]: array([-0.5995, -0.5885,  0.1856, -0.0075, -0.0154])
```

通常使用截距项拟合线性模型，就像我们之前在 Patsy 中看到的那样。`sm.add_constant`函数可以向现有矩阵添加一个截距列：

```py
In [68]: X_model = sm.add_constant(X)

In [69]: X_model[:5]
Out[69]: 
array([[ 1.    , -0.9005, -0.1894, -1.0279],
 [ 1.    ,  0.7993, -1.546 , -0.3274],
 [ 1.    , -0.5507, -0.1203,  0.3294],
 [ 1.    , -0.1639,  0.824 ,  0.2083],
 [ 1.    , -0.0477, -0.2131, -0.0482]])
```

`sm.OLS`类可以拟合普通最小二乘线性回归：

```py
In [70]: model = sm.OLS(y, X)
```

模型的`fit`方法返回一个包含估计模型参数和其他诊断信息的回归结果对象：

```py
In [71]: results = model.fit()

In [72]: results.params
Out[72]: array([0.0668, 0.268 , 0.4505])
```

`results`上的`summary`方法可以打印出模型的诊断输出：

```py
In [73]: print(results.summary())
OLS Regression Results 
=================================================================================
======
Dep. Variable:                      y   R-squared (uncentered): 
 0.469
Model:                            OLS   Adj. R-squared (uncentered): 
 0.452
Method:                 Least Squares   F-statistic: 
 28.51
Date:                Wed, 12 Apr 2023   Prob (F-statistic):                    2.
66e-13
Time:                        13:09:20   Log-Likelihood:                         -
25.611
No. Observations:                 100   AIC: 
 57.22
Df Residuals:                      97   BIC: 
 65.04
Df Model:                           3 

Covariance Type:            nonrobust 

==============================================================================
 coef    std err          t      P>|t|      [0.025      0.975]
------------------------------------------------------------------------------
x1             0.0668      0.054      1.243      0.217      -0.040       0.174
x2             0.2680      0.042      6.313      0.000       0.184       0.352
x3             0.4505      0.068      6.605      0.000       0.315       0.586
==============================================================================
Omnibus:                        0.435   Durbin-Watson:                   1.869
Prob(Omnibus):                  0.805   Jarque-Bera (JB):                0.301
Skew:                           0.134   Prob(JB):                        0.860
Kurtosis:                       2.995   Cond. No.                         1.64
==============================================================================
Notes:
[1] R² is computed without centering (uncentered) since the model does not contai
n a constant.
[2] Standard Errors assume that the covariance matrix of the errors is correctly 
specified.
```

这里的参数名称已经被赋予了通用名称`x1, x2`等。假设所有模型参数都在一个 DataFrame 中：

```py
In [74]: data = pd.DataFrame(X, columns=['col0', 'col1', 'col2'])

In [75]: data['y'] = y

In [76]: data[:5]
Out[76]: 
 col0      col1      col2         y
0 -0.900506 -0.189430 -1.027870 -0.599527
1  0.799252 -1.545984 -0.327397 -0.588454
2 -0.550655 -0.120254  0.329359  0.185634
3 -0.163916  0.824040  0.208275 -0.007477
4 -0.047651 -0.213147 -0.048244 -0.015374
```

现在我们可以使用 statsmodels 的公式 API 和 Patsy 公式字符串：

```py
In [77]: results = smf.ols('y ~ col0 + col1 + col2', data=data).fit()

In [78]: results.params
Out[78]: 
Intercept   -0.020799
col0         0.065813
col1         0.268970
col2         0.449419
dtype: float64

In [79]: results.tvalues
Out[79]: 
Intercept   -0.652501
col0         1.219768
col1         6.312369
col2         6.567428
dtype: float64
```

注意 statsmodels 如何将结果返回为带有 DataFrame 列名称附加的 Series。在使用公式和 pandas 对象时，我们也不需要使用`add_constant`。

给定新的样本外数据，可以根据估计的模型参数计算预测值：

```py
In [80]: results.predict(data[:5])
Out[80]: 
0   -0.592959
1   -0.531160
2    0.058636
3    0.283658
4   -0.102947
dtype: float64
```

在 statsmodels 中有许多用于分析、诊断和可视化线性模型结果的附加工具，您可以探索。除了普通最小二乘法之外，还有其他类型的线性模型。

### 估计时间序列过程

statsmodels 中的另一类模型是用于时间序列分析的模型。其中包括自回归过程、卡尔曼滤波和其他状态空间模型以及多变量自回归模型。

让我们模拟一些具有自回归结构和噪声的时间序列数据。在 Jupyter 中运行以下代码：

```py
init_x = 4

values = [init_x, init_x]
N = 1000

b0 = 0.8
b1 = -0.4
noise = dnorm(0, 0.1, N)
for i in range(N):
 new_x = values[-1] * b0 + values[-2] * b1 + noise[i]
 values.append(new_x)
```

这个数据具有 AR(2)结构（两个*滞后*），参数为`0.8`和`-0.4`。当拟合 AR 模型时，您可能不知道要包括的滞后项的数量，因此可以使用一些更大数量的滞后项来拟合模型：

```py
In [82]: from statsmodels.tsa.ar_model import AutoReg

In [83]: MAXLAGS = 5

In [84]: model = AutoReg(values, MAXLAGS)

In [85]: results = model.fit()
```

结果中的估计参数首先是截距，接下来是前两个滞后的估计值：

```py
In [86]: results.params
Out[86]: array([ 0.0235,  0.8097, -0.4287, -0.0334,  0.0427, -0.0567])
```

这些模型的更深层细节以及如何解释它们的结果超出了我在本书中可以涵盖的范围，但在 statsmodels 文档中还有很多内容等待探索。

## 12.4 scikit-learn 简介

[scikit-learn](http://scikit-learn.org)是最广泛使用和信任的通用 Python 机器学习工具包之一。它包含广泛的标准监督和无监督机器学习方法，具有模型选择和评估工具，数据转换，数据加载和模型持久性。这些模型可用于分类，聚类，预测和其他常见任务。您可以像这样从 conda 安装 scikit-learn：

```py
conda install scikit-learn
```

有很多在线和印刷资源可供学习机器学习以及如何应用类似 scikit-learn 的库来解决实际问题。在本节中，我将简要介绍 scikit-learn API 风格。

scikit-learn 中的 pandas 集成在近年来显著改善，当您阅读本文时，它可能已经进一步改进。我鼓励您查看最新的项目文档。

作为本章的示例，我使用了一份来自 Kaggle 竞赛的[经典数据集](https://www.kaggle.com/c/titanic)，关于 1912 年*泰坦尼克号*上乘客生存率。我们使用 pandas 加载训练和测试数据集：

```py
In [87]: train = pd.read_csv('datasets/titanic/train.csv')

In [88]: test = pd.read_csv('datasets/titanic/test.csv')

In [89]: train.head(4)
Out[89]: 
 PassengerId  Survived  Pclass 
0            1         0       3  \
1            2         1       1 
2            3         1       3 
3            4         1       1 
 Name     Sex   Age  SibSp 
0                              Braund, Mr. Owen Harris    male  22.0      1  \
1  Cumings, Mrs. John Bradley (Florence Briggs Thayer)  female  38.0      1 
2                               Heikkinen, Miss. Laina  female  26.0      0 
3         Futrelle, Mrs. Jacques Heath (Lily May Peel)  female  35.0      1 
 Parch            Ticket     Fare Cabin Embarked 
0      0         A/5 21171   7.2500   NaN        S 
1      0          PC 17599  71.2833   C85        C 
2      0  STON/O2\. 3101282   7.9250   NaN        S 
3      0            113803  53.1000  C123        S 
```

像 statsmodels 和 scikit-learn 这样的库通常无法处理缺失数据，因此我们查看列，看看是否有包含缺失数据的列：

```py
In [90]: train.isna().sum()
Out[90]: 
PassengerId      0
Survived         0
Pclass           0
Name             0
Sex              0
Age            177
SibSp            0
Parch            0
Ticket           0
Fare             0
Cabin          687
Embarked         2
dtype: int64

In [91]: test.isna().sum()
Out[91]: 
PassengerId      0
Pclass           0
Name             0
Sex              0
Age             86
SibSp            0
Parch            0
Ticket           0
Fare             1
Cabin          327
Embarked         0
dtype: int64
```

在统计学和机器学习的示例中，一个典型的任务是根据数据中的特征预测乘客是否会生存。模型在*训练*数据集上拟合，然后在外样本*测试*数据集上进行评估。

我想使用`Age`作为预测变量，但它有缺失数据。有很多方法可以进行缺失数据插补，但我将使用训练数据集的中位数来填充两个表中的空值：

```py
In [92]: impute_value = train['Age'].median()

In [93]: train['Age'] = train['Age'].fillna(impute_value)

In [94]: test['Age'] = test['Age'].fillna(impute_value)
```

现在我们需要指定我们的模型。我添加一个名为`IsFemale`的列，作为`'Sex'`列的编码版本：

```py
In [95]: train['IsFemale'] = (train['Sex'] == 'female').astype(int)

In [96]: test['IsFemale'] = (test['Sex'] == 'female').astype(int)
```

然后我们决定一些模型变量并创建 NumPy 数组：

```py
In [97]: predictors = ['Pclass', 'IsFemale', 'Age']

In [98]: X_train = train[predictors].to_numpy()

In [99]: X_test = test[predictors].to_numpy()

In [100]: y_train = train['Survived'].to_numpy()

In [101]: X_train[:5]
Out[101]: 
array([[ 3.,  0., 22.],
 [ 1.,  1., 38.],
 [ 3.,  1., 26.],
 [ 1.,  1., 35.],
 [ 3.,  0., 35.]])

In [102]: y_train[:5]
Out[102]: array([0, 1, 1, 1, 0])
```

我不断言这是一个好模型或这些特征是否被正确设计。我们使用 scikit-learn 中的`LogisticRegression`模型并创建一个模型实例：

```py
In [103]: from sklearn.linear_model import LogisticRegression

In [104]: model = LogisticRegression()
```

我们可以使用模型的`fit`方法将此模型拟合到训练数据中：

```py
In [105]: model.fit(X_train, y_train)
Out[105]: LogisticRegression()
```

现在，我们可以使用`model.predict`为测试数据集进行预测：

```py
In [106]: y_predict = model.predict(X_test)

In [107]: y_predict[:10]
Out[107]: array([0, 0, 0, 0, 1, 0, 1, 0, 1, 0])
```

如果您有测试数据集的真实值，可以计算准确率百分比或其他错误度量：

```py
(y_true == y_predict).mean()
```

在实践中，模型训练通常存在许多额外的复杂层。许多模型具有可以调整的参数，并且有一些技术，如*交叉验证*可用于参数调整，以避免过度拟合训练数据。这通常可以提供更好的预测性能或对新数据的鲁棒性。

交叉验证通过拆分训练数据来模拟外样本预测。根据像均方误差这样的模型准确度得分，您可以对模型参数执行网格搜索。一些模型，如逻辑回归，具有内置交叉验证的估计器类。例如，`LogisticRegressionCV`类可以与一个参数一起使用，该参数指示在模型正则化参数`C`上执行多精细的网格搜索：

```py
In [108]: from sklearn.linear_model import LogisticRegressionCV

In [109]: model_cv = LogisticRegressionCV(Cs=10)

In [110]: model_cv.fit(X_train, y_train)
Out[110]: LogisticRegressionCV()
```

手动进行交叉验证，可以使用`cross_val_score`辅助函数，该函数处理数据拆分过程。例如，要对我们的模型进行四个不重叠的训练数据拆分进行交叉验证，我们可以这样做：

```py
In [111]: from sklearn.model_selection import cross_val_score

In [112]: model = LogisticRegression(C=10)

In [113]: scores = cross_val_score(model, X_train, y_train, cv=4)

In [114]: scores
Out[114]: array([0.7758, 0.7982, 0.7758, 0.7883])
```

默认的评分指标取决于模型，但可以选择一个明确的评分函数。交叉验证模型训练时间较长，但通常可以获得更好的模型性能。

## 12.5 结论

虽然我只是浅尝了一些 Python 建模库的表面，但有越来越多的框架适用于各种统计和机器学习，要么是用 Python 实现的，要么有 Python 用户界面。

这本书专注于数据整理，但还有许多其他专门用于建模和数据科学工具的书籍。一些优秀的书籍包括：

+   《Python 机器学习入门》作者 Andreas Müller 和 Sarah Guido（O'Reilly）

+   《Python 数据科学手册》作者 Jake VanderPlas（O'Reilly）

+   《从零开始的数据科学：Python 基础》作者 Joel Grus（O'Reilly）

+   《Python 机器学习》作者 Sebastian Raschka 和 Vahid Mirjalili（Packt Publishing）

+   《使用 Scikit-Learn、Keras 和 TensorFlow 进行实践机器学习》作者 Aurélien Géron（O'Reilly）

尽管书籍可以是学习的宝贵资源，但当底层的开源软件发生变化时，它们有时会变得过时。熟悉各种统计或机器学习框架的文档是一个好主意，以便了解最新功能和 API。
