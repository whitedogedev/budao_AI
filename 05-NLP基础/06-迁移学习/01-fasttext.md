
## 1.认识fasttext工具

---

### 学习目标

* 了解fasttext工具的作用.
* 了解fasttext工具的优势及其原因.
* 掌握fasttext的安装方法.

---


<center><img style="margin: 0px 0px 0px 0px" src="https://fasttext.cc/img/fasttext-logo-color-web.png" width="400px" height="auto"></center>



---


* 作为NLP工程领域常用的工具包, fasttext有两大作用:
	* 进行文本分类
	* 训练词向量

---

* fasttext工具包的优势:
	* 正如它的名字, 在保持较高精度的情况下, 快速的进行训练和预测是fasttext的最大优势.


---


* fasttext优势的原因:
	* fasttext工具包中内含的fasttext模型具有十分简单的网络结构.
	* 使用fasttext模型训练词向量时使用层次softmax结构, 来提升超多类别下的模型性能.
	* **由于fasttext模型过于简单无法捕捉词序特征, 因此会进行n-gram特征提取以弥补模型缺陷提升精度.**

---

* fasttext的安装:

```shell
$ git clone https://github.com/facebookresearch/fastText.git
$ cd fastText
# 使用pip安装python中的fasttext工具包
$ sudo pip install .
```

---

* 验证安装:

```python
Python 3.7.3 (default, Mar 27 2019, 22:11:17)
[GCC 7.3.0] :: Anaconda, Inc. on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import fasttext
>>>
```



---


## 2.进行文本分类

---

### 学习目标

* 了解什么是文本分类及其种类.
* 掌握fasttext工具进行文本分类的过程.
---


### 2.1 什么是文本分类

* 文本分类的是将文档（例如电子邮件，帖子，文本消息，产品评论等）分配给一个或多个类别. 当今文本分类的实现多是使用机器学习方法从训练数据中提取分类规则以进行分类, 因此构建文本分类器需要带标签的数据.

---

### 2.2 文本分类的种类

* 二分类: 
	* 文本被分类两个类别中, 往往这两个类别是对立面, 比如: 判断一句评论是好评还是差评.
* 单标签多分类: 
	* 文本被分入到多个类别中, 且每条文本只能属于某一个类别(即被打上某一个标签), 比如: 输入一个人名, 判断它是来自哪个国家的人名. 
* 多标签多分类: 
	* 文本被分入到多个类别中, 但每条文本可以属于多个类别(即被打上多个标签), 比如: 输入一段描述, 判断可能是和哪些兴趣爱好有关, 一段描述中可能即讨论了美食, 又讨论了游戏爱好.

---



### 2.3 使用fasttext工具进行文本分类的过程

* 第一步: 获取数据
* 第二步: 训练集与验证集的划分
* 第三步: 训练模型
* 第四步: 使用模型进行预测并评估
* 第五步: 模型调优
* 第六步: 模型保存与重加载 


---


#### 第一步: 获取数据

```shell
# 获取烹饪相关的数据集, 它是由facebook AI实验室提供的演示数据集
$ wget https://dl.fbaipublicfiles.com/fasttext/data/cooking.stackexchange.tar.gz && tar xvzf cooking.stackexchange.tar.gz
# 查看数据的前10条
$ head cooking.stackexchange.txt
```

---

```text
__label__sauce __label__cheese How much does potato starch affect a cheese sauce recipe?
__label__food-safety __label__acidity Dangerous pathogens capable of growing in acidic environments
__label__cast-iron __label__stove How do I cover up the white spots on my cast iron stove?
__label__restaurant Michelin Three Star Restaurant; but if the chef is not there
__label__knife-skills __label__dicing Without knife skills, how can I quickly and accurately dice vegetables?
__label__storage-method __label__equipment __label__bread What's the purpose of a bread box?
__label__baking __label__food-safety __label__substitutions __label__peanuts how to seperate peanut oil from roasted peanuts at home?
__label__chocolate American equivalent for British chocolate terms
__label__baking __label__oven __label__convection Fan bake vs bake
__label__sauce __label__storage-lifetime __label__acidity __label__mayonnaise Regulation and balancing of readymade packed mayonnaise and other sauces
```

> * 数据说明:
	* cooking.stackexchange.txt中的每一行都包含一个标签列表，后跟相应的文档, 标签列表以类似"\_\_label\_\_sauce \_\_label\_\_cheese"的形式展现, 代表有两个标签sauce和cheese, 所有标签\_\_label\_\_均以前缀开头，这是fastText识别标签或单词的方式. 标签之后的一段话就是文本信息.如: How much does potato starch affect a cheese sauce recipe?

---

#### 第二步: 训练集与验证集的划分

```python
# 查看数据总数
$ wc cooking.stackexchange.txt 
```
```text
  15404  169582 1401900 cooking.stackexchange.txt 
```

```shell
# 12404条数据作为训练数据
$ head -n 12404 cooking.stackexchange.txt > cooking.train
# 3000条数据作为验证数据
$ tail -n 3000 cooking.stackexchange.txt > cooking.valid
```

---

#### 第三步: 训练模型

```python
# 代码运行在python解释器中
# 导入fasttext
>>> import fasttext
# 使用fasttext的train_supervised方法进行文本分类模型的训练
>>> model = fasttext.train_supervised(input="cooking.train")

# 获得结果
Read 0M words
# 不重复的词汇总数
Number of words:  14543
# 标签总数
Number of labels: 735
# Progress: 训练进度, 因为我们这里显示的是最后的训练完成信息, 所以进度是100%
# words/sec/thread: 每个线程每秒处理的平均词汇数
# lr: 当前的学习率, 因为训练完成所以学习率是0
# avg.loss: 训练过程的平均损失 
# ETA: 预计剩余训练时间, 因为已训练完成所以是0
Progress: 100.0% words/sec/thread:   60162 lr:  0.000000 avg.loss: 10.056812 ETA:   0h 0m 0s
```


---

#### 第四步: 使用模型进行预测并评估

```python
# 使用模型预测一段输入文本, 通过我们常识, 可知预测是正确的, 但是对应预测概率并不大
>>> model.predict("Which baking dish is best to bake a banana bread ?")
# 元组中的第一项代表标签, 第二项代表对应的概率
(('__label__baking',), array([0.06550845]))
```
```
# 通过我们常识可知预测是错误的
>>> model.predict("Why not put knives in the dishwasher?")
(('__label__food-safety',), array([0.07541209]))
```

```
# 为了评估模型到底表现如何, 我们在3000条的验证集上进行测试
>>> model.test("cooking.valid")
# 元组中的每项分别代表, 验证集样本数量, 精度以及召回率 
# 我们看到模型精度和召回率表现都很差, 接下来我们讲学习如何进行优化.
(3000, 0.124, 0.0541)
```


---

#### 第五步: 模型调优


* 原始数据处理:

```text
# 通过查看数据, 我们发现数据中存在许多标点符号与单词相连以及大小写不统一, 
# 这些因素对我们最终的分类目标没有益处, 反是增加了模型提取分类规律的难度,
# 因此我们选择将它们去除或转化

# 处理前的部分数据
__label__fish Arctic char available in North-America
__label__pasta __label__salt __label__boiling When cooking pasta in salted water how much of the salt is absorbed?
__label__coffee Emergency Coffee via Chocolate Covered Coffee Beans?
__label__cake Non-beet alternatives to standard red food dye
__label__cheese __label__lentils Could cheese "halt" the tenderness of cooking lentils?
__label__asian-cuisine __label__chili-peppers __label__kimchi __label__korean-cuisine What kind of peppers are used in Gochugaru ()?
__label__consistency Pavlova Roll failure
__label__eggs __label__bread What qualities should I be looking for when making the best French Toast?
__label__meat __label__flour __label__stews __label__braising Coating meat in flour before browning, bad idea?
__label__food-safety Raw roast beef on the edge of safe?
__label__pork __label__food-identification How do I determine the cut of a pork steak prior to purchasing it?
```

```shell
# 通过服务器终端进行简单的数据预处理
# 使标点符号与单词分离并统一使用小写字母
>> cat cooking.stackexchange.txt | sed -e "s/\([.\!?,'/()]\)/ \1 /g" | tr "[:upper:]" "[:lower:]" > cooking.preprocessed.txt
>> head -n 12404 cooking.preprocessed.txt > cooking.train
>> tail -n 3000 cooking.preprocessed.txt > cooking.valid
```

```text
# 处理后的部分数据
__label__fish arctic char available in north-america
__label__pasta __label__salt __label__boiling when cooking pasta in salted water how much of the salt is absorbed ?
__label__coffee emergency coffee via chocolate covered coffee beans ?
__label__cake non-beet alternatives to standard red food dye
__label__cheese __label__lentils could cheese "halt" the tenderness of cooking lentils ?
__label__asian-cuisine __label__chili-peppers __label__kimchi __label__korean-cuisine what kind of peppers are used in gochugaru  (  )  ?
__label__consistency pavlova roll failure
__label__eggs __label__bread what qualities should i be looking for when making the best french toast ?
__label__meat __label__flour __label__stews __label__braising coating meat in flour before browning ,  bad idea ?
__label__food-safety raw roast beef on the edge of safe ?
__label__pork __label__food-identification how do i determine the cut of a pork steak prior to purchasing it ?
```


* 数据处理后进行训练并测试:

```python
# 重新训练
>>> model = fasttext.train_supervised(input="cooking.train")
Read 0M words

# 不重复的词汇总数减少很多, 因为之前会把带大写字母或者与标点符号相连接的单词都认为是新的单词
Number of words:  8952
Number of labels: 735

# 我们看到平均损失有所下降
Progress: 100.0% words/sec/thread:   65737 lr:  0.000000 avg.loss:  9.966091 ETA:   0h 0m 0s

# 重新测试
>>> model.test("cooking.valid")
# 我们看到精度和召回率都有所提升
(3000, 0.161, 0.06962663975782038)
```

---

* 增加训练轮数:

```python
# 设置train_supervised方法中的参数epoch来增加训练轮数, 默认的轮数是5次
# 增加轮数意味着模型能够有更多机会在有限数据中调整分类规律, 当然这也会增加训练时间
>>> model = fasttext.train_supervised(input="cooking.train", epoch=25)
Read 0M words
Number of words:  8952
Number of labels: 735

# 我们看到平均损失继续下降
Progress: 100.0% words/sec/thread:   66283 lr:  0.000000 avg.loss:  7.203885 ETA:   0h 0m 0s

>>> model.test("cooking.valid")
# 我们看到精度已经提升到了42%, 召回率提升至18%.
(3000, 0.4206666666666667, 0.1819230214790255)
```

---

* 调整学习率:

```python
# 设置train_supervised方法中的参数lr来调整学习率, 默认的学习率大小是0.1
# 增大学习率意味着增大了梯度下降的步长使其在有限的迭代步骤下更接近最优点
>>> model = fasttext.train_supervised(input="cooking.train", lr=1.0, epoch=25)
Read 0M words
Number of words:  8952
Number of labels: 735

# 平均损失继续下降
Progress: 100.0% words/sec/thread:   66027 lr:  0.000000 avg.loss:  4.278283 ETA:   0h 0m 0s

>>> model.test("cooking.valid")
# 我们看到精度已经提升到了47%, 召回率提升至20%.
(3000, 0.47633333333333333, 0.20599682860025947)
```

---

* 增加n-gram特征:

```python
# 设置train_supervised方法中的参数wordNgrams来添加n-gram特征, 默认是1, 也就是没有n-gram特征
# 我们这里将其设置为2意味着添加2-gram特征, 这些特征帮助模型捕捉前后词汇之间的关联, 更好的提取分类规则用于模型分类, 当然这也会增加模型训时练占用的资源和时间.
>>> model = fasttext.train_supervised(input="cooking.train", lr=1.0, epoch=25, wordNgrams=2)
Read 0M words
Number of words:  8952
Number of labels: 735

# 平均损失继续下降
Progress: 100.0% words/sec/thread:   65084 lr:  0.000000 avg.loss:  3.189422 ETA:   0h 0m 0s

>>> model.test("cooking.valid")
# 我们看到精度已经提升到了49%, 召回率提升至21%.
(3000, 0.49233333333333335, 0.2129162462159435)
```


---

* 修改损失计算方式:

```python
# 随着我们不断的添加优化策略, 模型训练速度也越来越慢
# 为了能够提升fasttext模型的训练效率, 减小训练时间
# 设置train_supervised方法中的参数loss来修改损失计算方式(等效于输出层的结构), 默认是softmax层结构
# 我们这里将其设置为'hs', 代表层次softmax结构, 意味着输出层的结构(计算方式)发生了变化, 将以一种更低复杂度的方式来计算损失.
>>> model = fasttext.train_supervised(input="cooking.train", lr=1.0, epoch=25, wordNgrams=2, loss='hs')
Read 0M words
Number of words:  8952
Number of labels: 735
Progress: 100.0% words/sec/thread: 1341740 lr:  0.000000 avg.loss:  2.225962 ETA:   0h 0m 0s
>>> model.test("cooking.valid")
# 我们看到精度和召回率稍有波动, 但训练时间却缩短到仅仅几秒
(3000, 0.483, 0.20887991927346114)
```

---

* 自动超参数调优:

```python
# 手动调节和寻找超参数是非常困难的, 因为参数之间可能相关, 并且不同数据集需要的超参数也不同, 
# 因此可以使用fasttext的autotuneValidationFile参数进行自动超参数调优.
# autotuneValidationFile参数需要指定验证数据集所在路径, 它将在验证集上使用随机搜索方法寻找可能最优的超参数.
# 使用autotuneDuration参数可以控制随机搜索的时间, 默认是300s, 根据不同的需求, 我们可以延长或缩短时间.
# 验证集路径'cooking.valid', 随机搜索600秒
>>> model = fasttext.train_supervised(input='cooking.train', autotuneValidationFile='cooking.valid', autotuneDuration=600)

Progress: 100.0% Trials:   38 Best score:  0.376170 ETA:   0h 0m 0s
Training again with best arguments
Read 0M words
Number of words:  8952
Number of labels: 735
Progress: 100.0% words/sec/thread:   63791 lr:  0.000000 avg.loss:  1.888165 ETA:   0h 0m 0s
```

---


* 实际生产中多标签多分类问题的损失计算方式:

```python
# 针对多标签多分类问题, 使用'softmax'或者'hs'有时并不是最佳选择, 因为我们最终得到的应该是多个标签, 而softmax却只能最大化一个标签. 
# 所以我们往往会选择为每个标签使用独立的二分类器作为输出层结构, 
# 对应的损失计算方式为'ova'表示one vs all.
# 这种输出层的改变意味着我们在统一语料下同时训练多个二分类模型,
# 对于二分类模型来讲, lr不宜过大, 这里我们设置为0.2
>>> model = fasttext.train_supervised(input="cooking.train", lr=0.2, epoch=25, wordNgrams=2, loss='ova')
Read 0M words
Number of words:  8952
Number of labels: 735
Progress: 100.0% words/sec/thread:   65044 lr:  0.000000 avg.loss:  7.713312 ETA:   0h 0m 0s 

# 我们使用模型进行单条样本的预测, 来看一下它的输出结果.
# 参数k代表指定模型输出多少个标签, 默认为1, 这里设置为-1, 意味着尽可能多的输出.
# 参数threshold代表显示的标签概率阈值, 设置为0.5, 意味着显示概率大于0.5的标签
>>> model.predict("Which baking dish is best to bake a banana bread ?", k=-1, threshold=0.5)

# 我看到根据输入文本, 输出了它的三个最有可能的标签
((u'__label__baking', u'__label__bananas', u'__label__bread'), array([1.00000, 0.939923, 0.592677]))
```


---


#### 第六步: 模型保存与重加载

```python
# 使用model的save_model方法保存模型到指定目录
# 你可以在指定目录下找到model_cooking.bin文件
>>> model.save_model("./model_cooking.bin")

# 使用fasttext的load_model进行模型的重加载
>>> model = fasttext.load_model("./model_cooking.bin")

# 重加载后的模型使用方法和之前完全相同
>>> model.predict("Which baking dish is best to bake a banana bread ?", k=-1, threshold=0.5)
((u'__label__baking', u'__label__bananas', u'__label__bread'), array([1.00000, 0.939923, 0.592677]))
```

---


### 2.4 小节总结

* 学习了什么是文本分类:
	* 文本分类的是将文档（例如电子邮件，帖子，文本消息，产品评论等）分配给一个或多个类别. 当今文本分类的实现多是使用机器学习方法从训练数据中提取分类规则以进行分类, 因此构建文本分类器需要带标签的数据.
	
* 文本分类的种类:
  * 二分类:
  	* 文本被分类两个类别中, 往往这两个类别是对立面, 比如: 判断一句评论是好评还是差评.
  * 单标签多分类:
  	* 文本被分入到多个类别中, 且每条文本只能属于某一个类别(即被打上某一个标签), 比如: 输入一个人名, 判断它是来自哪个国家的人名.
  * 多标签多分类:
  	* 文本被分人到多个类别中, 但每条文本可以属于多个类别(即被打上多个标签), 比如: 输入一段描述, 判断可能是和哪些兴趣爱好有关, 一段描述中可能即讨论了美食, 又太讨论了游戏爱好.

* 使用fasttext工具进行文本分类的过程:
  * 第一步: 获取数据
  * 第二步: 训练集与验证集的划分
  * 第三步: 训练模型
  * 第四步: 使用模型进行预测并评估
  * 第五步: 模型调优
  * 第六步: 模型保存与重加载

---



## 3.训练词向量

---

### 学习目标

* 了解词向量的相关知识.
* 掌握fasttext工具训练词向量的过程. 

---


* 词向量的相关知识:
	* 用向量表示文本中的词汇(或字符)是现代机器学习中最流行的做法, 这些向量能够很好的捕捉语言之间的关系, 从而提升基于词向量的各种NLP任务的效果.

---

### 3.1 使用fasttext工具训练词向量的过程

* 第一步: 获取数据
* 第二步: 训练词向量
* 第三步: 模型超参数设定
* 第四步: 模型效果检验
* 第五步: 模型的保存与重加载

---

#### 第一步: 获取数据

```shell
# 在这里, 我们将研究英语维基百科的部分网页信息, 它的大小在300M左右
# 这些语料已经被准备好, 我们可以通过Matt Mahoney的网站下载.
# 首先创建一个存储数据的文件夹data
$ mkdir data
# 使用wget下载数据的zip压缩包, 它将存储在data目录中
$ wget -c http://mattmahoney.net/dc/enwik9.zip -P data
# 使用unzip解压, 如果你的服务器中还没有unzip命令, 请使用: yum install unzip -y
# 解压后在data目录下会出现enwik9的文件夹
$ unzip data/enwik9.zip -d data
```

---


* 查看原始数据:

```shell
$ head -10 data/enwik9

# 原始数据将输出很多包含XML/HTML格式的内容, 这些内容并不是我们需要的
<mediawiki xmlns="http://www.mediawiki.org/xml/export-0.3/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.mediawiki.org/xml/export-0.3/ http://www.mediawiki.org/xml/export-0.3.xsd" version="0.3" xml:lang="en">
  <siteinfo>
    <sitename>Wikipedia</sitename>
    <base>http://en.wikipedia.org/wiki/Main_Page</base>
    <generator>MediaWiki 1.6alpha</generator>
    <case>first-letter</case>
      <namespaces>
      <namespace key="-2">Media</namespace>
      <namespace key="-1">Special</namespace>
      <namespace key="0" />
```

---

* 原始数据处理:

```shell
# 使用wikifil.pl文件处理脚本来清除XML/HTML格式的内容
# 注: wikifil.pl文件已为大家提供
$ perl wikifil.pl data/enwik9 > data/fil9
```

---

* 查看预处理后的数据:

```shell
# 查看前80个字符
head -c 80 data/fil9

# 输出结果为由空格分割的单词
 anarchism originated as a term of abuse first used against early working class
```

---

####  第二步: 训练词向量

```python
# 代码运行在python解释器中
# 导入fasttext
>>> import fasttext
# 使用fasttext的train_unsupervised(无监督训练方法)进行词向量的训练
# 它的参数是数据集的持久化文件路径'data/fil9'
>>> model = fasttext.train_unsupervised('data/fil9')


# 有效训练词汇量为124M, 共218316个单词
Read 124M words
Number of words:  218316
Number of labels: 0
Progress: 100.0% words/sec/thread:   53996 lr:  0.000000 loss:  0.734999 ETA:   0h 0m
```

---

* 查看单词对应的词向量:

```python
# 通过get_word_vector方法来获得指定词汇的词向量
>>> model.get_word_vector("the")

array([-0.03087516,  0.09221972,  0.17660329,  0.17308897,  0.12863874,
        0.13912526, -0.09851588,  0.00739991,  0.37038437, -0.00845221,
        ...
       -0.21184735, -0.05048715, -0.34571868,  0.23765688,  0.23726143],
      dtype=float32)
```

---


#### 第三步: 模型超参数设定


```python
# 在训练词向量过程中, 我们可以设定很多常用超参数来调节我们的模型效果, 如:
# 无监督训练模式: 'skipgram' 或者 'cbow', 默认为'skipgram', 在实践中，skipgram模式在利用子词方面比cbow更好.
# 词嵌入维度dim: 默认为100, 但随着语料库的增大, 词嵌入的维度往往也要更大.
# 数据循环次数epoch: 默认为5, 但当你的数据集足够大, 可能不需要那么多次.
# 学习率lr: 默认为0.05, 根据经验, 建议选择[0.01，1]范围内.
# 使用的线程数thread: 默认为12个线程, 一般建议和你的cpu核数相同.

>>> model = fasttext.train_unsupervised('data/fil9', "cbow", dim=300, epoch=1, lr=0.1, thread=8)

Read 124M words
Number of words:  218316
Number of labels: 0
Progress: 100.0% words/sec/thread:   49523 lr:  0.000000 avg.loss:  1.777205 ETA:   0h 0m 0s
```

---


#### 第四步: 模型效果检验

```python
# 检查单词向量质量的一种简单方法就是查看其邻近单词, 通过我们主观来判断这些邻近单词是否与目标单词相关来粗略评定模型效果好坏.

# 查找"运动"的邻近单词, 我们可以发现"体育网", "运动汽车", "运动服"等. 
>>> model.get_nearest_neighbors('sports')

[(0.8414610624313354, 'sportsnet'), (0.8134572505950928, 'sport'), (0.8100415468215942, 'sportscars'), (0.8021156787872314, 'sportsground'), (0.7889881134033203, 'sportswomen'), (0.7863013744354248, 'sportsplex'), (0.7786710262298584, 'sporty'), (0.7696356177330017, 'sportscar'), (0.7619683146476746, 'sportswear'), (0.7600985765457153, 'sportin')]


# 查找"音乐"的邻近单词, 我们可以发现与音乐有关的词汇.
>>> model.get_nearest_neighbors('music')

[(0.8908010125160217, 'emusic'), (0.8464668393135071, 'musicmoz'), (0.8444250822067261, 'musics'), (0.8113634586334229, 'allmusic'), (0.8106718063354492, 'musices'), (0.8049437999725342, 'musicam'), (0.8004694581031799, 'musicom'), (0.7952923774719238, 'muchmusic'), (0.7852965593338013, 'musicweb'), (0.7767147421836853, 'musico')]

# 查找"小狗"的邻近单词, 我们可以发现与小狗有关的词汇.
>>> model.get_nearest_neighbors('dog')

[(0.8456876873970032, 'catdog'), (0.7480780482292175, 'dogcow'), (0.7289096117019653, 'sleddog'), (0.7269964218139648, 'hotdog'), (0.7114801406860352, 'sheepdog'), (0.6947550773620605, 'dogo'), (0.6897546648979187, 'bodog'), (0.6621081829071045, 'maddog'), (0.6605004072189331, 'dogs'), (0.6398137211799622, 'dogpile')]

```

---



#### 第五步: 模型的保存与重加载

```python
# 使用save_model保存模型
>>> model.save_model("fil9.bin")

# 使用fasttext.load_model加载模型
>>> model = fasttext.load_model("fil9.bin")
>>> model.get_word_vector("the")

array([-0.03087516,  0.09221972,  0.17660329,  0.17308897,  0.12863874,
        0.13912526, -0.09851588,  0.00739991,  0.37038437, -0.00845221,
        ...
       -0.21184735, -0.05048715, -0.34571868,  0.23765688,  0.23726143],
      dtype=float32)
```



---


### 3.2 小节总结

* 学习了词向量的相关知识:
	* 用向量表示文本中的词汇(或字符)是现代机器学习中最流行的做法, 这些向量能够很好的捕捉语言之间的关系, 从而提升基于词向量的各种NLP任务的效果.

	---

* 使用fasttext工具训练词向量的过程:
	* 第一步: 获取数据
	* 第二步: 训练词向量
	* 第三步: 模型超参数设定
	* 第四步: 模型效果检验
	* 第五步: 模型的保存与重加载

---


## 4.词向量迁移


---


### 学习目标

* 了解什么是词向量迁移.
* 了解fasttext工具中有哪些可迁移的词向量模型.
*  掌握如何使用fasttext进行词向量模型迁移.


---

* 什么是词向量迁移:
	* 使用在大型语料库上已经进行训练完成的词向量模型.

---

* fasttext工具中可以提供的可迁移的词向量:
	* fasttext提供了157种语言的在CommonCrawl和Wikipedia语料上进行训练的可迁移词向量模型, 它们采用CBOW模式进行训练, 词向量维度为300维. 可通过该地址查看具体语言词向量模型: [https://fasttext.cc/docs/en/crawl-vectors.html](https://fasttext.cc/docs/en/crawl-vectors.html)
	* fasttext提供了294种语言的在Wikipedia语料上进行训练的可迁移词向量模型, 它们采用skipgram模式进行训练, 词向量维度同样是300维. 可通过该地址查看具体语言词向量模型: [https://fasttext.cc/docs/en/pretrained-vectors.html](https://fasttext.cc/docs/en/pretrained-vectors.html)

---

### 4.1 如何使用fasttext进行词向量模型迁移

* 第一步: 下载词向量模型压缩的bin.gz文件
* 第二步: 解压bin.gz文件到bin文件
* 第三步: 加载bin文件获取词向量
* 第四步: 利用邻近词进行效果检验

---

#### 第一步: 下载词向量模型压缩的bin.gz文件

```shell
# 这里我们以迁移在CommonCrawl和Wikipedia语料上进行训练的中文词向量模型为例:
# 下载中文词向量模型(bin.gz文件)
wget https://dl.fbaipublicfiles.com/fasttext/vectors-crawl/cc.zh.300.bin.gz
```

---

#### 第二步: 解压bin.gz文件到bin文件

```shell
# 使用gunzip进行解压, 获取cc.zh.300.bin文件
gunzip cc.zh.300.bin.gz
```

---

#### 第三步: 加载bin文件获取词向量

```python
# 加载模型
>>> model = fasttext.load_model("cc.zh.300.bin")

# 查看前100个词汇(这里的词汇是广义的, 可以是中文符号或汉字))
>>> model.words[:100]
['，', '的', '。', '</s>', '、', '是', '一', '在', '：', '了', '（', '）', "'", '和', '不', '有', '我', ',', ')', '(', '“', '”', '也', '人', '个', ':', '中', '.', '就', '他', '》', '《', '-', '你', '都', '上', '大', '！', '这', '为', '多', '与', '章', '「', '到', '」', '要', '？', '被', '而', '能', '等', '可以', '年', '；', '|', '以', '及', '之', '公司', '对', '中国', '很', '会', '小', '但', '我们', '最', '更', '/', '1', '三', '新', '自己', '可', '2', '或', '次', '好', '将', '第', '种', '她', '…', '3', '地', '對', '用', '工作', '下', '后', '由', '两', '使用', '还', '又', '您', '?', '其', '已']


# 使用模型获得'音乐'这个名词的词向量
>>> model.get_word_vector("音乐")
array([-6.81843981e-02,  3.84048335e-02,  4.63239700e-01,  6.11658543e-02,
        9.38086119e-03, -9.63955745e-02,  1.28141120e-01, -6.51574507e-02,
        ...
        3.13430429e-02, -6.43611327e-02,  1.68979481e-01, -1.95011273e-01],
      dtype=float32)	

```


---

#### 第四步: 利用邻近词进行效果检验

```python
# 以'音乐'为例, 返回的邻近词基本上与音乐都有关系, 如乐曲, 音乐会, 声乐等.
>>> model.get_nearest_neighbors("音乐")
[(0.6703276634216309, '乐曲'), (0.6569967269897461, '音乐人'), (0.6565821170806885, '声乐'), (0.6557438373565674, '轻音乐'), (0.6536258459091187, '音乐家'), (0.6502416133880615, '配乐'), (0.6501686573028564, '艺术'), (0.6437276005744934, '音乐会'), (0.639589250087738, '原声'), (0.6368917226791382, '音响')]


# 以'美术'为例, 返回的邻近词基本上与美术都有关系, 如艺术, 绘画, 霍廷霄(满城尽带黄金甲的美术师)等.
>>> model.get_nearest_neighbors("美术")
[(0.724744975566864, '艺术'), (0.7165924310684204, '绘画'), (0.6741853356361389, '霍廷霄'), (0.6470299363136292, '纯艺'), (0.6335071921348572, '美术家'), (0.6304370164871216, '美院'), (0.624431312084198, '艺术类'), (0.6244068741798401, '陈浩忠'), (0.62302166223526, '美术史'), (0.621710479259491, '环艺系')]


# 以'周杰伦'为例, 返回的邻近词基本上与明星有关系, 如杰伦, 周董, 陈奕迅等.
>>> model.get_nearest_neighbors("周杰伦")
[(0.6995140910148621, '杰伦'), (0.6967097520828247, '周杰倫'), (0.6859776377677917, '周董'), (0.6381043195724487, '陈奕迅'), (0.6367626190185547, '张靓颖'), (0.6313326358795166, '张韶涵'), (0.6271176338195801, '谢霆锋'), (0.6188404560089111, '周华健'), (0.6184280514717102, '林俊杰'), (0.6143589019775391, '王力宏')]

```

---


### 4.2 小节总结
* 学习了什么是词向量迁移:
	* 使用在大型语料库上已经进行训练完成的词向量模型.

	---

* 学习了fasttext工具中可以提供的可迁移的词向量:
	* fasttext提供了157种语言的在CommonCrawl和Wikipedia语料上进行训练的可迁移词向量模型, 它们采用CBOW模式进行训练, 词向量维度为300维. 可通过该地址查看具体语言词向量模型: https://fasttext.cc/docs/en/crawl-vectors.html
	* fasttext提供了294种语言的在Wikipedia语料上进行训练的可迁移词向量模型, 它们采用skipgram模式进行训练, 词向量维度同样是300维. 可通过该地址查看具体语言词向量模型: https://fasttext.cc/docs/en/pretrained-vectors.html

	---

* 如何使用fasttext进行词向量模型迁移:
	* 第一步: 下载词向量模型压缩的bin.gz文件
	* 第二步: 解压bin.gz文件到bin文件
	* 第三步: 加载bin文件获取词向量
	* 第四步: 利用邻近词进行效果检验

---
