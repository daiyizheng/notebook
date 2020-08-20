---
title: 知识图谱问答KBQA数据集介绍
date: 2020-08-08 09:28:33
summary: KBQA数据和相关论文
tags:
  - 问答数据
  - 数据相关论文
categories: 数据集
---



转载 https://blog.csdn.net/qq_37974244/article/details/107020421

# 一、WebQuestions

提出该数据集的论文：[Semantic Parsing on Freebase from Question-Answer Pairs](https://www.aclweb.org/anthology/D13-1160/)

数据集地址：https://worksheets.codalab.org/worksheets/0xba659fe363cb46e7a505c5b6a774dc8a

WebQuestions数据集（2013年提出）是由斯坦福大学研究人员通过Google Suggest API构建得到的，数据集本身共包含5810条（问题，答案）对，其中简单问题占比在84%，复杂的多跳和推理问题相对较少。根据提出者的最初数据划分方式，WebQuestions被分为训练集和测试集两个集合，其中训练集包含3778条数据，测试集包含2032条数据。

# 二、SimpleQuestions

提出该数据集的论文：[Large-scale Simple Question Answering with Memory Networks](https://arxiv.org/pdf/1506.02075.pdf)

数据集地址：https://research.fb.com/downloads/babi/

SimpleQuestions数据集（2015年提出）是一个针对简单问题而构建的数据集，它采用人工标注的方法根据知识库中的事实生成对应的问句，并且以**Freebase**作为答案来源。该数据集共包含108,442条数据（包含关系标注），其中训练集为75910条（70%），验证集为10845条（10%），测试集为21687条（20%）。

# 三、ComplexQuestions

提出该数据集的论文：[Constraint-Based Question Answering with Knowledge Graph](https://www.aclweb.org/anthology/C16-1236/)

作者本人的数据集地址：https://github.com/JunweiBao/MulCQA/tree/ComplexQuestions

ComplexQuestions数据集（2016年提出）是一个专门针对复杂问题而构建的数据集，在构建该数据集过程中，作者从一个实际使用的搜索引擎（具体哪个暂未知）中筛选并得到了878条可用的问答对。除了这878条数据，作者还从WebQuestions等数据集上额外选出了1222条数据，由此共得到了2100条复杂问题对。总体来说，该数据集共包含2100条问答对，其中训练集个数为1300条，测试集个数为800条。

# 四、GraphQuestions

提出该数据集的论文：[On Generating Characteristic-rich Question Sets for QA Evaluation](https://www.aclweb.org/anthology/D16-1054/)

数据集地址：https://github.com/ysu1989/GraphQuestions

这是一个比较难的数据集（2016年提出），涉及较多比较复杂的逻辑，以**Freebase**作为知识库。该数据集在构建时先设计问题涉及的模式（即知识库中的一个子图），然后让人根据图改写成自然语言题目。举个例子：“the nine eleven were carried out with the involvement of what terrorist organizations?”

# 五、30M Factoid Questions

提出该数据集的论文：[Generating Factoid QuestionsWith Recurrent Neural Networks: The 30M Factoid Question-Answer Corpus](https://arxiv.org/pdf/1603.06807.pdf)

数据集地址：https://academictorrents.com/details/973fb709bdb9db6066213bbc5529482a190098ce

该数据集是由模型自动构建的，包含30M的问答对。按照论文中的说法，问句质量和人类构建的质量相当，很有使用价值。