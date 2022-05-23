
## 1.认识BERT

---

### 学习目标

* 了解什么是BERT.
* 掌握BERT的架构.
* 掌握BERT的预训练任务.

---


### 1.1 什么是BERT

* BERT是2018年10月由Google AI研究院提出的一种预训练模型.
	* BERT的全称是Bidirectional Encoder Representation from Transformers.
	* BERT在机器阅读理解顶级水平测试SQuAD1.1中表现出惊人的成绩: 全部两个衡量指标上全面超越人类, 并且在11种不同NLP测试中创出SOTA表现. 包括将GLUE基准推高至80.4% (绝对改进7.6%), MultiNLI准确度达到86.7% (绝对改进5.6%). 成为NLP发展史上的里程碑式的模型成就.

---

### 1.2 BERT的架构

* 总体架构: 如下图所示, 最左边的就是BERT的架构图, 可以很清楚的看到BERT采用了Transformer Encoder block进行连接, 因为是一个典型的双向编码模型.

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu4w3so8j21660gy0y0.jpg" height="auto" width="auto"/>

---

* 从上面的架构图中可以看到, 宏观上BERT分三个主要模块.
	* 最底层黄色标记的Embedding模块.
	* 中间层蓝色标记的Transformer模块.
	* 最上层绿色标记的预微调模块.

---

* Embedding模块: BERT中的该模块是由三种Embedding共同组成而成, 如下图

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu60jyw2j21ec0hc423.jpg" height="auto" width="auto"/>


---

> * Token Embeddings 是词嵌入张量, 第一个单词是CLS标志, 可以用于之后的分类任务.

> * Segment Embeddings 是句子分段嵌入张量, 是为了服务后续的两个句子为输入的预训练任务.

> * Position Embeddings 是位置编码张量, 此处注意和传统的Transformer不同, 不是三角函数计算的固定位置编码, 而是通过学习得出来的.

> * **整个Embedding模块的输出张量就是这3个张量的直接加和结果.**

---


* 双向Transformer模块: BERT中只使用了经典Transformer架构中的Encoder部分, 完全舍弃了Decoder部分. 而两大预训练任务也集中体现在训练Transformer模块中.


---


* 预微调模块: 
	* 经过中间层Transformer的处理后, BERT的最后一层根据任务的不同需求而做不同的调整即可.
	* 比如对于sequence-level的分类任务, BERT直接取第一个[CLS] token 的final hidden state, 再加一层全连接层后进行softmax来预测最终的标签.


---

> * 对于不同的任务, 微调都集中在预微调模块, 几种重要的NLP微调任务架构图展示如下

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5ujmdmj20vq0swwi6.jpg" height="auto" width="auto"/>


---

> * 从上图中可以发现, 在面对特定任务时, 只需要对预微调层进行微调, 就可以利用Transformer强大的注意力机制来模拟很多下游任务, 并得到SOTA的结果. (句子对关系判断, 单文本主题分类, 问答任务(QA), 单句贴标签(NER))

---

> * 若干可选的超参数建议如下:

```text
Batch size: 16, 32
Learning rate (Adam): 5e-5, 3e-5, 2e-5
Epochs: 3, 4
```

---


### 1.3 BERT的预训练任务

* BERT包含两个预训练任务:
	* 任务一: Masked LM (带mask的语言模型训练)
	* 任务二: Next Sentence Prediction (下一句话预测任务)

---

* 任务一: Masked LM (带mask的语言模型训练)

	* 关于传统的语言模型训练, 都是采用left-to-right, 或者left-to-right + right-to-left结合的方式, 但这种单向方式或者拼接的方式提取特征的能力有限. 为此BERT提出一个深度双向表达模型(deep bidirectional representation). 即采用MASK任务来训练模型.

	* 1: 在原始训练文本中, 随机的抽取15%的token作为参与MASK任务的对象.

	* 2: 在这些被选中的token中, 数据生成器并不是把它们全部变成[MASK], 而是有下列3种情况.

		* 2.1: 在80%的概率下, 用[MASK]标记替换该token, 比如my dog is hairy -> my dog is [MASK]
		* 2.2: 在10%的概率下, 用一个随机的单词替换token, 比如my dog is hairy -> my dog is apple
		* 2.3: 在10%的概率下, 保持该token不变, 比如my dog is hairy -> my dog is hairy

	* 3: 模型在训练的过程中, 并不知道它将要预测哪些单词? 哪些单词是原始的样子? 哪些单词被遮掩成了[MASK]? 哪些单词被替换成了其他单词? 正是在这样一种高度不确定的情况下, 反倒逼着模型快速学习该token的分布式上下文的语义, 尽最大努力学习原始语言说话的样子. 同时因为原始文本中只有15%的token参与了MASK操作, 并不会破坏原语言的表达能力和语言规则.

---


* 任务二: Next Sentence Prediction (下一句话预测任务)

	* 在NLP中有一类重要的问题比如QA(Quention-Answer), NLI(Natural Language Inference), 需要模型能够很好的理解两个句子之间的关系, 从而需要在模型的训练中引入对应的任务. 在BERT中引入的就是Next Sentence Prediction任务. 采用的方式是输入句子对(A, B), 模型来预测句子B是不是句子A的真实的下一句话.

	* 1: 所有参与任务训练的语句都被选中作为句子A.

		* 1.1: 其中50%的B是原始文本中真实跟随A的下一句话. (标记为IsNext, 代表正样本)
		* 1.2: 其中50%的B是原始文本中随机抽取的一句话. (标记为NotNext, 代表负样本)

	* 2: 在任务二中, BERT模型可以在测试集上取得97%-98%的准确率.

---

### 1.4 小节总结

* 学习了什么是BERT.
	* BERT是一个基于Transformer Encoder的预训练语言模型.
	* BERT在11种NLP测试任务中创出SOAT表现.

* 学习了BERT的结构.
	* 最底层的Embedding模块, 包括Token Embeddings, Segment Embeddings, Position Embeddings.
	* 中间层的Transformer模块, 只使用了经典Transformer架构中的Encoder部分.
	* 最上层的预微调模块, 具体根据不同的任务类型来做相应的处理.

* 学习了BERT的两大预训练任务.
	* MLM任务(Masked Language Model), 在原始文本中随机抽取15%的token参与任务.
		* 在80%概率下, 用[MASK]替换该token.
		* 在10%概率下, 用一个随机的单词替换该token.
		* 在10%概率下, 保持该token不变.
	* NSP任务(Next Sentence Prediction), 采用的方式是输入句子对(A, B), 模型预测句子B是不是句子A的真实的下一句话.
		* 其中50%的B是原始文本中真实跟随A的下一句话.(标记为IsNext, 代表正样本)
		* 其中50%的B是原始文本中随机抽取的一句话. (标记为NotNext, 代表负样本)


---

---

---

## 2.BERT模型的优点和缺点?

---

### 学习目标

* 理解BERT模型的优点和原因.
* 理解BERT模型的缺点和原因.

---

### 2.1 BERT的优点

* 1: 通过预训练, 加上Fine-tunning, 在11项NLP任务上取得最优结果.

* 2: BERT的根基源于Transformer, 相比传统RNN更加高效, 可以并行化处理同时能捕捉长距离的语义和结构依赖.

* 3: BERT采用了Transformer架构中的Encoder模块, 不仅仅获得了真正意义上的bidirectional context, 而且为后续微调任务留出了足够的调整空间.

---

### 2.2 BERT的缺点

* 1: BERT模型过于庞大, 参数太多, 不利于资源紧张的应用场景, 也不利于上线的实时处理.

* 2: BERT目前给出的中文模型中, 是以字为基本token单位的, 很多需要词向量的应用无法直接使用. 同时该模型无法识别很多生僻词, 只能以UNK代替.

* 3: BERT中第一个预训练任务MLM中, [MASK]标记只在训练阶段出现, 而在预测阶段不会出现, 这就造成了一定的信息偏差, 因此训练时不能过多的使用[MASK], 否则会影响模型的表现.

* 4: 按照BERT的MLM任务中的约定, 每个batch数据中只有15%的token参与了训练, 被模型学习和预测, 所以BERT收敛的速度比left-to-right模型要慢很多(left-to-right模型中每一个token都会参与训练).

---

### 2.3 小节总结

* 学习了BERT模型的3个优点:
    * 在11个NLP任务上取得SOAT成绩.
    * 利用了Transformer的并行化能力以及长语句捕捉语义依赖和结构依赖.
    * BERT实现了双向Transformer并为后续的微调任务留出足够的空间.

* 学习了BERT模型的4个缺点:
    * BERT模型太大, 太慢.
    * BERT模型中的中文模型是以字为基本token单位的, 无法利用词向量, 无法识别生僻词.
    * BERT模型中的MLM任务, [MASK]标记在训练阶段出现, 预测阶段不出现, 这种偏差会对模型有一定影响.
    * BERT模型的MLM任务, 每个batch只有15%的token参与了训练, 造成大量文本数据的"无用", 收敛速度慢, 需要的算力和算时都大大提高.

---

---

---


## 3.BERT的MLM任务中为什么采用了80%, 10%, 10%的策略?

---

### 学习目标

* 理解在MLM任务中采用80%, 10%, 10%策略的原因.

---

### 3.1 MLM任务中的策略约定分析

* 1: 首先, 如果所有参与训练的token被100%的[MASK], 那么在fine-tunning的时候所有单词都是已知的, 不存在[MASK], 那么模型就只能根据其他token的信息和语序结构来预测当前词, 而无法利用到这个词本身的信息, 因为它们从未出现在训练过程中, 等于模型从未接触到它们的信息, 等于整个语义空间损失了部分信息. 采用80%的概率下应用[MASK], 既可以让模型去学着预测这些单词, 又以20%的概率保留了语义信息展示给模型.

* 2: 保留下来的信息如果全部使用原始token, 那么模型在预训练的时候可能会偷懒, 直接照抄当前token信息. 采用10%概率下random token来随机替换当前token, 会让模型不能去死记硬背当前的token, 而去尽力学习单词周边的语义表达和远距离的信息依赖, 尝试建模完整的语言信息.

* 3: 最后再以10%的概率保留原始的token, 意义就是保留语言本来的面貌, 让信息不至于完全被遮掩, 使得模型可以"看清"真实的语言面貌.

---

### 3.2 小节总结

* BERT中MLM任务中的[MASK]是以一种显示的方式告诉模型"这个词我不告诉你, 你自己从上下文里猜", 非常类似于同学们在做完形填空. 如果[MASK]以外的部分全部都用原始token, 模型会学习到"如果当前词是[MASK], 就根据其他词的信息推断这个词; 如果当前词是一个正常的单词, 就直接照抄". 这样一来, 到了fine-tunning阶段, 所有单词都是正常单词了, 模型就会照抄所有单词, 不再提取单词之间的依赖关系了.

* BERT中MLM任务以10%的概率填入random token, 就是让模型时刻处于"紧张情绪"中, 让模型搞不清楚当前看到的token是真实的单词还是被随机替换掉的单词, 这样模型在任意的token位置就只能把当前token的信息和上下文信息结合起来做综合的判断和建模. 这样一来, 到了fine-tunning阶段, 模型也会同时提取这两方面的信息, 因为模型"心理很紧张", 它不知道当前看到的这个token, 所谓的"正常单词"到底有没有"提前被动过手脚".

---

---

---


## 4.长文本预测任务如果想用BERT来实现, 要如何构造训练样本?

---

### 学习目标

* 掌握利用BERT处理长文本的任务如何构造训练样本.

---

### 4.1 BERT处理长文本的方法

* 首选要明确一点, BERT预训练模型所接收的最大sequence长度是512.

* 那么对于长文本(文本长度超过512的句子), 就需要特殊的方式来构造训练样本. 核心就是如何进行截断.
    * 1: head-only方式: 这是只保留长文本头部信息的截断方式, 具体为保存前510个token (要留两个位置给[CLS]和[SEP]).
    * 2: tail-only方式: 这是只保留长文本尾部信息的截断方式, 具体为保存最后510个token (要留两个位置给[CLS]和[SEP]).
    * 3: head+only方式: 选择前128个token和最后382个token (文本总长度在800以内), 或者前256个token和最后254个token (文本总长度大于800).

---

### 4.2 小节总结

* 学习了长文本处理如果要利用BERT的话, 需要进行截断处理.
    * 第一种方式就是只保留前面510个token.
    * 第二种方式就是只保留后面510个token.
    * 第三种方式就是前后分别保留一部分token, 总数是510.



----

---

【因为bert、GPT等模型都是通过Transformer构建完成，以下对Transformer难点再次总结】

----

----



## 5.Transformer的结构是什么样的? 各个子模块各有什么作用?

---

### 学习目标

* 掌握Encoder模块的结构和作用
* 掌握Decoder模块的结构和作用
* 掌握其他模块的结构和作用

---

### 5.1 Encoder模块

* Encoder模块的结构和作用:
	* 经典的Transformer结构中的Encoder模块包含6个Encoder Block.
	* 每个Encoder Block包含一个多头自注意力层, 和一个前馈全连接层.

---

* 关于Encoder Block:
	* 在Transformer架构中, 6个一模一样的Encoder Block层层堆叠在一起, 共同组成完整的Encoder, 因此剖析一个Block就可以对整个Encoder的内部结构有清晰的认识.

---

* 多头自注意力层(self-attention):

> * 首先来看self-attention的计算规则图:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5otqnoj20340650sm.jpg" height="auto" width="auto"/>

---

> * 上述attention可以被描述为将query和key-value键值对的一组集合映射到输出, 输出被计算为values的加权和, 其中分配给每个value的权重由query与对应key的相似性函数计算得来. 这种attention的形式被称为Scaled Dot-Product Attention, 对应的数学公式形式如下:


<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5vvm7vj20hk04q3yh.jpg" height="auto" width="auto"/> 

---

> * 所谓的多头self-attention层, 则是先将Q, K, V经过参数矩阵进行映射, 再做self-attention, 最后将结果拼接起来送入一个全连接层即可.


<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5zmpbvj20bm0htjrw.jpg" height="auto" width="auto"/>


---

> * 上述的多头self-attention, 对应的数学公式形式如下:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5xdta3j20v807ejs2.jpg" height="auto" width="auto"/>

---

> * 多头self-attention层的作用: 实验结果表明, Multi-head可以在更细致的层面上提取不同head的特征, 总体计算量和单一head相同的情况下, 提取特征的效果更佳.


---


* 前馈全连接层模块

	* 前馈全连接层模块, 由两个线性变换组成, 中间有一个Relu激活函数, 对应的数学公式形式如下:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5pv3rnj20h203c0sn.jpg" height="auto" width="auto"/>


---

> * 注意: 原版论文中的前馈全连接层, 输入和输出的维度均为d_model = 512, 层内的连接维度d_ff = 2048, 均采用4倍的大小关系.

---

> * 前馈全连接层的作用: 单纯的多头注意力机制并不足以提取到理想的特征, 因此增加全连接层来提升网络的能力.


---


### 5.2 Decoder模块

* Decoder模块的结构和作用:
	* 经典的Transformer结构中的Decoder模块包含6个Decoder Block.
	* 每个Decoder Block包含三个子层.
		* 一个多头self-attention层
		* 一个Encoder-Decoder attention层
		* 一个前馈全连接层

---

* Decoder Block中的多头self-attention层
	* Decoder中的多头self-attention层与Encoder模块一致, 但需要注意的是Decoder模块的多头self-attention需要做look-ahead-mask, 因为在预测的时候"不能看见未来的信息", 所以要将当前的token和之后的token全部mask.

---

* Decoder Block中的Encoder-Decoder attention层
	* 这一层区别于自注意力机制的Q = K = V, 此处矩阵Q来源于Decoder端经过上一个Decoder Block的输出, 而矩阵K, V则来源于Encoder端的输出, 造成了Q != K = V的情况. 
	* 这样设计是为了让Decoder端的token能够给予Encoder端对应的token更多的关注.

---

* Decoder Block中的前馈全连接层
	* 此处的前馈全连接层和Encoder模块中的完全一样.

---

* Decoder Block中有2个注意力层的作用: 多头self-attention层是为了拟合Decoder端自身的信息, 而Encoder-Decoder attention层是为了整合Encoder和Decoder的信息.


---


### 5.3 Add & Norm模块

* Add & Norm模块接在每一个Encoder Block和Decoder Block中的每一个子层的后面. 具体来说Add表示残差连接, Norm表示LayerNorm.
	* 对于每一个Encoder Block, 里面的两个子层后面都有Add & Norm.
	* 对于每一个Decoder Block, 里面的三个子层后面都有Add & Norm.
	* 具体的数学表达形式为: LayerNorm(x + Sublayer(x)), 其中Sublayer(x)为子层的输出.

---

* Add残差连接的作用: 和其他神经网络模型中的残差连接作用一致, 都是为了将信息传递的更深, 增强模型的拟合能力. 试验表明残差连接的确增强了模型的表现.

---

* Norm的作用: 随着网络层数的额增加, 通过多层的计算后参数可能会出现过大, 过小, 方差变大等现象, 这会导致学习过程出现异常, 模型的收敛非常慢. 因此对每一层计算后的数值进行规范化可以提升模型的表现.


---


### 5.4 位置编码器Positional Encoding

* Transformer中直接采用正弦函数和余弦函数来编码位置信息, 如下图所示:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu6030juj20hi05ajri.jpg" height="auto" width="auto"/>

---

* 需要注意: 三角函数应用在此处的一个重要的优点, 因为对于任意的PE(pos+k), 都可以表示为PE(pos)的线性函数, 大大方便计算. 而且周期性函数不受序列长度的限制, 也可以增强模型的泛化能力.

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5xpsx4j20i404ct8v.jpg" height="auto" width="auto"/>

---

### 5.5 小节总结

* Encoder模块
	* 经典的Transformer架构中的Encoder模块包含6个Encoder Block.
	* 每个Encoder Block包含两个子模块, 分别是多头自注意力层, 和前馈全连接层.
		* 多头自注意力层采用的是一种Scaled Dot-Product Attention的计算方式, 实验结果表明, Mul
		ti-head可以在更细致的层面上提取不同head的特征, 比单一head提取特征的效果更佳.
		* 前馈全连接层是由两个全连接层组成, 线性变换中间增添一个Relu激活函数, 具体的维度采用4倍关系, 即多头自注意力的d_model=512, 则层内的变换维度d_ff=2048.


* 2: Decoder模块
	* 经典的Transformer架构中的Decoder模块包含6个Decoder Block.
	* 每个Decoder Block包含3个子模块, 分别是多头自注意力层, Encoder-Decoder Attention层, 和前馈全连接层.
		* 多头自注意力层采用和Encoder模块一样的Scaled Dot-Product Attention的计算方式, 最大的
		区别在于需要添加look-ahead-mask, 即遮掩"未来的信息".
		* Encoder-Decoder Attention层和上一层多头自注意力层最主要的区别在于Q != K = V, 矩阵Q来源于上一层Decoder Block的输出, 同时K, V来源于Encoder端的输出.
		* 前馈全连接层和Encoder中完全一样.


* 3: Add & Norm模块
	* Add & Norm模块接在每一个Encoder Block和Decoder Block中的每一个子层的后面.
	* 对于每一个Encoder Block, 里面的两个子层后面都有Add & Norm.
	* 对于每一个Decoder Block, 里面的三个子层后面都有Add & Norm.
	* Add表示残差连接, 作用是为了将信息无损耗的传递的更深, 来增强模型的拟合能力.
	* Norm表示LayerNorm, 层级别的数值标准化操作, 作用是防止参数过大过小导致的学习过程异常
	, 模型收敛特别慢的问题.


* 4: 位置编码器Positional Encoding
	* Transformer中采用三角函数来计算位置编码.
	* 因为三角函数是周期性函数, 不受序列长度的限制, 而且这种计算方式可以对序列中不同位置的编码的重要程度同等看待.


---

---

---


## 6.Transformer结构中的Decoder端具体输入是什么? 在训练阶段和预测阶段一致吗?

---

### 学习目标

* 掌握Transformer结构中的Decoder端的输入张量特点和含义.
* 掌握Decoder在训练阶段的输入是什么.
* 掌握Decoder在预测阶段的输入是什么.

---

### 6.1 Decoder端的输入解析

* Decoder端的架构: Transformer原始论文中的Decoder模块是由N=6个相同的Decoder Block堆叠而成, 其中每一个Block是由3个子模块构成, 分别是多头self-attention模块, Encoder-Decoder attention模块, 前馈全连接层模块.

---

* **6个Block的输入不完全相同:**
	* **最下面的一层Block接收的输入是经历了MASK之后的Decoder端的输入 + Encoder端的输出.**
	* **其他5层Block接收的输入模式一致, 都是前一层Block的输出 + Encoder端的输出.**

---

* Decoder在训练阶段的输入解析:
	* 从第二层Block到第六层Block的输入模式一致, 无需特殊处理, 都是固定操作的循环处理.
	* 聚焦在第一层的Block上: 训练阶段每一个time step的输入是上一个time step的输入加上真实标签序列向后移一位. 具体来说, 假设现在的真实标签序列等于"How are you?", 当time step=1时, 输入张量为一个特殊的token, 比如"SOS"; 当time step=2时, 输入张量为"SOS How"; 当time step=3时, 输入张量为"SOS How are", 以此类推...
	* 注意: 在真实的代码实现中, 训练阶段不会这样动态输入, 而是一次性的把目标序列全部输入给第一层的Block, 然后通过多头self-attention中的MASK机制对序列进行同样的遮掩即可.

---

* Decoder在预测阶段的输入解析:
	* 同理于训练阶段, 预测时从第二层Block到第六层Block的输入模式一致, 无需特殊处理, 都是固定操作的循环处理.
	* 聚焦在第一层的Block上: 因为每一步的输入都会有Encoder的输出张量, 因此这里不做特殊讨论, 只专注于纯粹从Decoder端接收的输入. 预测阶段每一个time step的输入是从time step=0, input_tensor="SOS"开始, 一直到上一个time step的预测输出的累计拼接张量. 具体来说: 
		* 当time step=1时, 输入的input_tensor="SOS", 预测出来的输出值是output_tensor="What";
		* 当time step=2时, 输入的input_tensor="SOS What", 预测出来的输出值是output_tensor="is";
		* 当time step=3时, 输入的input_tensor="SOS What is", 预测出来的输出值是output_tensor="the";
		* 当time step=4时, 输入的input_tensor="SOS What is the", 预测出来的输出值是output_tensor="matter";
		* 当time step=5时, 输入的input_tensor="SOS What is the matter", 预测出来的输出值是output_tensor="?";
		* 当time step=6时, 输入的input_tensor="SOS What is the matter ?", 预测出来的输出值是output_tensor="EOS", 代表句子的结束符, 说明解码结束, 预测结束.

---


### 6.2 小节总结

* 1: 在Transformer结构中的Decoder模块的输入, 区分于不同的Block, 最底层的Block输入有其特殊的地方. 第二层到第六层的输入一致, 都是上一层的输出和Encoder的输出.

* 2: 最底层的Block在训练阶段, 每一个time step的输入是上一个time step的输入加上真实标签序列向后移一位. 具体来看, 就是每一个time step的输入序列会越来越长, 不断的将之前的输入融合进来.

* 3: 最底层的Block在训练阶段, 真实的代码实现中, 采用的是MASK机制来模拟输入序列不断添加的过程.

* 4: 最底层的Block在预测阶段, 每一个time step的输入是从time step=0开始, 一直到上一个time step的预测值的累积拼接张量. 具体来看, 也是随着每一个time step的输入序列会越来越长. 相比于训练阶段最大的不同是这里不断拼接进来的token是每一个time step的预测值, 而不是训练阶段每一个time step取得的groud truth值.

---

---

---


## 7.Transformer中一直强调的self-attention是什么? 为什么能发挥如此大的作用? 计算的时候如果不使用三元组(Q, K, V), 而仅仅使用(Q, V)或者(K, V)或者(V)行不行?

---

### 学习目标

* 掌握self-attention的机制和原理.
* 掌握为什么要使用三元组(Q, K, V)来计算self-attention.

---

### 7.1 self-attention的机制和原理

* self-attention是一种通过自身和自身进行关联的attention机制, 从而得到更好的representation来表达自身.


* self-attention是attention机制的一种特殊情况:
	* 在self-attention中, Q=K=V, 序列中的每个单词(token)都和该序列中的其他所有单词(token)进行attention规则的计算.


* attention机制计算的特点在于, 可以直接跨越一句话中不同距离的token, 可以远距离的学习到序列的知识依赖和语序结构.

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5ypo7cj21160n40tt.jpg" height="auto" width="auto"/>

---

> * 从上图中可以看到, self-attention可以远距离的捕捉到语义层面的特征(its的指代对象是Law).

> * 应用传统的RNN, LSTM, 在获取长距离语义特征和结构特征的时候, 需要按照序列顺序依次计算, 距离越远的联系信息的损耗越大, 有效提取和捕获的可能性越小.

> * 但是应用self-attention时, 计算过程中会直接将句子中任意两个token的联系通过一个计算步骤直接联系起来, 


---

* 关于self-attention为什么要使用(Q, K, V)三元组而不是其他形式:
	* 首先一条就是从分析的角度看, 查询Query是一条独立的序列信息, 通过关键词Key的提示作用, 得到最终语义的真实值Value表达, 数学意义更充分, 完备.
	* 这里不使用(K, V)或者(V)没有什么必须的理由, 也没有相关的论文来严格阐述比较试验的结果差异, 所以可以作为开放性问题未来去探索, 只要明确在经典self-attention实现中用的是三元组就好.

---

### 7.2 小节总结

* self-attention机制的重点是使用三元组(Q, K, V)参与规则运算, 这里面Q=K=V.

* **self-attention最大的优势是可以方便有效的提取远距离依赖的特征和结构信息, 不必向RNN那样依次计算产生传递损耗.**

* 关于self-attention采用三元组的原因, 经典实现的方式数学意义明确, 理由充分, 至于其他方式的可行性暂时没有论文做充分的对比试验研究.



---

---

---


## 8.Transformer为什么需要进行Multi-head Attention? Multi-head Attention的计算过程是什么?

---

### 学习目标

* 掌握Transformer中应用多头注意力的原因.
* 掌握Transformer中多头注意力的计算方式.

---

### 8.1 采用Multi-head Attention的原因

* 1: 原始论文中提到进行Multi-head Attention的原因是将模型分为多个头, 可以形成多个子空间, 让模型去关注不同方面的信息, 最后再将各个方面的信息综合起来得到更好的效果.

* 2: 多个头进行attention计算最后再综合起来, 类似于CNN中采用多个卷积核的作用, 不同的卷积核提取不同的特征, 关注不同的部分, 最后再进行融合.

* 3: 直观上讲, 多头注意力有助于神经网络捕捉到更丰富的特征信息.

---

### 8.2 Multi-head Attention的计算方式

* 1: Multi-head Attention和单一head的Attention唯一的区别就在于, 其对特征张量的最后一个维度进行了分割, 一般是对词嵌入的embedding_dim=512进行切割成head=8, 这样每一个head的嵌入维度就是512/8=64, 后续的Attention计算公式完全一致, 只不过是在64这个维度上进行一系列的矩阵运算而已.

* 2: 在head=8个头上分别进行注意力规则的运算后, 简单采用拼接concat的方式对结果张量进行融合就得到了Multi-head Attention的计算结果.

---

### 8.3 小节总结

* 学习了Transformer架构采用Multi-head Attention的原因.
	* 将模型划分为多个头, 分别进行Attention计算, 可以形成多个子空间, 让模型去关注不同方面的信息特征, 更好的提升模型的效果.
	* 多头注意力有助于神经网络捕捉到更丰富的特征信息.

* 学习了Multi-head Attention的计算方式.
	* 对特征张量的最后一个维度进行了分割, 一般是对词嵌入的维度embedding_dim进行切割, 切割后的计算规则和单一head完全一致.
	* 在不同的head上应用了注意力计算规则后, 得到的结果张量直接采用拼接concat的方式进行融合, 就得到了Multi-head Attention的结果张量.

---

---

---


## 9.Transformer相比于RNN/LSTM有什么优势? 为什么?

---

### 学习目标

* 掌握Transformer相比于RNN/LSTM的优势和背后的原因.

---

### 9.1 Transformer的并行计算

* 对于Transformer比传统序列模型RNN/LSTM具备优势的第一大原因就是强大的并行计算能力.
	* 对于RNN来说, 任意时刻t的输入是时刻t的输入x(t)和上一时刻的隐藏层输出h(t-1), 经过运算后得到当前时刻隐藏层的输出h(t), 这个h(t)也即将作为下一时刻t+1的输入的一部分. 这个计算过程是RNN的本质特征, RNN的历史信息是需要通过这个时间步一步一步向后传递的. 而这就意味着RNN序列后面的信息只能等到前面的计算结束后, 将历史信息通过hidden state传递给后面才能开始计算, 形成链式的序列依赖关系, 无法实现并行.
	* 对于Transformer结构来说, 在self-attention层, 无论序列的长度是多少, 都可以一次性计算所有单词之间的注意力关系, 这个attention的计算是同步的, 可以实现并行.

---

### 9.2 Transformer的特征抽取能力

* 对于Transformer比传统序列模型RNN/LSTM具备优势的第二大原因就是强大的特征抽取能力.
	* Transformer因为采用了Multi-head Attention结构和计算机制, 拥有比RNN/LSTM更强大的特征抽取能力, 这里并不仅仅由理论分析得来, 而是大量的试验数据和对比结果, 清楚的展示了Transformer的特征抽取能力远远胜于RNN/LSTM.
	* 注意: 不是越先进的模型就越无敌, 在很多具体的应用中RNN/LSTM依然大有用武之地, 要具体问题具体分析.

---

### 9.3 小节总结

* 学习了Transformer相比于RNN/LSTM的优势和原因.
	* 1: 第一大优势是并行计算的优势.
	* 2: 第二大优势是特征提取能力强.

---

---

---


## 10.为什么说Transformer可以代替seq2seq?

---

### 学习目标

* 掌握Transformer可以替代seq2seq的核心原因.

---

### 10.1 seq2seq的两大缺陷

* 1: seq2seq架构的第一大缺陷是将Encoder端的所有信息压缩成一个固定长度的语义向量中, 用这个固定的向量来代表编码器端的全部信息. 这样既会造成信息的损耗, 也无法让Decoder端在解码的时候去用注意力聚焦哪些是更重要的信息.

* 2: seq2seq架构的第二大缺陷是无法并行, 本质上和RNN/LSTM无法并行的原因一样.

---

### 10.2 Transformer的改进

* Transformer架构同时解决了seq2seq的两大缺陷, 既可以并行计算, 又应用Multi-head Attention机制来解决Encoder固定编码的问题, 让Decoder在解码的每一步可以通过注意力去关注编码器输出中最重要的那些部分.

---

### 10.3 小节总结

* 学习了seq2seq架构的两大缺陷.
	* 第一个缺陷是Encoder端的所有信息被压缩成一个固定的输出张量, 当序列长度较长时会造成比较严重的信息损耗.
	* 第二个缺陷是无法并行计算.

* 学习了Transformer架构对seq2seq两大缺陷的改进.
	* Transformer应用Multi-head Attention机制让编码器信息可以更好的展示给解码器.
	* Transformer可以实现Encoder端的并行计算. 

---

---

---


## 11.self-attention公式中的归一化有什么作用? 为什么要添加scaled?

---

### 学习目标

* 理解softmax函数的输入是如何影响输出分布的.
* 理解softmax函数反向传播进行梯度求导的数学过程.
* 理解softmax函数出现梯度消失的原因.
* 理解self-attention计算规则中归一化的原因.

---

### 11.1 self-attention中的归一化概述

* 训练上的意义: 随着词嵌入维度d_k的增大, q * k 点积后的结果也会增大, 在训练时会将softmax函数推入梯度非常小的区域, 可能出现梯度消失的现象, 造成模型收敛困难.


* 数学上的意义: 假设q和k的统计变量是满足标准正态分布的独立随机变量, 意味着q和k满足均值为0, 方差为1. 那么q和k的点积结果就是均值为0, 方差为d_k, 为了抵消这种方差被放大d_k倍的影响, 在计算中主动将点积缩放1/sqrt(d_k), 这样点积后的结果依然满足均值为0, 方差为1.

---

### 11.2 softmax的梯度变化

* 这里我们分3个步骤来解释softmax的梯度问题:
	* 第一步: softmax函数的输入分布是如何影响输出的.
	* 第二步: softmax函数在反向传播的过程中是如何梯度求导的.
	* 第三步: softmax函数出现梯度消失现象的原因.

---

* 第一步: softmax函数的输入分布是如何影响输出的.
	* 对于一个输入向量x, softmax函数将其做了一个归一化的映射, 首先通过自然底数e将输入元素之间的差距先"拉大", 然后再归一化为一个新的分布. 在这个过程中假设某个输入x中最大的元素下标是k, 如果输入的数量级变大(就是x中的每个分量绝对值都很大), 那么在数学上会造成y_k的值非常接近1.
	* 具体用一个例子来演示, 假设输入的向量x = [a, a, 2a], 那么随便给几个不同数量级的值来看看对y3产生的影响

```text
a = 1时,   y3 = 0.5761168847658291
a = 10时,  y3 = 0.9999092083843412
a = 100时, y3 = 1.0
```

---

> * 采用一段实例代码将a在不同取值下, 对应的y3全部画出来, 以曲线的形式展示:

```python
from math import exp
from matplotlib import pyplot as plt
import numpy as np 
f = lambda x: exp(x * 2) / (exp(x) + exp(x) + exp(x * 2))
x = np.linspace(0, 100, 100)
y_3 = [f(x_i) for x_i in x]
plt.plot(x, y_3)
plt.show()
```

---

> * 得到如下的曲线:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5ves36j210m0r0t9e.jpg" height="auto" width="auto"/>

---

> * 从上图可以很清楚的看到输入元素的数量级对softmax最终的分布影响非常之大.

> * 结论: 在输入元素的数量级较大时, softmax函数几乎将全部的概率分布都分配给了最大值分量所对应的标签.


---

* 第二步: softmax函数在反向传播的过程中是如何梯度求导的.

> * 首先定义神经网络的输入和输出:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5nkq45j20ra06waaa.jpg" height="auto" width="auto"/>

---

> * 反向传播就是输出端的损失函数对输入端求偏导的过程, 这里要分两种情况, 第一种如下所示:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5t5q0aj20s00loq3z.jpg" height="auto" width="auto"/>

---

> * 第二种如下所示:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5tsgzvj20r60hkdgk.jpg" height="auto" width="auto"/>

---

> * 经过对两种情况分别的求导计算, 可以得出最终的结论如下:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5z44qdj20qk07kaag.jpg" height="auto" width="auto"/>

---


* 第三步: softmax函数出现梯度消失现象的原因.

> * 根据第二步中softmax函数的求导结果, 可以将最终的结果以矩阵形式展开如下:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5pcdc5j20qm07oaae.jpg" height="auto" width="auto"/>

---

> * 根据第一步中的讨论结果, 当输入x的分量值较大时, softmax函数会将大部分概率分配给最大的元素, 假设最大元素是x1, 那么softmax的输出分布将产生一个接近one-hot的结果张量y_ = [1, 0, 0,..., 0], 此时结果矩阵变为:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5qpyycj20m6076aa7.jpg" height="auto" width="auto"/>

---

> * 结论: 综上可以得出, 所有的梯度都消失为0(接近于0), 参数几乎无法更新, 模型收敛困难.

---

---


### 11.3 维度与点积大小的关系

* 针对为什么维度会影响点积的大小, 原始论文中有这样的一点解释如下:

```text
To illustrate why the dot products get large, assume that the components of q and k 
are independent random variables with mean 0 and variance 1. Then their doct product,
q*k = (q1k1+q2k2+......+q(d_k)k(d_k)), has mean 0 and variance d_k.
```

---

> * 我们分两步对其进行一个推导, 首先就是假设向量q和k的各个分量是相互独立的随机变量, X = q_i, Y = k_i, X和Y各自有d_k个分量, 也就是向量的维度等于d_k, 有E(X) = E(Y) = 0, 以及D(X) = D(Y) = 1.

> * 可以得到E(XY) = E(X)E(Y) = 0 * 0 = 0

> * 同理, 对于D(XY)推导如下:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5s7qcdj20po0a2wfa.jpg" height="auto" width="auto"/>

---

> * 根据期望和方差的性质, 对于互相独立的变量满足下式:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5o0fkjj20kq094t8s.jpg" height="auto" width="auto"/>

---

> * 根据上面的公式, 可以很轻松的得出q*k的均值为E(qk) = 0, D(qk) = d_k.

> * 所以方差越大, 对应的qk的点积就越大, 这样softmax的输出分布就会更偏向最大值所在的分量.

> * 一个技巧就是将点积除以sqrt(d_k), 将方差在数学上重新"拉回1", 如下所示:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5rc8xpj20i8042a9y.jpg" height="auto" width="auto"/>

---

> * 最终的结论: 通过数学上的技巧将方差控制在1, 也就有效的控制了点积结果的发散, 也就控制了对应的梯度消失的问题!

---

### 11.4 小节总结

* 1: 学习了softmax函数的输入是如何影响输出分布的.
	* softmax函数本质是对输入的数据分布做一次归一化处理, 但是输入元素的数量级对softmax最终的分布影响非常之大.
	* 在输入元素的数量级较大时, softmax函数几乎将全部的概率分布都分配给了最大值分量所对应的标签.

* 2: 学习了softmax函数在反向传播的过程中是如何梯度求导的.
	* 具体的推导过程见讲义正文部分, 注意要分两种情况讨论, 分别处理.

* 3: 学习了softmax函数出现梯度消失现象的原因.
	* 结合第一步, 第二步的结论, 可以很清楚的看到最终的梯度矩阵接近于零矩阵, 这样在进行参数更新的时候就会产生梯度消失现象.

* 4: 学习了维度和点积大小的关系推导.
	* 通过期望和方差的推导理解了为什么点积会造成方差变大.
	* 理解了通过数学技巧除以sqrt(d_k)就可以让方差恢复成1.

---

---

---


## 12.Transformer架构的并行化是如何进行的? 具体体现在哪里?

---

### 学习目标

* 掌握Transformer架构的并行化是如何进行的.
* 理解为什么采用这样的方式可以实现Transformer的并行化.

---

### 12.1 Transformer架构中Encoder的并行化

* 首先Transformer的并行化主要体现在Encoder模块上.


<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5wstu5j20z80l2jsa.jpg" height="auto" width="auto"/>

---

> * 1: 上图最底层绿色的部分, 整个序列所有的token可以并行的进行Embedding操作, 这一层的处理是没有依赖关系的.

> * 2: 上图第二层土黄色的部分, 也就是Transformer中最重要的self-attention部分, 这里对于任意一个单词比如x1, 要计算x1对于其他所有token的注意力分布, 得到z1. 这个过程是具有依赖性的, 必须等到序列中所有的单词完成Embedding才可以进行. 因此这一步是不能并行处理的. 但是从另一个角度看, 我们真实计算注意力分布的时候, 采用的都是矩阵运算, 也就是可以一次性的计算出所有token的注意力张量, 从这个角度看也算是实现了并行, 只是矩阵运算的"并行"和词嵌入的"并行"概念上不同而已.

> * 3: 上图第三层蓝色的部分, 也就是前馈全连接层, 对于不同的向量z之间也是没有依赖关系的, 所以这一层是可以实现并行化处理的. 也就是所有的向量z输入Feed Forward网络的计算可以同步进行, 互不干扰.

---

### 12.2 Transformer架构中Decoder的并行化

* 其次Transformer的并行化也部分的体现在Decoder模块上.

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu5y8jn4j213q0nmwi6.jpg" height="auto" width="auto"/>

---

> * 1: Decoder模块在训练阶段采用了并行化处理. 其中Self-Attention和Encoder-Decoder Attention两个子层的并行化也是在进行矩阵乘法, 和Encoder的理解是一致的. 在进行Embedding和Feed Forward的处理时, 因为各个token之间没有依赖关系, 所以也是可以完全并行化处理的, 这里和Encoder的理解也是一致的.

> * 2: Decoder模块在预测阶段基本上不认为采用了并行化处理. 因为第一个time step的输入只是一个"SOS", 后续每一个time step的输入也只是依次添加之前所有的预测token. 

> * 3: 注意: 最重要的区别是训练阶段目标文本如果有20个token, 在训练过程中是一次性的输入给Decoder端, 可以做到一些子层的并行化处理. 但是在预测阶段, 如果预测的结果语句总共有20个token, 则需要重复处理20次循环的过程, 每次的输入添加进去一个token, 每次的输入序列比上一次多一个token, 所以不认为是并行处理.

---

### 12.3 小节总结

* 学习了Transformer架构中Encoder模块的并行化机制.
	* Encoder模块在训练阶段和测试阶段都可以实现完全相同的并行化.
	* Encoder模块在Embedding层, Feed Forward层, Add & Norm层都是可以并行化的.
	* Encoder模块在self-attention层, 因为各个token之间存在依赖关系, 无法独立计算, 不是真正意义上的并行化.
	* Encoder模块在self-attention层, 因为采用了矩阵运算的实现方式, 可以一次性的完成所有注意力张量的计算, 也是另一种"并行化"的体现.

* 学习了Transformer架构中Decoder模块的并行化机制.
	* Decoder模块在训练阶段可以实现并行化.
	* Decoder模块在训练阶段的Embedding层, Feed Forward层, Add & Norm层都是可以并行化的.
	* Decoder模块在self-attention层, 以及Encoder-Decoder Attention层, 因为各个token之间存在依赖关系, 无法独立计算, 不是真正意义上的并行化.
	* Decoder模块在self-attention层, 以及Encoder-Decoder Attention层, 因为采用了矩阵运算的实现方式, 可以一次性的完成所有注意力张量的计算, 也是另一种"并行化"的体现.
	* Decoder模块在预测计算不能并行化处理.

---

