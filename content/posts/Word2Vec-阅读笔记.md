+++
date = '2026-08-19T19:32:59+08:00'
draft = false
title = 'Word2Vec 阅读笔记'
tags = ['深度学习', '论文笔记'] 
categories = ['论文阅读']          
summary = 'word embedding是NLP（自然语言处理）的基石，很多语言模型都要将词（或者token）用连续的词向量而不是one-hot编码表示，本篇笔记旨在讲解提出了CBOW和Skip-gram这俩种词向量训练方法的论文' 
+++
~~新学习了很多markdown格式，这篇笔记中应该会有不少这方面的应用，可能比较生疏，请见谅~~
~~放张镜流镇楼~~
![图片无法显示](/artworks/jingliu1.jpg "镜流")
##论文基本信息
-论文标题：Efficient Estimation of Word Representations in Vector Space
-作者：Tomas Mikolov,Kai Chen，Greg Corrado，Jeffrey Dean
-原始文章：[文章连接](https://arxiv.org/abs/1301.3781)

##笔记主体
###文章介绍
文章首先对NLP中对词的看法做了一个简单介绍。先指出目前很多NLP架构将词看成不可拆分的单元（也就是用one-hot编码表示词语）
