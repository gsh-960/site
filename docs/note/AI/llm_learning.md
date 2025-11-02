# 对话,参数与状态管理
## 对话
大模型的问答工作流程是怎么样的，下面以“ACP is a very”为输入文本向大模型发起一个提问，下图展示从发起提问到输出文本的完整流程。

<a href="https://img.alicdn.com/imgextra/i1/O1CN01yLBhyu1gSAlt3oI0p_!!6000000004140-2-tps-2212-1070.png" target="_blank">
<img src="https://img.alicdn.com/imgextra/i1/O1CN01yLBhyu1gSAlt3oI0p_!!6000000004140-2-tps-2212-1070.png" width="800">
</a>

大模型的问答工作流程有以下五个阶段：

**第一阶段：输入文本分词化**

分词（Token）是大模型处理文本的基本单元，通常是词语、词组或者符号。我们需要将“ACP is a very”这个句子分割成更小且具有独立语义的词语（Token），并且为每个Token分配一个ID。如果您对通义千问的tokenizer细节感兴趣，请参考：[Tokenization](https://github.com/QwenLM/Qwen/blob/main/tokenization_note_zh.md)。

<a href="https://gw.alicdn.com/imgextra/i1/O1CN019gAS3k1DrhwpIdHl6_!!6000000000270-0-tps-2414-546.jpg" target="_blank">
<img src="https://gw.alicdn.com/imgextra/i1/O1CN019gAS3k1DrhwpIdHl6_!!6000000000270-0-tps-2414-546.jpg" width="800">
</a>

**第二阶段：Token向量化**

计算机只能理解数字，无法直接理解Token的含义。因此需要将Token进行数字化转换（即转化为向量），使其可以被计算机所理解。Token向量化会将每个Token转化为固定维度的向量。


**第三阶段：大模型推理**

大模型通过大量已有的训练数据来学习知识，当我们输入新内容，比如“ACP is a very”时，大模型会结合所学知识进行推测。它会计算所有可能Token的概率，得到候选Token的概率集合。最后，大模型通过计算选出一个Token作为下一个输出。

这就解释了为什么当询问公司的项目管理工具时，模型无法提供内部工具的建议，这是因为其推测能力是基于已有的训练数据，对它未接触的知识无法给出准确的回答。因此，在需要答疑机器人回答私域知识时，需要针对性地解决这一问题，在本小节第3部分会进一步阐述。

**第四阶段：输出Token**

由于大模型会根据候选Token的概率进行随机挑选，这就会导致“即使问题完全相同，每次的回答都略有不同”。为了控制生成内容的随机性，目前普遍是通过temperature和top_p来调整的。

例如，下图中大模型输出的第一个候选Token集合为“informative（50%）”、“fun（25%）”、“enlightening（20%）”、“boring（5%）”。通过调整temperature或top_p参数，将影响大模型在候选Token集合中的选择倾向，如选择概率最高的“informative”。你可以在本小节 2.2 中进一步了解这两个参数。

<a href="https://img.alicdn.com/imgextra/i3/O1CN01N93ZE81e6zAZA4TiK_!!6000000003823-0-tps-582-340.jpg" target="_blank">
<img src="https://img.alicdn.com/imgextra/i3/O1CN01N93ZE81e6zAZA4TiK_!!6000000003823-0-tps-582-340.jpg" width="180">
</a>

特别地，“informative”会被继续送入大模型，用于生成候选Token。这个过程被称为自回归，它会利用到输入文本和已生成文本的信息。大模型采用这种方法依次生成候选Token。

**第五阶段：输出文本**

循环第三阶段和第四阶段的过程，直到输出特殊Token（如<EOS>，end of sentence，即“句子结束”标记）或输出长度达到阈值，从而结束本次问答。大模型会将所有生成的内容输出。当然你可以使用大模型的流式输出能力，即预测一些Token立即进行返回。这个例子最终会输出“ACP is a very informative course.”。

!!! note 
    AI请求是无状态的，每次请求都是独立的，如果想要AI模仿人与人之间的对话，需要将历史对话作为新请求的一部分，一并通过API发送给大模型，从而实现多轮对话
    可以参考[多轮对话](https://help.aliyun.com/zh/model-studio/multi-round-conversation)。

## 参数
假设在一个对话问答场景中，用户提问为：“在大模型ACP课程中，你可以学习什么？”。为了模拟大模型生成内容的过程，我们预设了一个候选Token集合，这些Token分别为：“RAG”、“提示词”、“模型”、“写作”、“画画”。大模型会从这5个候选Token中选择一个作为结果输出（next-token），如下所示。
> 用户提问：在大模型ACP课程中，你可以学习什么？<br><br>
> 大模型回答：RAG <br>

在这个过程中，有两个重要参数会影响大模型的输出：temperature 和 top_p，它们用来控制大模型生成内容的随机性和多样性。下面介绍这两个参数的工作原理和使用方式。

### 2.2.1 temperature：调整候选Token集合的概率分布
在大模型生成下一个词（next-token）之前，它会先为候选Token计算一个初始概率分布。这个分布表示每个候选Token作为next-token的概率。temperature是一个调节器，它通过改变候选Token的概率分布，影响大模型的内容生成。通过调节这个参数，你可以灵活地控制生成文本的多样性和创造性。

为了更直观地理解，下图展示了不同temperature值对候选Token概率分布的影响。绘图代码放置在 /resources/2_1 文件目录下。

<a href="https://img.alicdn.com/imgextra/i4/O1CN0137QeqL1o3uhFmaXHU_!!6000000005170-0-tps-3538-1242.jpg" target="_blank">
<img src="https://img.alicdn.com/imgextra/i4/O1CN0137QeqL1o3uhFmaXHU_!!6000000005170-0-tps-3538-1242.jpg" width="1000">
</a>

图中的低、中、高温度基于通义千问Max模型的范围[0, 2)划分。

由上图可知，温度从低到高（0.1 -> 0.7 -> 1.2），概率分布从陡峭趋于平滑，候选Token“RAG”从出现的概率从0.8 -> 0.6 -> 0.3，虽然依然是出现概率最高的，但是已经和其它的候选Token概率接近了，最终输出也会从相对固定到逐渐多样化。

针对不同使用场景，可参考以下建议设置 temperature 参数：
- 明确答案（如生成代码）：调低温度。
- 创意多样（如广告文案）：调高温度。
- 无特殊需求：使用默认温度（通常为中温度范围）。

!!! note
    需要注意的是，当 temperature=0 时，虽然会最大限度降低随机性，但无法保证每次输出完全一致。如果想深入了解，可查阅 [temperature的底层算法实现](https://github.com/huggingface/transformers/blob/v4.49.0/src/transformers/generation/logits_process.py#L226)。

### 2.2.2 top_p：控制候选Token集合的采样范围

top_p 是一种筛选机制，用于从候选 Token 集合中选出符合特定条件的“小集合”。具体方法是：按概率从高到低排序，选取累计概率达到设定阈值的 Token 组成新的候选集合，从而缩小选择范围。

下图展示了不同top_p值对候选Token集合的采样效果。

<a href="https://img.alicdn.com/imgextra/i4/O1CN01tL546W1x4DYoeKICj_!!6000000006389-0-tps-2732-1282.jpg" target="_blank">
<img src="https://img.alicdn.com/imgextra/i4/O1CN01tL546W1x4DYoeKICj_!!6000000006389-0-tps-2732-1282.jpg" width="700">
</a>

图示中蓝色部分表示累计概率达到top_p阈值（如0.5或0.8）的Token，它们组成新的候选集合；灰色部分则是未被选中的Token。

当top_p=0.5时，模型优先选择最高概率的Token，即“RAG”；而当top_p=0.8时，模型会在“RAG”、“提示词”、“模型”这三个Token中随机选择一个生成输出。


由此可见，top_p值对大模型生成内容的影响可总结为：
- 值越大 ：候选范围越广，内容更多样化，适合创意写作、诗歌生成等场景。
- 值越小 ：候选范围越窄，输出更稳定，适合新闻初稿、代码生成等需要明确答案的场景。
- 极小值（如 0.0001）：理论上模型只选择概率最高的 Token，输出非常稳定。但实际上，由于分布式系统、模型输出的额外调整等因素可能引入的微小随机性，仍无法保证每次输出完全一致。

**是否需要同时调整temperature和top_p？**

为了确保生成内容的可控性，建议不要同时调整top_p和temperature，同时调整可能导致输出结果不可预测。你可以优先调整其中一种参数，观察其对结果的影响，再逐步微调。

!!!note
    <br>**知识延展：top_k**<br>
    top_p和temperature这两个是大模型的核心参数，一般的模型都会设置，还有其他一些参数，例如top_k，不同的模型可能也会支持其他一些参数
    在通义千问系列模型中，参数top_k也有类似top_p的能力，可查阅[通义千问API文档](https://help.aliyun.com/zh/model-studio/developer-reference/use-qwen-by-calling-api?spm=a2c4g.11186623.help-menu-2400256.d_3_3_0.68332bdb2Afk2s&scm=20140722.H_2712576._.OR_help-V_1)。它是一种采样机制，从概率排名前k的Token中随机选择一个进行输出。一般来说，top_k越大，生成内容越多样化；top_k越小，内容则更固定。当top_k设置为1时，模型仅选择概率最高的Token，输出会更加稳定，但也会导致缺乏变化和创意。<br><br>
    **知识延展：seed**<br>
    在通义千问系列模型中，参数seed也支持控制生成内容的确定性，可查阅[通义千问API文档](https://help.aliyun.com/zh/model-studio/developer-reference/use-qwen-by-calling-api?spm=a2c4g.11186623.help-menu-2400256.d_3_3_0.68332bdb2Afk2s&scm=20140722.H_2712576._.OR_help-V_1)。在每次模型调用时传入相同的seed值，并保持其他参数不变，模型会尽最大可能返回相同结果，但无法保证每次结果完全一致。<br><br> 


## 状态管理
状态管理是将底层无状态的AI服务，报装成用户感知上有状态体验的关键技术
![](imgs/VUnVdG.png)
![](imgs/DoHBsV.png)

## 上下文与记忆系统详解
// todo
## LangChain开发工具链入门
// todo