## 1.认识GPT

---

### 学习目标

* 了解什么是GPT.
* 掌握GPT的架构.
* 掌握GPT的预训练任务.

---

### 1.1 什么是GPT

* GPT是OpenAI公司提出的一种语言预训练模型.
    * OpenAI在论文[<< Improving Language Understanding by Generative Pre-Training >>](https://s3-us-west-2.amazonaws.com/openai-assets/research-covers/language-unsupervised/language_understanding_paper.pdf)中提出GPT模型.
    * OpenAI后续又在论文[<< Language Models are Unsupervised Multitask Learners >>](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf)中提出GPT2模型.
    * GPT和GPT2模型结构差别不大, 但是GPT2采用了更大的数据集进行训练.

* OpenAI GPT模型是在Google BERT模型之前提出的, **与BERT最大的区别在于GPT采用了传统的语言模型方法进行预训练, 即使用单词的上文来预测单词, 而BERT是采用了双向上下文的信息共同来预测单词.**
    * 正是因为训练方法上的区别, **使得GPT更擅长处理自然语言生成任务(NLG), 而BERT更擅长处理自然语言理解任务(NLU).**

---

### 1.2 GPT的架构

* 再次看三个语言模型的对比架构图, 中间的就是GPT:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cumejzc8j21660gyq71.jpg" height="auto" width="auto"/>

---

> * 从上图可以很清楚的看到GPT采用的是单向Transformer模型, 例如给定一个句子[u1, u2, ..., un], GPT在预测单词ui的时候只会利用[u1, u2, ..., u(i-1)]的信息, 而BERT会同时利用上下文的信息[u1, u2, ..., u(i-1), u(i+1), ..., un].

---

> * 作为两大模型的直接对比, BERT采用了Transformer的Encoder模块, 而GPT采用了Transformer的Decoder模块. 并且GPT的Decoder Block和经典Transformer Decoder Block还有所不同, 如下图所示:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cumbp7uej20lc0uidhk.jpg" height="auto" width="auto"/>

---

> * 如上图所示, 经典的Transformer Decoder Block包含3个子层, 分别是Masked Multi-Head Attention层, encoder-decoder attention层, 以及Feed Forward层. 但是在GPT中取消了第二个encoder-decoder attention子层, 只保留Masked Multi-Head Attention层, 和Feed Forward层.

---

> * 作为单向Transformer Decoder模型, GPT利用句子序列信息预测下一个单词的时候, 要使用Masked Multi-Head Attention对单词的下文进行遮掩, 来防止未来信息的提前泄露. 例如给定一个句子包含4个单词[A, B, C, D], GPT需要用[A]预测B, 用[A, B]预测C, 用[A, B, C]预测D. 很显然的就是当要预测B时, 需要将[B, C, D]遮掩起来.

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cum7gk8nj20zq0o8act.jpg" height="auto" width="auto"/>

---

> * 具体的遮掩操作是在slef-attention进行softmax之前进行的, 一般的实现是将MASK的位置用一个无穷小的数值-inf来替换, 替换后执行softmax计算得到新的结果矩阵. 这样-inf的位置就变成了0. 如上图所示, 最后的矩阵可以很方便的做到当利用A预测B的时候, 只能看到A的信息; 当利用[A, B]预测C的时候, 只能看到A, B的信息.

---

> * 注意: 对比于经典的Transformer架构, 解码器模块采用了6个Decoder Block; GPT的架构中采用了12个Decoder Block.

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cumfffjtj20ik0ocdgq.jpg" height="auto" width="auto"/>

---

### 1.3 GPT训练过程

* GPT的训练也是典型的两阶段过程:
    * 第一阶段: 无监督的预训练语言模型.
    * 第二阶段: 有监督的下游任务fine-tunning.

---

* 第一阶段: 无监督的预训练语言模型.

> * 给定句子U = [u1, u2, ..., un], GPT训练语言模型时的目标是最大化下面的似然函数:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cum75t7qj20za07k3yp.jpg" height="auto" width="auto"/>

---

> * 有上述公式可知, GPT是一个单向语言模型, 假设输入张量用h0表示, 则计算公式如下:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cumav04rj20ss068mx4.jpg" height="auto" width="auto"/>

---

> * 其中Wp是单词的位置编码, We是单词本身的word embedding. Wp的形状是[max_seq_len, embedding_dim], We的形状是[vocab_size, embedding_dim].

---

> * 得到输入张量h0后, 要将h0传入GPT的Decoder Block中, 依次得到ht:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu6ya19ij20zk05it8v.jpg" height="auto" width="auto"/>

---

> * 最后通过得到的ht来预测下一个单词:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu6xh34dj20tm05iq33.jpg" height="auto" width="auto"/>

---

* 第二阶段: 有监督的下游任务fine-tunning.

> * GPT经过预训练后, 会针对具体的下游任务对模型进行微调. 微调采用的是有监督学习, 训练样本包括单词序列[x1, x2, ..., xn]和label y. GPT微调的目标任务是根据单词序列[x1, x2, ..., xn]预测标签y.

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cumdjsh5j20ys05qq34.jpg" height="auto" width="auto"/>

---

> * 其中Wy表示预测输出的矩阵参数, 微调任务的目标是最大化下面的函数:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cumba33tj20wq07ygls.jpg" height="auto" width="auto"/>

---

> * 综合两个阶段的目标任务函数, 可知GPT的最终优化函数为:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cum8ednbj20lk05edfq.jpg" height="auto" width="auto"/>

---


### 1.4 小节总结

* 学习了什么是GPT.
    * GPT是OpenAI公司提出的一种预训练语言模型.
    * 本质上来说, GPT是一个单向语言模型.

* 学习了GPT的架构.
    * GPT采用了Transformer架构中的解码器模块.
    * GPT在使用解码器模块时做了一定的改造, 将传统的3层Decoder Block变成了2层Block, 删除了encoder-decoder attention子层, 只保留Masked Multi-Head Attention子层和Feed Forward子层.
    * GPT的解码器总共是由12个改造后的Decoder Block组成的.

* 学习了GPT的预训练任务.
    * 第一阶段: 无监督的预训练语言模型. 只利用单词前面的信息来预测当前单词.
    * 第二阶段: 有监督的下游任务fine-tunning.

---

---

---


## 2.认识GPT2

---

### 学习目标

* 掌握GPT2的架构
* 掌握GPT2的训练任务和模型细节

---

### 2.1 GPT2的架构

* 从模型架构上看, GPT2并没有特别新颖的架构, 它和只带有解码器模块的Transformer很像.

> * 所谓语言模型, 作用就是根据已有句子的一部分, 来预测下一个单词会是什么. 现实应用中大家最熟悉的一个语言模型应用, 就是智能手机上的输入法, 它可以根据当前输入的内容智能推荐下一个要打的字.

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cumgeenqj20pa0gumxv.jpg" height="auto" width="auto"/> 

---

> * GPT2也是一个语言预测生成模型, 只不过比手机上应用的模型要大很多, 也更加复杂. 常见的手机端应用的输入法模型基本占用50MB空间, 而OpenAI的研究人员使用了40GB的超大数据集来训练GPT2, 训练后的GPT2模型最小的版本也要占用超过500MB空间来存储所有的参数, 至于最大版本的GPT2则需要超过6.5GB的存储空间.

> * 自从Transformer问世以来, 很多预训练语言模型的工作都在尝试将编码器或解码器堆叠的尽可能高, 那类似的模型可以堆叠到多深呢? 事实上, 这个问题的答案也就是区别不同GPT2版本的主要因素之一. 比如最小版本的GPT2堆叠了12层, 中号的24层, 大号的36层, 超大号的堆叠了整整48层!

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu71kag2j21120hcjtb.jpg" height="auto" width="auto"/>

---

### 2.2 GPT2模型的细节

* 以机器人第一法则为例, 来具体看GPT2的工作细节.
    * 机器人第一法则: 机器人不得伤害人类, 或者目睹人类将遭受危险而袖手旁观.

---

* 首先明确一点: GPT2的工作流程很像传统语言模型, 一次只输出一个单词(token).

> * GPT2之所以在生成式任务中表现优秀, 是因为在每个新单词(token)产生后, 该单词就被添加在之前生成的单词序列后面, 添加后的新序列又会成为模型下一步的新输入. 这种机制就叫做自回归(auto-regression), 如下所示:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cum9t7vfj21dq0gkmy4.jpg" height="auto" width="auto"/>

---

* 其次明确一点: GPT2模型是一个只包含了Transformer Decoder模块的模型.

> * 和BERT模型相比, GPT2的解码器在self-attention层上有一个关键的差异: 它将后面的单词(token)遮掩掉, 而BERT是按照一定规则将单词替换成[MASK].

> * 举个例子, 如果我们重点关注4号位置的单词及其前序路径, 我们可以让模型只允许注意当前计算的单词和它之前的单词, 如下图所示:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu778jruj210w0jg75v.jpg" height="auto" width="auto"/>

---

> * 注意: 能够清楚的区分BERT使用的自注意力模块(self-attention)和GPT2使用的带掩码的自注意力模块(masked self-attention)很重要! 普通的self-attention允许模型的任意一个位置看到它右侧的信息(下图左侧), 而带掩码的self-attention则不允许这么做(下图右侧).

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cumfvz1tj20z40dqaao.jpg" height="auto" width="auto"/>

---

> * 在Transformer原始论文发表后, 一篇名为<< Generating Wikipedia by Summarizing Long Sequences >>的论文提出用另一种Transformer模块的排列方式来进行语言建模-它直接扔掉了编码器, 只保留解码器. 这个早期的基于Transformer的模型由6个Decoder Block堆叠而成:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cumexy4pj20zc0hkq4c.jpg" height="auto" width="auto"/>

---

> * 上图中所有的解码器模块都是一样的, 因为只展开了第一个解码器的内部结构. 和GPT一样, 只保留了带掩码的self-attention子层, 和Feed Forward子层.

> * 这些解码器和经典Transformer原始论文中的解码器模块相比, 除了删除了第二个Encoder-Decoder Attention子层外, 其他构造都一样.

---

* GPT2工作细节探究.
    * GPT2可以处理最长1024个单词的序列.
    * 每个单词都会和它的前序路径一起"流经"所有的解码器模块.

---

> * 对于生成式模型来说, 基本工作方式都是提供一个预先定义好的起始token, 比如记做"s".

> * 此时模型的输入只有一个单词, 所以只有这个单词的路径是活跃的. 单词经过层层处理, 最终得到一个词向量. 该向量可以对于词汇表的每个单词计算出一个概率(GPT2的词汇表中有50000个单词). 在本例中, 我们选择概率最高的单词["The"]作为下一个单词.

> * 注意: 这种选择最高概率输出的策略有时会出现问题-如果我们持续点击输入法推荐单词的第一个, 它可能会陷入推荐同一个词的循环中, 只有你点击第二个或第三个推荐词, 才能跳出这种循环. 同理, GPT2有一个top-k参数, 模型会从概率最大的前k个单词中抽样选取下一个单词.

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cumgtawbj21bg0n20ud.jpg" height="auto" width="auto"/>

---

> * 接下来, 我们将输出的单词["The"]添加在输入序列的尾部, 从而构建出新的输入序列["s", "The"], 让模型进行下一步的预测:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cum9d4saj21be0mstae.jpg" height="auto" width="auto"/>

---

> * 此时第二个单词的路径是当前唯一活跃的路径了. GPT2的每一层都保留了它们对第一个单词的解释, 并且将运用这些信息处理第二个单词, GPT2不会根据第二个单词重新来解释第一个单词.

---

* 关于输入编码: 当我们更加深入的了解模型的内部细节时, 最开始就要面对模型的输入, 和其他自然语言模型一样, GPT2同样从嵌入矩阵中查找单词对应的嵌入向量, 该矩阵(embedding matrix)也是整个模型训练结果的一部分.

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu704pg0j20yo0jyq40.jpg" height="auto" width="auto"/>

---

> * 1: 如上图所示, 每一行都是一个词嵌入向量: 一个能够表征某个单词, 并捕获其语义的数字向量. 嵌入的维度大小和GPT2模型的大小相关, 最小的模型采用了768这个维度, 最大的采用了1600这个维度.

---

> * 2: 所以在整个模型运作起来的最开始, 我们需要在嵌入矩阵中查找起始单词"s"对应的嵌入向量. 但在将其输入给模型之前, 还需要引入位置编码(positional encoding), 1024分输入序列位置中的每一个都对应了一个位置编码, 同理于词嵌入矩阵, 这些位置编码组成的矩阵也是整个模型训练结果的一部分.

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cumd83hzj20xm0jsgmm.jpg" height="auto" width="auto"/>

---

> * 经历前面的1, 2两步, 输入单词在进入模型第一个transformer模块前的所有处理步骤就结束了. 综上所述, GPT2模型包含两个权值矩阵: 词嵌入矩阵和位置编码矩阵. 而输入到transformer模块中的张量就是这两个矩阵对应的加和结果.

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cum7x7lij21fc0o0gnw.jpg" height="auto" width="auto"/>

---

* transformer模块的堆叠:
    * 最底层的transformer模块处理单词的步骤:
        * 首先通过自注意力层处理, 接着将其传递给前馈全连接层, 这其中包含残差连接和Layer Norm等子层操作.
        * 最底层的transformer模块处理结束后, 会将结果张量传递给第二层的transformer模块, 继续进行计算.
        * 每一个transformer模块的处理方式都是一样的, 不断的重复相同的模式, 但是每个模块都会维护自己的self-attention层和Feed Forward层的权重值.

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cumaa389j21d60o0wh1.jpg" height="auto" width="auto"/>

---

* GPT2的自注意力机制回顾
    * 自然语言的含义是极度依赖上下文的, 比如下面所展示的"机器人第二法则":

> * 机器人必须遵守人类给它的命令, 除非该命令违背了第一法则.

---

> * 在上述语句中, 有三处单词具有指代含义, 除非我们知道这些词所精确指代的上下文, 否则根本不可能理解这句话的真实语义. 

> * 当模型处理这句话的时候, 模型必须知道以下三点:

> * [它]指代机器人.

> * [命令]指代前半句话中人类给机器人下达的命令, 即[人类给它的命令].

> * [第一法则]指代机器人第一法则的完整内容.

---

> * 这就是自注意力机制所做的工作, 它在处理每个单词之前, 融入了模型对于用来解释某个单词的上下文的相关单词的理解. 具体的做法是: 给序列中的每一个单词都赋予一个相关度得分, 本质上就是注意力权重.

---

> * 看下图, 举个例子, 最上层的transformer模块在处理单词"it"的时候会关注"a robot", 所以"a", "robot", "it", 这三个单词与其得分相乘加权求和后的特征向量会被送入之后的Feed Forward层.

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cum90er9j20z20ka760.jpg" height="auto" width="auto"/>

---

> * 自注意力机制沿着序列的每一个单词的路径进行处理, 主要由3个向量组成:

> * 1: Query(查询向量), 当前单词的查询向量被用来和其它单词的键向量相乘, 从而得到其它词相对于当前词的注意力得分.

> * 2: Key(键向量), 键向量就像是序列中每个单词的标签, 它使我们搜索相关单词时用来匹配的对象.

> * 3: Value(值向量), 值向量是单词真正的表征, 当我们算出注意力得分后, 使用值向量进行加权求和得到能代表当前位置上下文的向量.

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cumhaqylj21060km3zj.jpg" height="auto" width="auto"/>

---

> * 如上图所示, 一个简单的比喻是在档案柜中找文件. 查询向量Query就像一张便利贴, 上面写着你正在研究的课题. 键向量Key像是档案柜中文件夹上贴的标签. 当你找到和便利贴上所写相匹配的文件夹时, 拿出对应的文件夹, 文件夹里的东西便是值向量Value.

> * 将单词的查询向量Query分别乘以每个文件夹的键向量Key，得到各个文件夹对应的注意力得分Score.

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cume13u7j210k0jw758.jpg" height="auto" width="auto"/>

---

> * 我们将每个文件夹的值向量Value乘以其对应的注意力得分Score, 然后求和, 得到最终自注意力层的输出, 如下图所示:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cu75ax9jj20zu0l840b.jpg" height="auto" width="auto"/>

---

> * 这样将值向量加权混合得到的结果也是一个向量, 它将其50%的注意力放在了单词"robot"上, 30%的注意力放在了"a"上, 还有19%的注意力放在了"it"上.

---

* 模型的输出:

> * 当最后一个transformer模块产生输出之后, 模型会将输出张量乘上 词嵌入矩阵:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cumc5jkoj20z60j2aau.jpg" height="auto" width="auto"/>

---

> * 我们知道, 词嵌入矩阵的每一行都对应模型的词汇表中一个单词的嵌入向量. 所以这个乘法操作得到的结果就是词汇表中每个单词对应的注意力得分, 如下图所示:

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cumhri11j20zu0fydgt.jpg" height="auto" width="auto"/>

---

> * 一般来说, 我们都采用贪心算法, 选取得分最高的单词作为输出结果(top_k = 1).

> * 但是一个更好的策略是对于词汇表中得分较高的一部分单词, 将它们的得分作为概率从整个单词列表中进行抽样(得分越高的单词越容易被选中).

> * 通常会用一个折中的方法, 即选取top_k = 40, 这样模型会考虑注意力得分排名前40的单词.

<img src="https://tva1.sinaimg.cn/large/e6c9d24ely1h2cumcmbzij20z60je0tz.jpg" height="auto" width="auto"/>

---

> * 如上图所示, 模型就完成了一个时间步的迭代, 输出了一个单词. 接下来模型会不断的迭代, 直至生成完整的序列(序列长度达到1024的上限, 或者序列的某一个时间步生成了结束符).

---

### 2.3 小节总结

* 学习了GPT2的架构:
    * GPT2只采用了Transformer架构中的Decoder模块.
    * GPT2是在GPT基础上发展出的更强大的语言预训练模型.

* 学习了GPT2的工作细节:
    * GPT2可以处理最长1024个单词的序列.
    * 每个单词都会和它的前序路径一起"流经"所有的解码器模块.
    * GPT2本质上也是自回归模型.
    * 输入张量要经历词嵌入矩阵和位置编码矩阵的加和后, 才能输入进transformer模块中.

* 学习了GPT2自注意力机制的细节:
    * 首先, GPT2的自注意力是Masked self-attention, 只能看见左侧的序列, 不能看见右侧的信息.
    * Query, Key, Value这三个张量之间的形象化的例子, 生动的说明了各自的作用和运算方式.
    * 最后的输出可以采用多个方法, 贪心方案, 概率分布方案, 或者top-k方案等.


---

---

---


## 3.请详述BERT, GPT, ELMo模型的对比和各自的优缺点?

---

### 学习目标

* 理解BERT, GPT, ELMo相互间的不同点.
* 理解BERT, GPT, ELMo相互比较下的各自优点和缺点.

---

### 3.1 BERT, GPT, ELMo之间的不同点

* 关于特征提取器:
    * ELMo采用两部分双层双向LSTM进行特征提取, 然后再进行特征拼接来融合语义信息.
    * GPT和BERT采用Transformer进行特征提取.
    * 很多NLP任务表明Transformer的特征提取能力强于LSTM, 对于ELMo而言, 采用1层静态token embedding + 2层LSTM, 提取特征的能力有限.

* 单/双向语言模型:
    * 三者之中, 只有GPT采用单向语言模型, 而ELMo和BERT都采用双向语言模型.
    * ELMo虽然被认为采用了双向语言模型, 但实际上是左右两个单向语言模型分别提取特征, 然后进行特征拼接, 这种融合特征的能力比BERT一体化的融合特征方式弱.
    * 三者之中, 只有ELMo没有采用Transformer. GPT和BERT都源于Transformer架构, GPT的单向语言模型采用了经过修改后的Decoder模块, Decoder采用了look-ahead mask, 只能看到context before上文信息, 未来的信息都被mask掉了. 而BERT的双向语言模型采用了Encoder模块, Encoder只采用了padding mask, 可以同时看到context before上文信息, 以及context after下文信息.


---

### 3.2 BERT, GPT, ELMo各自的优点和缺点

* ELMo:
    * 优点:
        * 从早期的Word2Vec预训练模型的最大缺点出发, 进行改进, 这一缺点就是无法解决多义词的问题.
        * ELMo根据上下文动态调整word embedding, 可以解决多义词的问题.
    * 缺点:
        * ELMo使用LSTM提取特征的能力弱于Transformer.
        * ELMo使用向量拼接的方式融合上下文特征的能力弱于Transformer.

* GPT:
    * 优点:
        * GPT使用了Transformer提取特征, 使得模型能力大幅提升.
    * 缺点:
        * GPT只使用了单向Decoder, 无法融合未来的信息.

* BERT:
    * 优点:
        * BERT使用了双向Transformer提取特征, 使得模型能力大幅提升.
        * 添加了两个预训练任务, MLM + NSP的多任务方式进行模型预训练.
    * 缺点:
        * 模型过于庞大, 参数量太多, 需要的数据和算力要求过高, 训练好的模型应用场景要求高.
        * 更适合用于语言嵌入表达, 语言理解方面的任务, 不适合用于生成式的任务.


---

### 3.3 小节总结

* 学习了BERT, GPT, ELMo之间的区别:s
    * **三者所选取的特征提取器不同.**
        * BERT采用的是Transformer架构中的Encoder模块.
        * GPT采用的是Transformer架构中的Decoder模块.
        * ELMo采用的双层双向LSTM模块.

    * **三者所采用的语言模型单/双向不同.**
        * BERT采用的是最彻底的双向语言模型, 可以同时关注context before和context after.
        * GPT采用的是单向语言模型, 即Transformer中的Decoder, 由于采用了mask机制, 所以未来信息context after都不可见.
        * ELMo表面上被认为是双向语言模型, 但实际上是左右两个单向LSTM模型分别提取特征, 在进行简单的拼接融合.

