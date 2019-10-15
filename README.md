# ccks2019-ckbqa-4th-codes
中文知识库问答代码，在CCKS2019 CKBQA评测获得67.6%的F值，第四名
## 任务介绍
  这个评测任务主要是基于中文开放域知识库的智能问答，主办方是北京大学的邹磊、胡森老师。知识库是他们搭建的PKUBASE，语料在https://github.com/pkumod/CKBQA 上。我们主要是参考了ccks2018 COQA评测第二名的方法，在此基础上加入了基于Bert的序列标注、语义匹配等模型，以及桥接等回答复杂问题的环节。具体方法在评测论文里，和本次比赛前三名的方法一起放在了/pdf下供参考。
## 代码介绍
  ### 文件结构：
  - src: 主要的程序文件
  - helper: 用于生成一些字典/语料集预处理的程序文件
  - data: 存放各种字典，中间文件及最终答案
  - corpus: 原始语料集文件
  - PKUBASE: 评测方提供的知识库文件
  ### 依赖字典：
  这些字典均可通过helper中的程序生成
  - question_2_mention.pkl: 一个基于Bert的序列标注模型识别出的问题中mention，因为直接调用该模型比较困难，故存成字典
  - segment_dic.txt: 分词词典
  - prop_dic.pkl: 知识库中的属性值字典
  - mention2entity_dic.pkl: 实体链接字典，实际就是评测方提供的文件存成了pkl格式
  - char_2_prop.pkl：知识库中字符to属性值的倒排索引，用于属性值模糊匹配
  ### 训练流程：
  - mention_extractor.py: 抽取问题中的实体mention
  - prop_extractor.py: 抽取问题中的属性值
  - entity_extractor.py: 将mention prop链接到知识库，得到候选实体，并为每个实体生成一些人工特征
  - entity_filter.py: 根据特征，对候选实体进行逻辑回归筛选，保存逻辑回归模型
  - tuple_extractor.py: 根据候选实体从知识库中抽取两跳内的关系作为候选答案，并根据微调后的bert文本匹配模型，生成问题+候选答案的相似度特征
  - tuple_filter.py: 根据bert相似度特征对候选答案进行排序筛选
  ### 测试流程
  - answer_bot.py: 该程序会依次调用上述文件，并针对双实体问题进行桥接，最终输出基于神经网络的方法可提交的结果
  - 语义解析模块.ipynb：针对上述基于神经网络方法难以回答且结构清晰的问题，写了一些正则式进行补充，主要还是得到最终可提交结果
  ### 主要参考
  - 预训练的bert文本匹配： https://github.com/terrifyzhao/bert-utils
