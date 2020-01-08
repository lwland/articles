### word2vec
1. google2013年发布的工具，可以较好的表达词之间的关系
2. 模型
   1. Skip-gram 跳字模型
   2. CBOW 连续词袋模型
3. 训练方法
   1. 负采样
   2. 层次softmax
4. 词嵌入（word embedding) 将词语转换到数学空间中
5. 统计语言模型模型：对文本S=w1w2w3...wT 在一种语言空间里出现的概率是P(S)=P(w1w2w3...wT)
   1. 联合概率转换成一系列条件概率的乘积则 P(S)=P(w1)P(w2|w1)P(w3|w1w2).....P(wT|w1w2...wT-1)
   2. 马尔科夫假设 下一个词的出现仅依赖于它前面的一个或多个词
      1. bigram 下一个词的出现仅依赖于它前面的一个词 P(S)=P(w1)P(w2|w1)P(w3|w2).....P(wT|wT-1)
      2. trigram 下一个词的出现依赖于他前面的两个词 P(S)=P(w1)P(w2|w1)P(w3|w1w2)P(w4|w2w3).....P(wt|wT-1wT-2);
      3. Ngram 下一个词的出现依赖于他前面的N个词
6. NLP词的表示方法
   1. one-hot 词库很大的时候维度灾难 词语之间没有关联
   2. 词的分布式表示 distributed representation 通过训练讲每个词的纬度降低到一个较短的词向量上
      1. 基于矩阵的分布式表示
      2. 基于聚类的分布式表示
      3. 基于神经网络的分布式表示
         1. Nerual Network Language model
         2. Log-bilinear Language model
         3. recurrent Nerual Network Language model
         4. C&W
         5. CBOW skip-gram