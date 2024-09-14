# 前言

> 原文：[`wesmckinney.com/book/preface`](https://wesmckinney.com/book/preface)
>
> 译者：[飞龙](https://github.com/wizardforcel)
>
> 协议：[CC BY-NC-SA 4.0](http://creativecommons.org/licenses/by-nc-sa/4.0/)


> 此开放访问网络版本的《Python 数据分析第三版》现已作为[印刷版和数字版](https://amzn.to/3DyLaJc)的伴侣提供。如果您发现任何勘误，请[在此处报告](https://oreilly.com/catalog/0636920519829/errata)。请注意，由 Quarto 生成的本站点的某些方面与 O'Reilly 的印刷版和电子书版本的格式不同。
> 
> 如果您发现本书的在线版本有用，请考虑[订购纸质版](https://amzn.to/3DyLaJc)或[无 DRM 的电子书](https://www.ebooks.com/en-us/book/210644288/python-for-data-analysis/wes-mckinney/?affId=WES398681F)以支持作者。本网站的内容不得复制或再生产。代码示例采用 MIT 许可，可在 GitHub 或 Gitee 上找到。

本书的第一版于 2012 年出版，当时 Python 的开源数据分析库，尤其是 pandas，非常新且快速发展。到了 2016 年和 2017 年写第二版时，我不仅需要将书更新为 Python 3.6（第一版使用 Python 2.7），还需要更新 pandas 在过去五年中发生的许多变化。现在是 2022 年，Python 语言的变化较少（我们现在使用 Python 3.10，3.11 将于 2022 年底发布），但 pandas 仍在不断发展。

在这第三版中，我的目标是将内容与当前版本的 Python、NumPy、pandas 和其他项目保持同步，同时对于讨论近几年出现的较新的 Python 项目保持相对保守。由于这本书已成为许多大学课程和职业人士的重要资源，我将尽量避免讨论可能在一两年内过时的主题。这样，纸质副本在 2023 年、2024 年甚至更久以后也不会太难理解。

第三版的一个新特性是托管在我的网站上的开放访问在线版本，网址为[`wesmckinney.com/book`](https://wesmckinney.com/book)，可作为印刷版和数字版的所有者的资源和便利。我打算保持那里的内容相对及时更新，因此如果您拥有纸质书并遇到某些问题，请在那里查看最新的内容更改。

## 本书中使用的约定

本书中使用以下排版约定：

*斜体*

指示新术语、URL、电子邮件地址、文件名和文件扩展名。

`等宽`

用于程序清单，以及段落内引用程序元素，如变量或函数名、数据库、数据类型、环境变量、语句和关键字。

`等宽粗体`

显示用户应按照字面意思键入的命令或其他文本。

<等宽斜体>

显示应替换为用户提供的值或由上下文确定的值的文本。

提示：

此元素表示提示或建议。

注意：

此元素表示一般说明。

警告：

此元素表示警告或注意事项。

## 使用代码示例

您可以在本书的 GitHub 存储库中找到每章的数据文件和相关材料，网址为[`github.com/wesm/pydata-book`](https://github.com/wesm/pydata-book)，该存储库在 Gitee 上有镜像（供无法访问 GitHub 的用户使用），网址为[`gitee.com/wesmckinn/pydata-book`](https://gitee.com/wesmckinn/pydata-book)。

这本书旨在帮助您完成工作。一般来说，如果本书提供示例代码，您可以在程序和文档中使用它。除非您复制了代码的大部分内容，否则无需征得我们的许可。例如，编写一个使用本书中几个代码块的程序不需要许可。销售或分发 O'Reilly 图书中的示例需要许可。通过引用本书回答问题并引用示例代码不需要许可。将本书中大量示例代码合并到产品文档中需要许可。

我们感谢，但不要求署名。署名通常包括标题、作者、出版商和 ISBN。例如：“*Python for Data Analysis* by Wes McKinney（O'Reilly）。版权所有 2022 年 Wes McKinney，978-1-098-10403-0。”

如果您觉得您对代码示例的使用超出了合理使用范围或上述许可，请随时通过 permissions@oreilly.com 与我们联系。

## 致谢

这项工作是多年来与世界各地许多人进行富有成果的讨论和合作的成果。我想感谢其中的一些人。

## 追悼：约翰·D·亨特（1968-2012）

我们亲爱的朋友和同事约翰·D·亨特在 2012 年 8 月 28 日与结肠癌搏斗后去世。这发生在我完成本书第一版最终手稿后不久。

约翰在 Python 科学和数据社区的影响和遗产难以估量。除了在 21 世纪初开发 matplotlib（当时 Python 并不那么流行）之外，他还帮助塑造了一代关键的开源开发者文化，这些开发者已经成为我们现在经常视为理所当然的 Python 生态系统的支柱。

我很幸运在 2010 年 1 月早期与约翰建立了联系，就在发布 pandas 0.1 后不久。他的启发和指导帮助我在最黑暗的时刻推动前进，实现了我对 pandas 和 Python 作为一流数据分析语言的愿景。

John 与 Fernando Pérez 和 Brian Granger 非常亲近，他们是 IPython、Jupyter 和 Python 社区中许多其他倡议的先驱。我们曾希望一起合作写一本书，但最终我成为了拥有最多空闲时间的人。我相信他会为我们在过去九年中所取得的成就感到自豪，无论是作为个人还是作为一个社区。

## 致谢第三版（2022 年）

自从我开始写这本书的第一版以来已经有十多年了，自从我最初作为 Python 程序员开始我的旅程以来已经有 15 年了。那时发生了很多变化！Python 已经从一个相对小众的数据分析语言发展成为最受欢迎、最广泛使用的语言，支持着数据科学、机器学习和人工智能工作的多数（如果不是大多数！）。

自 2013 年以来，我并没有积极参与 pandas 开源项目，但其全球开发者社区仍在蓬勃发展，成为以社区为中心的开源软件开发模式的典范。许多处理表格数据的“下一代”Python 项目直接模仿 pandas 的用户界面，因此该项目已经对 Python 数据科学生态系统未来的发展轨迹产生了持久的影响。

希望这本书能继续为想要学习如何在 Python 中处理数据的学生和个人提供宝贵的资源。

我特别感谢 O'Reilly 允许我在我的网站[`wesmckinney.com/book`](https://wesmckinney.com/book)上发布这本书的“开放获取”版本，希望它能触达更多人，并帮助扩大数据分析领域的机会。J.J. Allaire 在帮助我将这本书从 Docbook XML“移植”到[Quarto](https://quarto.org)时是一个救星，Quarto 是一个出色的新科学技术出版系统，适用于印刷和网络。

特别感谢我的技术审阅者 Paul Barry、Jean-Christophe Leyder、Abdullah Karasan 和 William Jamir，他们的详细反馈极大地提高了内容的可读性、清晰度和可理解性。

## 致谢第二版（2017 年）

距离我在 2012 年 7 月完成这本书第一版手稿已经快五年了。很多事情发生了变化。Python 社区已经大幅增长，围绕它的开源软件生态系统也蓬勃发展。

如果不是 pandas 核心开发者们不懈的努力，这本书的新版将不会存在，他们已经将这个项目及其用户社区发展成为 Python 数据科学生态系统的支柱之一。这些人包括但不限于 Tom Augspurger、Joris van den Bossche、Chris Bartak、Phillip Cloud、gfyoung、Andy Hayden、Masaaki Horikoshi、Stephan Hoyer、Adam Klein、Wouter Overmeire、Jeff Reback、Chang She、Skipper Seabold、Jeff Tratner 和 y-p。

在撰写这本第二版时，我要感谢 O'Reilly 的工作人员在写作过程中耐心地帮助我。其中包括 Marie Beaugureau、Ben Lorica 和 Colleen Toporek。我再次有幸得到 Tom Augspurger、Paul Barry、Hugh Brown、Jonathan Coe 和 Andreas Müller 等杰出的技术审阅者的帮助。谢谢。

这本书的第一版已经被翻译成许多外语，包括中文、法语、德语、日语、韩语和俄语。翻译所有这些内容并让更广泛的受众获得是一项巨大且常常被忽视的工作。感谢您帮助更多世界上的人学习如何编程和使用数据分析工具。

在过去几年里，我很幸运地得到了 Cloudera 和 Two Sigma Investments 对我持续的开源开发工作的支持。随着开源软件项目相对于用户群体规模而言资源更加稀缺，企业为关键开源项目的开发提供支持变得越来越重要。这是正确的做法。

## 致谢第一版（2012）

如果没有许多人的支持，我很难写出这本书。

在 O'Reilly 的工作人员中，我非常感激我的编辑 Meghan Blanchette 和 Julie Steele，他们在整个过程中指导我。Mike Loukides 也在提案阶段与我合作，帮助使这本书成为现实。

我得到了许多人的技术审查。特别是 Martin Blais 和 Hugh Brown 在改进书中的示例、清晰度和组织方面提供了极大帮助。James Long，Drew Conway，Fernando Pérez，Brian Granger，Thomas Kluyver，Adam Klein，Josh Klein，Chang She 和 Stéfan van der Walt 分别审查了一个或多个章节，从许多不同的角度提供了有针对性的反馈。

我从数据社区的朋友和同事那里得到了许多出色的示例和数据集的创意，其中包括：Mike Dewar，Jeff Hammerbacher，James Johndrow，Kristian Lum，Adam Klein，Hilary Mason，Chang She 和 Ashley Williams。

当然，我要感谢许多开源科学 Python 社区的领导者，他们为我的开发工作奠定了基础，并在我写这本书时给予了鼓励：IPython 核心团队（Fernando Pérez，Brian Granger，Min Ragan-Kelly，Thomas Kluyver 等），John Hunter，Skipper Seabold，Travis Oliphant，Peter Wang，Eric Jones，Robert Kern，Josef Perktold，Francesc Alted，Chris Fonnesbeck 等等。还有许多其他人，无法一一列举。还有一些人在这个过程中提供了大量的支持、想法和鼓励：Drew Conway，Sean Taylor，Giuseppe Paleologo，Jared Lander，David Epstein，John Krowas，Joshua Bloom，Den Pilsworth，John Myles-White 等等。

我还要感谢一些在我成长过程中的人。首先是我的前 AQR 同事，多年来一直在我的 pandas 工作中支持我：Alex Reyfman，Michael Wong，Tim Sargen，Oktay Kurbanov，Matthew Tschantz，Roni Israelov，Michael Katz，Ari Levine，Chris Uga，Prasad Ramanan，Ted Square 和 Hoon Kim。最后，我的学术导师 Haynes Miller（MIT）和 Mike West（Duke）。

2014 年，我得到了 Phillip Cloud 和 Joris van den Bossche 的重要帮助，更新了书中的代码示例，并修复了由于 pandas 变化而导致的一些不准确之处。

在个人方面，Casey 在写作过程中提供了宝贵的日常支持，容忍我在本已过度忙碌的日程表上拼凑出最终草稿时的起起伏伏。最后，我的父母 Bill 和 Kim 教导我始终追随梦想，永不妥协。
