# 通过Anaconda安装环境

Python的开发环境中拥有诸如NumPy、Pandas、Matplotlib等功能齐全的库，能够为数据分析工作提供极大的便利，不过库的管理及版本问题不能让数据分析人员专注于数据分析。而是将大量的时间花费在解决包配置与包冲突等问题上。

基于上述需求，可以使用Anaconda进行开发，它是一个集成了大量常用扩展包的环境，能够避免包配置或兼容等各种问题。

## 1.1 Anaconda发行版本概述

Anaconda是一个可以便捷获取和管理包，同时对环境进行统一管理的发行版本，它包含了conda、Python在内的超过180个科学包及其依赖项。
Anaconda发行版本具有以下特点：

- (1) 包含了众多流行的科学、数学、工程和数据分析的Python库。
- (2) 完全开源和免费。
- (3) 额外的加速和优化是收费的，但对于学术用途，可以申请免费的Licensee
- (4) 全平台支持 Linux、Windows. Mac OS X,支持 Python 2.6. 2.7、3.4、3.5、3.6,可以自由切换。

人工智能的初学者也可以安装Anaconda进行学习。

## 1.2 安装 Anaconda

以Windows系统为例，接下来我们来看一下如何从Anaconda官方网站下载合适的安装包，并成功安装到计算机。

anaconda官网链接：[https://www.anaconda.com/download/](https://www.anaconda.com/download/)进入Anaconda的官方网站.如下图所示：

![anaconda下载](https://tva1.sinaimg.cn/large/007S8ZIlly1gf09u1pu89j318v0ocgpn.jpg)

上图展示了适合Windows平台下载的版本，大家选择合适的版本单击下载即可；这里，我们下载“Python3.7版本”下的“64位图形安装程序 ”。

下载完以后，就可以进行安装了。Anaconda的安装是比较简单的，直接按照提不选择下一步即可。为了避免不必要的麻烦,建议采用默认安装路径，在指定完安装路径后，继续单击按钮，窗口会提示是否勾选如下复选框选项，如下图所示。

![anaconda复选框](https://tva1.sinaimg.cn/large/007S8ZIlly1gf09u3c4spj318v0ocgpn.jpg)

在上图中，

- 第1个复选框表示是否允许将Anaconda添加到系统路径环境变量中，
- 第2个复选框表示Anaconda使用的Python版本是否为3.7。

勾选两个复选框,单击“Install”按钮，点至提示安装成功。安装完以后，在系统左下角的“开始”菜单 “所有程序”中找到Anaconda3文件夹，可以看到该目录下包含了多个组件，如下图所示。

![在这里插入图片描述](https://tva1.sinaimg.cn/large/007S8ZIlly1gfwgqelpdxj307r077t9b.jpg)



关于下图中Anaconda3目录下的组件说明如下：

-  (1 ) Anaconda Navigator：用于管理工具包和环境的图形用户界面，后续涉及的众多管理命令也可以在Navigator中手动实现。
-  (2 ) Anaconda Prompt: Anaconda 自带的命令行。
-  (3) Jupyter Notebook：基于Web的交互式计算环境，可以编辑易于人们阅读的文档，用
    于展示数据分析的过程。
-  (4 ) Spyder : 一个使用Python语言、跨平台的科学运算集成开发环境。

单击图中的"Anaconda Navigator"图标，若能够成功启动Anaconda Navigator,则说明安装成功，否则说明安装失败。Anaconda Navigator成功打开后的首页界面如下图所示。

![anaconda启动界面](https://tva1.sinaimg.cn/large/007S8ZIlly1gf09u2nd9uj31hb0gzgnm.jpg)

## 1.3 管理 Python 包

Anaconda集成了常用的扩展包，能够方便地对这些扩展包进行管理，比如安装和卸载包，这些操作都需要依赖conda。conda是一个在Windows、Mac OS和Linux上运行的开源软件包管理系统和环境管理系统，可以快速地安装、运行和更新软件包及其依赖项。

在Windows系统下，用户可以打开Anaconda Prompt工具，然后在Anaconda Prompt命令检测是否被安装，示例命令如下。

```python
>>>  (base)   C:XUsersXadmin >  conda一Version

conda4.5.4
```

一旦发现有，就会返回其当前的版本号。

注意：如果希望快速了解如何使用conda命令管理包，则可以在Anaconda"conda-h”或"conda-—help"命令来查看帮助文档。

conda命令的常见操作主要可以分为以下几种：

### 1.3.1 查看当前环境下包信息

使用“list"命令可以获取当前环境中已经安装的包信息，命令格式如下。

```
conda list
```

执行上述命令后，终端会显示当前环境已安装的包名及版本号。

### 1.3.2.查找包

使用search命令可以查找可供安装的包，命令格式如下

```
conda search --full-name 包的全名
```

上述命令中，--full-name为精确査找的参数.后面紧跟的是包的全名。例如，查找全名为“python”的包有哪些版本可供安装，示例命令如下。

```
conda search --full-name python
```

### 1.3.3 安装包

使用install命令可以安装包。如果希望在指定的环境中进行安装，则可以在install命令的后面显式地指定环境名称，命令格式如下。

```
conda install --name env_name package_name
```

上述命令中，env_name参数表示包安装的环境名称，package_name表示将要安装的包名称。例如、在Python 3环境中安装pandas包，示例命令如下。

```
conda install --name python3 pandas
```

如果要在当前的环境中安装包，则可以直接使用install命令进行安装，命令格式如下。

```
conda install package_name
```

执行上述命令，会在当前的环境下安装package_name包。
​若无法使用conda install命令进行安装时，则可以使用pip命令进行安装。值得一提的是，pip只是包管理器，它无法对环境进行管理，所以要想在指定的环境中使用pip安装包，需要先切换到指定环境中使用pip命令进行安装。pip命令格式如下。

```
pip install package_name
```

例如，使用pip命令安装名称为see的包，示例如下。

```
pip install see
```

### 1.3.4 卸载包

如果要在指定的环境中卸载包，则可以在指定环境下使用remove命令进行移除，命令格式如下。

```
conda  remove  --name   env_name   package_name
```

例如，卸载Python3环境下的pandas包，示例命令如下。

```
conda  remove  --name  python3  pandas
```

同样，如果要卸载当前环境中的包，可以直接使用remove命令进行卸载，命令格式如下。

```
conda  remove  package_name
```

### 1.3.5 更新包

更新当前环境下所有的包，可使用如下命令完成。

```
conda update --a11
```

如果只想更新某个包或某些包，则直接在update命令的后面加上包名即可，多个包之间使用空格隔开，示例命令如下。

```
conda update numpy  # 更新numpy包
conda update pandas numpy matplotlib   # 更新pandas numpy、matpLotLib包
```

注意：Miniconda是最小的conda安装环境，只包含最基本的python与conda以及相关的必需依赖项。对于空间要求严格的用户，Miniconda是一种选择，它只包含了最基本的库，其他的库需要自己手动安装。

## 1.4 启用JupyterNotebook	

Jupyter notebook（交互式笔记本）是一个支持实时代码、数学方程、可视化和Mark down的web应用程序，它支持40多种编程语言。对于数据分析来说，Jupyter notebook最大的优点是可以重现整个分析过程，并将说明文字、代码、图表、公式和结论都整合在一个文档中，用户可以通过电子邮件、Dropbox、GitHub和Jupyter Notebook  Viewer将分析结构分享给其他人。接下来，本节将针对Jupyter Notebook工具的启动进行详细的讲解。

只要当前的系统中安装了Anaconda环境，则默认就已经拥有了Jupyter Notebook,不需要再另行下载和安装。在Windows系统的“开始"菜单中，打开目录，找到并单击Jupyter Notebook”，会弹出下图的启动窗口。

![notebook启动1](https://tva1.sinaimg.cn/large/007S8ZIlly1gf09u18y7kj30qo0dcjuq.jpg)

同时，系统默认的浏览器会弹出下图所示页面。

![anaconda启动2](https://tva1.sinaimg.cn/large/007S8ZIlly1gf09u12ptcj30qo0dcjuq.jpg)

上图是浏览器中打开的Jupyter Notebook主界面，默认打开和保存的目录为C:\user\当前用户名。

除了上述的启动方式外，还可以用命令行打开，这种方式可以控制 Jupyter Notebook的显示和保存路径，推荐的启动方式。在命令提示符中先使用命令进入对的目录，然后在此目录下输入"Jupyter notebook“后按【Enter】键打开，这样一来，显示工程目录和保存ipynb文件都将在此目录下进行。









