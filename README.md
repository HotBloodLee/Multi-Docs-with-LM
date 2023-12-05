# Multi-Docs-with-LM

## 简介

搭建适应多文档的LM，基于微调、prompt等方式

## 现有工作

高效LM最近证明，通过简单地将多个文档连接到一个序列中，Transformer可以卸载识别和连接文档之间相关信息的目标。最近，有人建议这些长上下文的LMs可以配备新的预训练目标，使它们能够更有效地处理多个文档。

这种模型通常使用数据集进行预训练，其中每个实例都是一组相关文档(例如，新闻文章都讨论特定事件)，这有助于跨文本关系的建模。现有的多文档预训练目标包括揭示文档中的掩码令牌，或生成显著的掩码句子，鼓励模型使用其他文档来恢复缺失信息。虽然这些模型是成功的，但它们要么局限于分类任务，要么主要用于总结。

针对长文本的Transformer通常使用sparse self-attention架构（Longformer、LongT5等），通过稀疏注意力来让单文档模型适应多文档任务。通过使用局部滑动窗口(称为局部注意)和几个特定输入位置的全局注意模式的组合来稀疏变压器的完整自注意矩阵。

其中，Lonformer是建立在BART模型的基础上，通过使用额外的位置嵌入和全局注意力权重，并引入了在预选令牌上运行的全局注意力模式。LongT5通过使用ETC和BIGBIRD模型中引入的类似技术扩展了T5模型，通过自动全球化令牌组的聚合表示，减轻了手动选择全局令牌的需求。

之后，跨文档语言模型(CDLM：Cross-Document Language Model) 建议在相关文档集上预训练一个更长的编码器，并在几个多文档任务上显示出优越的性能结果。遵循这种方法，LinkBERT的作者使用了类似的方法，但使用了维基百科的超链接，以便为LM预训练策划链接文档的信息对。

为了在序列到序列的任务上采用多文档预训练方法，建立在Longformer(LED)之上的PRIMERA使用金字塔估计方法在相关文档的聚类中选择显著句子，类似于用于预训练单文档PEGASUS模型的方法。这些工作假设并表明，利用生成的QA数据进行预训练可以鼓励上下文表示为其他非QA下游任务编码有用的语义信息。

另一个相关的工作包括将大规模QA生成的数据合并到预训练LM中的方法。

利用跨文档问答任务来指导模型同时理解细粒度的短文本生成和粗粒度的长文本生成，同时涉及多文档的内容理解：github：[peekacross](https://github.com/aviclu/peekacross)

数据生成：生成多文档的问答对（github：[peekacross](https://github.com/aviclu/peekacross)）

未来的工作可以扩展本工作（github：[peekacross](https://github.com/aviclu/peekacross)）的思想，使用我们提出的方法为只有解码器的大型LMs配备跨文档建模，以及设置上下文学习和提示调优。我们预见到，我们的方法对于检索增强语言建模设置(Izacard等人，2022)应该是特别重要的，其中使用相关文档作为外包的外部非参数知识来源。最后，可以进一步研究使用单个文档来触发跨文档关系，正如本工作中首先介绍的那样。