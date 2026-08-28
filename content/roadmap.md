+++
title = '学习路线'
url = '/roadmap/'
summary = '按知识关系整理博客中的论文阅读笔记。'
+++

这里不按发布时间罗列文章，而是按照知识之间的联系整理阅读顺序。标记为“整理中”的内容会在完成后补上链接。

## 自然语言处理与 Transformer

### 1. 词的连续表示

- [Word2Vec 阅读笔记]({{< relref "/posts/Word2Vec-阅读笔记.md" >}})：从 one-hot 表示走向可学习的词向量，理解 CBOW 与 Skip-gram。

### 2. 序列中的信息交互

- [Attention Is All You Need 阅读笔记]({{< relref "/posts/attention-is-all-you-need-阅读笔记.md" >}})：理解 Self-Attention、Multi-Head Attention、位置编码和 Transformer 架构。
- [Layer Normalization 阅读笔记]({{< relref "/posts/layer-normalization-阅读笔记.md" >}})：理解 Transformer 中归一化模块的计算方式与作用。

### 3. 预训练语言模型

- **BERT 阅读笔记**：整理中。它会连接双向 Transformer 编码器与预训练、微调范式。

## 计算机视觉与深层网络

### 1. 深层网络的优化

- [ResNet 阅读笔记]({{< relref "/posts/ResNet-阅读笔记.md" >}})：理解残差连接、退化问题，以及深层网络为什么更容易训练。

## 接下来准备补充

- Transformer 的逐模块代码实现
- Batch Normalization、Layer Normalization 与 RMSNorm 对比
- BERT 的 Masked Language Modeling 与微调方法
- 从 ResNet 到现代视觉骨干网络
- 论文复现中的环境、实验结果和踩坑记录

> 这张路线图会随着新文章持续更新。如果想按发布时间浏览，可以前往[文章归档]({{< relref "/archives.md" >}})。
