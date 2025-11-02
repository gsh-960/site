# AI的发展历程
!!! note
    为什么要学 AI 应用开发？ 作为一名从传统开发转型过来的工程师，我深知向 AI 转变带来的挑战。以前写 API、设计数据库，逻辑都是确定性的：输入什么，输出什么，一切尽在掌控中。但 AI 应用开发不同——你要学会与"不确定性"共舞。大模型的输出可能不稳定，用户意图可能模糊,检索的知识可能不准。
    这个核心转变在于：从"确定性编程"到"需要去设计 Prompt、管理上下文、处理幻觉、优化检索策略的概率性工程"。需要掌握全新的技术栈——例如模型、向量数据库、RAG 架构、Agent 编排，而这些在传统开发教材里是找不到的。 更困惑的是学习路径的断层。网上的教程可能让你先从神经网络、Transformer 开始...花两个月啃完，却发现距离实际业务需求还很远，比如开发知识库、智能客服、Agent 开发等。但 AI 应用开发并不一定需要从零训练模型——用好现成的模型 + 工程能力 + 明确场景，就能做出解决实际问题的应用。

## 1950s 思想萌芽
[图灵测试](https://baike.baidu.com/item/%E5%9B%BE%E7%81%B5%E6%B5%8B%E8%AF%95/1701255) (Turing Test)
提出者: 艾伦·图灵(Alan Turing)  
时间: 1950年  
核心概念: 通过人类评判员与机器和人类进行文本对话，如果评判员无法区分哪个是机器，则认为该机器具备智能  
意义: 奠定了人工智能测试评估的理论基础  

[达特茅斯会议](https://baike.baidu.com/item/%E8%BE%BE%E7%89%B9%E8%8C%85%E6%96%AF%E4%BC%9A%E8%AE%AE?fromModule=lemma_search-box) (Dartmouth Conference)  
时间: 1956年  
组织者: 约翰·麦卡锡(John McCarthy)  
成果: 正式确立"人工智能"这一术语和学科领域  
参会者: 包括马文·明斯基(Marvin Minsky)等计算机科学先驱  

## 1980s
1980s 知识工程时期  
专家系统兴起  
[MYCIN系统](https://baike.baidu.com/item/MYCIN%E7%B3%BB%E7%BB%9F?fromModule=lemma_search-box): 医疗诊断专家系统，用于细菌感染诊断和抗生素处方建议  
[DENDRAL系统](https://baike.baidu.com/item/DENDRAL%E7%B3%BB%E7%BB%9F?fromModule=lemma_search-box): 化学结构推断专家系统，最早的专家系统之一  
[XCON系统](https://baike.baidu.com/item/DENDRAL%E7%B3%BB%E7%BB%9F?fromModule=lemma_search-box): DEC公司计算机配置专家系统  

技术特点  
基于规则的推理机制  
知识库与推理引擎分离  
领域专业知识的形式化表达  

## 2010s
神经网络核心技术突破

硬件支持
GPU并行计算能力大幅提升  
大规模分布式计算平台成熟  
云计算资源普及  

里程碑事件  
[ImageNet挑战](https://baike.baidu.com/item/ImageNet?fromModule=lemma_search-box#3): 推动计算机视觉技术快速发展  
[AlphaGo](https://baike.baidu.com/item/%E9%98%BF%E5%B0%94%E6%B3%95%E5%9B%B4%E6%A3%8B/19319610?fromtitle=AlphaGo&fromid=19315265&fromModule=lemma_search-box): 2016年击败世界围棋冠军李世石

## 2020s
生成式AI爆发 代表性产品 ChatGPT: OpenAI开发的对话式AI助手

# AI、机器学习、深度学习的区别和联系
## 机器学习
![机器学习](imgs/Z3YJV5.png)
## 深度学习
基于多层神经网络的机器学习方法，通过多层次的非线性变换来学习数据的特征表示它的核心思想是模仿人脑神经网络的工作方式来处理信息。
非线性和神经网络是深度学习两个重要特征，这意味着深度学习模型可以输出不能用数学公式表达的结果，更加接近人脑的工作方式。
## LLM(大语言模型)
大语言模型像是一个读过全世界图书的大脑    
通常拥有数十亿到数千亿超大规模参数  
过大规模预训练学习通用知识  
能够理解和生成自然语言文本










