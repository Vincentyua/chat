# chat
**这是一个基于seq2seq模型的聊天系统，使用LSTM/GRU+注意力机制。使用开源框架pytorch。**
项目过程中的学习记录见[record.pdf](https://github.com/duyeouc/chat/blob/master/Record.pdf)。

## 环境
- python 3.6
- pytorch 0.4
- 其他python库


## 语料
本项目语料使用了20万句电影对话语料，去除低频词汇、过长句子后剩余约14万句，构建词典大小13000词。
训练此模型，首先在```main_train.py```中指定你的语料路径。并且，语料格式须满足：每个句子占一行，每两行为一个对话语句对。如：
```
Nice to meet you.
Nice to meet you, too.

I am sorry.
You are welcome.
```
一些公开语料：[[语料]](https://github.com/rkadlec/ubuntu-ranking-dataset-creator)

## 预处理
- 删除过长句子
- 删除包括低频词汇的句子
- 句子小写，标点分离
- 生成字典，训练语句对

## 模型
基于sequence to sequence模型，项目分别实现了LSTM和GRU的模型构建，并实现了注意力机制。通过```main_train.py```指定相关参数选择使用LSTM或GRU，以及是否使用注意力机制。
本项目中训练的模型结构如下所示：
![model](https://github.com/duyeouc/chat/blob/master/img/model.svg)
- Encoder采用4层双向LSTM，Decoder采用4层LSTM
- 采用注意力机制
- 隐藏层维度为512
- batch_size = 128
- 学习率初始为0.001，训练过程中衰减至0.0001
- 采用Adam梯度下降
- 损失函数采用交叉熵损失(cross-entropy loss)

## 训练
执行```python main_train.py```以训练模型。相关参数解释如下：
- corpus：语料路径
- batch_size
- n_iteration：迭代次数
- learning_rate：学习率
- n_layers：层数
- hidden_size:隐藏层维度
- print_every：每多少次打印损失
- save_every：每多少次保存模型
- load_pretrain:与训练模型路径
- voc：词典
- pairs:训练语句对
- bidirectional：是否双向
- dropout：dropout失活概率
- use_ATTN：是否使用注意力机制
- rnn_type：单元类型，'LSTM'或'GRU'


## 测试
执行```python main_test.py```执行测试。指定模型路径后，即可进行对话。'q'终止。

## 参考
- [Sequence to Sequence Learning with Neural Networks
](http://papers.nips.cc/paper/5346-sequence-to-sequence-learning-with-neural-networks)
- [pytorch-seq2seq基础篇](https://fgc.stpi.narl.org.tw/activity/videoDetail/4b1141305df38a7c015e194f22f8015b)
- [吴恩达DeepLearning课程-squence models](https://www.coursera.org/learn/nlp-sequence-models)
- [pytorch-Docs](https://pytorch.org/docs/)
- [pytorch-LSTM](https://www.cnblogs.com/duye/p/9913386.html)
