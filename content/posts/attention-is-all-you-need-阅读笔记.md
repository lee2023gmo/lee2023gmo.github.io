+++
date = '2026-08-03T15:38:37+08:00'
draft = false
title = 'Attention Is All You Need 阅读笔记'
+++
《Attention Is All You Need》作为提出了Transformer的经典之作，最近刚读完，写点东西来回顾一下这篇文章。

文章的第一，二部分都是一个简单的介绍，没什么好说的。
第三部分算是文章的主体部分，讲解transformer的具体架构.
首先是架构的俩个主体部分，一个是encoder，一个是decoder。Encoder用来汇总输入的向量组的信息，由N个block组成，每个block的架构完全一样，都是一个self attention层后进行residual，再对每个输出的向量分别进行layer-normalization，后面再接一个feed forward（其实就是一个俩层的MLP，中间用relu做激活函数）然后再来个residual和layer-normalization,就完事了。Decoder比encoder稍微复杂一点，把encoder中的self attention加一个mask，同时中间再加个cross attention层。在decoder最后一个块输出后，我们再接上一个线性层和softmax层得到最终得到一个表示概率的向量。(注：residual是指将输入的值加到输出上得到一个新的输出，layer-normalization则是一种对向量的操作，求出一个向量所有分量的平均数，在求出其标准差，每个分量减去平均数再除以标准差得到新的值。)

之后讲解了transformer的核心机制attention。Attention是一种接受一组向量的输入，输出相同数量的另一组向量的架构，而输出中每一个向量都考虑了所有输入向量的信息，实现了仅用一层网络就考虑输入中任何俩个位置的关系。Attention层有三个可训练参数，分别是Wq,Wk,Wv。将输入视为行向量，按顺序拼接成输入矩阵I，则得矩阵Q=I*Wq,K=I*Wk,V=I*Wv，则输出矩阵O可表示为O=softmax(QKT/sqrt(dk))*V，(dk是Wq的列数)这里得softmax是指对矩阵的每个行向量分别做softmax除以根号dk则是因为点积的方差随 d_k 增大而变大，导致 softmax 输出集中在 0 或 1 的饱和区，梯度变得非常小（梯度消失），所以才用 1/√d_k 进行缩放。这个公式体现了self attention高度的可并行化程度，用一个简单的公式就可以把整个层所干的所有事情说清楚，这也是transformer最后能淘汰rnn等网络的重要原因。至于多头注意力，也很好理解，就是作h次独立的小self attention，把h次输出的向量拼接起来，就得到了最终的输出。然后文章讲解了attention在transformer中作用的地方，跟我们最开始描述的架构一致，只讲一下cross attention和mask。Cross attention作用是让decoder能够得到输入数据的信息，记cross attention层输入向量拼接起来的矩阵维D,encoder的最终输出组成的矩阵为E，在原本的公式中，将Q=D*Wq，K=E*Wk,V=E*Wv,就是cross attention。至于mask，目的是为了不让decoder输入序列的信息向左传递（为了保证训练时的并行化，transformer在训练时是直接将答案整个地输入decoder层，而不是像测试时那样等decoder输出一个词后再把这个词对应地向量作为输入再输入到decoder层中去，如果不加mask，模型在训练时可以直接看到答案是怎样的，相当于写作业的时候直接抄答案，自然不能得到好的训练效果），技术上实现也很简单，取一个矩阵M,对角线右上方的值全为负无穷，其他地方为0，将QKT/sqrt(dk)与M相加后再做softmax，就能到达目的了。

然后是feed forward，没什么好讲的，就是一个普通的MLP。

接下来是词嵌入（也就是embedding），文章采用的是可学习的词嵌入（通过一个矩阵将one-hot向量转变为词嵌入向量），值得一提的是，为了减少训练时间，encoder和decoder的词嵌入向量使用的矩阵W是完全相同的，而decoder后的那个线性层则是使用W的转置。文章还提到在词嵌入层会将输出向量乘以sqrt(d_model)。（但是文章并未说明原因，询问ai知是为了不让后来的postion encoding带来的位置信息淹没原本的语义信息）

主体部分的最后讲的是postion encoding，不难发现，self attention各个位置之间是完全对称的（如果你调换俩个输入向量的位置，对输出向量的作用相当于调换这俩个输入向量对应的输出向量的位置），没有考虑到各个向量之间的相对和绝对位置关系，但是，自然语言中词的位置关系是及其重要的信息，因此，我们引入了postion encoding。文章中采用了正弦波形式的postion encoding，用来考虑各个词之间的相对和绝对位置关系，表现与可学习的postion encoding相差无几。

第四部分讲了attention相比于rnn和cnn的优势，简单总结就是在部分时候运行速度快，且对矩阵乘法加速的相关研究进一步放大了这个优势，而且能仅靠一层网络就考虑任意俩个位置之间的相关性，和高度的并行化能力。

第五部分讲的是训练的相关信息，包括数据的获取，使用的gpu，优化器和防过拟合的方法。前三者没什么好讲的，防过拟合的方法包括residual dropout和label smoothing。Residual dropout是指将各层的输出进行dropout后再进行residual，而label smoothing则是指将训练数据中词对应的one hot向量中该词对应维的值设为1-ε而非1，剩下的维度平分ε，放置模型走向极端。

最后面就是一些讲transformer的牛逼之处和简单的总结，也没什么好说的，统统略过（）。
