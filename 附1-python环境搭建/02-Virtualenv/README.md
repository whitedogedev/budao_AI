# 创建隔离的Python环境

**`virtualenv`是创建隔离的Python环境的工具。从Python 3.3开始，它的一个子集已集成到[venv模块](https://docs.python.org/3/library/venv.html)下的标准库中 。但是请注意，该`venv`模块并未提供该库的所有功能（例如，无法创建引导脚本，无法为除主机python之外的其他python版本创建虚拟环境，不可重定位等）。一般而言，这样的工具仍然更喜欢使用virtualenv，因为它易于升级（通过pip），统一处理不同的Python版本以及一些更高级的功能。**

**`virtualenv`可以为您创建一个具有自己的安装目录的环境，该环境不与其他virtualenv环境共享库（也可以选择不访问全局安装的库）。**

