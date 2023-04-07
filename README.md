# 总体概述


&emsp;&emsp;框架语义学（Frame Semantics）是基于认知机理的语言学理论，其核心思想是通过基于认知的图式化场景来描述语言的意义，描述单位是框架[1]。   
&emsp;&emsp;汉语框架网(Chinese FrameNet, 简称CFN)是以框架语义学为理论基础，以汉语真实语料为依据构建的框架语义资源，包括框架、词元、框架关系、例句及篇章。CFN 1.0数据集是由山西大学以汉语真实语料为依据构建的框架语义资源，数据以框架知识及标注例句组成，包含了近700个语义框架及20000条标注例句。CFN 1.0数据集遵循[CC BY-NC 4.0协议](https://creativecommons.org/licenses/by-nc/4.0/)，欢迎广大同行一期加入到CFN的建设中，推进中文信息处理领域的发展。      
&emsp;&emsp;框架语义解析（Frame Semantic Parsing，FSP）是自然语言处理领域中的一项重要任务，其目标是从句中提取框架语义结构[2]，实现对句子中涉及到的事件或情境的深层理解。FSP在阅读理解[3-4]、文本摘要[5-6]、关系抽取[7]等下游任务有着重要意义。目前对于FSP的研究有以下方法：多任务的pipeline策略[8-9]、多种联合学习策略[10-11]和基于框架知识建模的方法[12-14]等。    
<!-- 
&emsp;&emsp;框架语言学（Frame Semantics）是一种语言学理论，它强调语言中词汇的语义是由概念框架（Frame）所组成的。概念框架是一种认知结构，用于组织和理解我们对世界的经验和知识。汉语框架网(Chinese FrameNet, 简称CFN)是以Fillmore的框架语义学为理论基础，以FrameNet为参照，以汉语真实语料为依据构建的框架语义资源，包括框架、词元、框架关系、例句及篇章。  
&emsp;&emsp;框架语义解析（Frame Semantic Parsing）[1]是自然语言处理领域中的一项重要任务，其目标是将自然语言文本中的句子映射到语义框架中，从而实现对句子中涉及到的事件或情境的深层理解，目前对于该任务已有大量相关研究，如引入框架知识来识别和理解文本中的语义结构，并基于此进行框架识别、框架角色标注和框架语义解析等工作[2-4]。以上任务对自然语言处理中的各种下游任务有着重要意义。例如：在阅读理解任务中，利用框架和框架元素建模语义交互信息，提升模型语言理解能力[5-6]。在文本摘要任务中，利用框架语义有效地解析文本中的关键信息和核心概念，以便更好地生成摘要[7-8]。在关系抽取任务中，建模语义场景表示并用于远程监督，有效提升了模型性能[9]。  
&emsp;&emsp;CFN 1.0数据集由山西大学以FrameNet为参照，以汉语真实语料为依据构建的框架语义资源，数据以框架知识及标注例句组成，包含框架识别、论元范围识别和论元角色识别三个子任务。CFN数据集包含了超过600个语义框架及15000条标注例句。对于汉语框架语义解析评测，我们将原始数据集的格式统一更新成JSON格式方便选手统一处理。   -->
# 任务介绍

&emsp;&emsp;汉语框架语义解析（Chinese FSP，CFSP）是基于汉语框架网(Chinese FrameNet, CFN)的语义解析任务，分为以下三个子任务：
   1.	子任务1: 框架识别（Frame Identification），识别句子中给定目标词激活的框架。  
   2.	子任务2: 论元范围识别（Argument Identification），识别句子中给定目标词所支配论元的边界范围。  
   3.	子任务3: 论元角色识别（Role Classification），预测子任务2所识别论元的语义角色标签。  

## 框架识别（Frame Identification）
### 1. 任务描述

&emsp;&emsp;框架识别任务是框架语义学研究中的核心任务，其要求根据给定句子中目标词的上下文语境，为其寻找一个可以激活的框架。框架识别任务是自然语言处理中非常重要的任务之一，它可以帮助计算机更好地理解人类语言，并进一步实现语言处理的自动化和智能化。具体来说，框架识别任务可以帮助计算机识别出句子中的关键信息和语义框架，从而更好地理解句子的含义。这对于自然语言处理中的许多任务都是至关重要的。

### 2. 任务说明

&emsp;&emsp;该任务给定一个包含目标词 $w_t$ 的句子 $S$ ，任务需要根据目标词语境识别出激活的框架，并给出识别出的框架名称。  
   1. 输入：句子相关信息（id、文本内容和分词结果）及目标词，数据为json格式给出，数据信息详情见：[评测数据](#dataform)。  
   2. 输出：句子id及目标词所激活框架的识别结果，数据为json格式，样例如下：  

      ```json
      [
         [ 2611, "等同" ]
      ]
      ```

### 3. 评测指标

&emsp;&emsp;框架识别采用正确率作为评价指标：

   $$task1\\_acc = 正确识别的个数 / 总数$$


## 论元范围识别（Argument Identification）

### 1. 任务描述

&emsp;&emsp;给定一条汉语句子及目标词，在目标词已知的条件下，从句子中自动识别出目标词所搭配的语义角色的边界。该任务的主要目的是确定句子中每个目标词所涉及的论元（即框架元素）在句子中的位置。论元范围识别任务对于框架语义解析任务来说非常重要，因为正确识别谓词和论元的范围可以帮助系统更准确地识别论元的语义角色，并进一步分析句子的语义结构。

### 2. 任务说明

&emsp;&emsp;论元范围识别任务是指，在给定包含目标词的句子中，识别出目标词所支配的语义角色的边界。
   1. 输入：句子相关信息（id、文本内容和分词结果）及目标词，数据为json格式给出，数据信息详情见：[评测数据](#dataform)。   
   2. 输出：句子id，及识所别出的论元范围，每个元组包含例句id：`tasi_id`, `span`起始位置, `span`结束位置，样例如下：
      ```json
      [
         [ 1148, 0, 2 ],
         [ 1148, 4, 17 ]
      ]
      ```

### 3. 评测指标

&emsp;&emsp;框架识别采用P、R、F1作为评价指标：
   $${\rm{precision}} = \frac{{{\rm{InterSec(gold,pred)}}}}{{{\rm{Len(pred)}}}}$$  
   
   $${\rm{recall}} = \frac{{{\rm{InterSec(gold,pred)}}}}{{{\rm{Len(gold)}}}}$$  
   
   $${\rm{task2\\_f1}} = \frac{{{\rm{2\*precision\*recall}}}}{{{\rm{precision}} + {\rm{recall}}}}$$  
   
   其中：gold 和 pred 分别表示真实结果与预测结果，InterSec(\*)表示计算二者共有的token数量， Len(\*)表示计算token数量。

## 论元角色识别（Role Classification）

### 1. 任务描述

&emsp;&emsp;框架语义解析任务中，论元角色识别任务是非常重要的一部分。该任务旨在确定句子中每个论元对应的角色标签，即每个论元在所属框架中的语义角色。例如，在“我昨天买了一本书”这个句子中，“我”是“商业购买”框架中的“买方”框架元素，“一本书”是“商品”框架元素。论元角色识别任务的完成对于许多自然语言处理任务都是至关重要的，例如信息提取、关系抽取和机器翻译等。它可以帮助计算机更好地理解句子的含义，从而更准确地提取句子中的信息，进而帮助人们更好地理解文本。

### 2. 任务说明

&emsp;&emsp;论元范围识别任务是指，在给定包含目标词的句子中，识别出目标词所支配语义角色的边界及其角色名称。  
&emsp;&emsp;框架及其框架元素的所属关系在`frame_info.json`文件中，框架示例信息详见：[评测数据](#dataform)：  
   1. 输入：句子相关信息（id、文本内容和分词结果）、目标词、task1识别出的框架信息以及task2识别出的论元角色范围，数据为json格式给出，数据信息详情见：[评测数据](#dataform)。   
   2. 输出：句子id，及框架元素识别的结果。  
      ```json
      [
         [ 1148, 0, 2, "实体1" ],
         [ 1148, 4, 17, "实体1" ]
      ]
      ```

### 3. 评测指标

&emsp;&emsp;框架识别采用正确率作为评价指标：

   $${\rm{precision}} = \frac{{{\rm{Count(gold \cap pred)}}}} {{{\rm{Count(pred)}}}}$$  
   
   $${\rm{recall}} = \frac{{{\rm{Count(gold \cap pred)}}}} {{{\rm{Count(gold)}}}}$$  
   
   $${\rm{task3\\_f1}} = \frac{{{\rm{2\*precision\*recall}}}}{{{\rm{precision}} + {\rm{recall}}}}$$  

   其中，gold 和 pred 分别表示真实结果与预测结果， Count(\*) 表示计算集合元素的数量。


# 结果提交
   &emsp;&emsp;本次评测结果在阿里天池平台上进行提交和排名。参赛队伍需要在阿里天池平台的“提交结果”界面提交预测结果，提交的压缩包命名为submit.zip，其中包含三个子任务的预测文件。
   + submit.zip
      + task1_test.json
      + task2_test.json
      + task3_test.json
  
   &emsp;&emsp;选⼿可以只提交部分任务的结果，如只提交“task1”任务：`zip submit.zip task1_test.json`，未预测任务的分数默认为0。

# 系统排名

1. 所有评测任务均采用百分制分数显示，小数点后保留2位。
2. 系统排名取各项任务得分的加权和（三个子任务权重依次为 0.3，0.3，0.4），即：
	$${\rm{task\\_score=0.3\*task1\\_acc+0.3\*task2\\_f1+0.4\*task3\\_f1}}$$
   
4. 如果某项任务未提交，默认分数为0，仍参与到系统平均分的计算。

# <span id="dataform">评测数据</span>

## 数据集概述
   &emsp;&emsp;本评测使用山西大学提供的CFN数据集，包含标注例句及相关的框架信息。标注例句相关信息如下：
   |数据集划分 | Train | Dev | Test_A |Test_B| All|
   |-------|------|-------|------|------|----|
   |例句数 | 10000 | 2000 | 4000 |4000| 20000|
   |框架数 | 671 | 354 | 432 | 446 | 695|
   |框架元素数 | 947 | 649 | 711 |723 | 987|
   |词元数量 | 2359 | 670 | 931 |976 | 2596|

   &emsp;&emsp;标注数据由json格式给出，数据集包含以下内容：

   + CFN-train.json: 训练集标注数据。
   + CFN-dev.json: 验证集标注数据。
   + CFN-test-A.json: A榜测试集。
   + CFN-test-B.json: B榜测试集。
   + frame_info.json: 框架信息。
   + result：提交示例。
      + task1_test.json：task1子任务提交示例。
      + task2_test.json：task2子任务提交示例。
      + task3_test.json：task3子任务提交示例。
   + README.md: 说明文件。

## 数据说明
   1. 标注数据的字段信息如下  
   + sentence_id：例句id
   + cfn_spans：框架元素标注信息。
   + frame：例句所激活的框架名称。
   + target：目标词的相关信息
      + start：目标词在句中的起始位置
      + end：目标词在句中的结束位置
      + pos：目标词的词性
   + text：标注例句
   + word：例句的分词结果及其词性信息
   + 
   &emsp;&emsp;数据样例：
   ```json
   [{
      "sentence_id": 2611,
      "cfn_spans": [
         { "start": 0, "end": 2, "fe_abbr": "ent_1", "fe_name": "实体1" },
         { "start": 4, "end": 17, "fe_abbr": "ent_2", "fe_name": "实体2" }
      ],
      "frame": "等同",
      "target": { "start": 3, "end": 3, "pos": "v" },
      "text": "餐饮业是天津市在海外投资的重点之一。",
      "word": [
         { "start": 0, "end": 2, "pos": "n" },
         { "start": 3, "end": 3, "pos": "v" },
         { "start": 4, "end": 6, "pos": "nz" },
         { "start": 7, "end": 7, "pos": "p" },
         { "start": 8, "end": 9, "pos": "n" },
         { "start": 10, "end": 11, "pos": "v" },
         { "start": 12, "end": 12, "pos": "u" },
         { "start": 13, "end": 14, "pos": "n" },
         { "start": 15, "end": 16, "pos": "n" },
         { "start": 17, "end": 17, "pos": "wp" }
      ]
   }]
   ```  

   2. 框架信息在`frame_info.json`中，框架数据的字段信息如下：
   + frame_name：框架名称
   + frame_ename：框架英文名称
   + frame_def：框架定义  
   + fes：框架元素信息
      + fe_name：框架元素名称  
      + fe_abbr：框架元素缩写  
      + fe_ename：框架元素英文名称  
      + fe_def：框架元素定义  
   
   数据样例：
   ```json
   [{
   "frame_name": "等同",
   "frame_ename": "Equating",
   "frame_def": "表示两个实体具有相等、相同、同等看待等的关系。",
   "fes": [
      { "fe_name": "实体集", "fe_abbr": "ents", "fe_ename": "Entities", "fe_def": "具有同等关系的两个或多个实体" },
      { "fe_name": "实体1", "fe_abbr": "ent_1", "fe_ename": "Entity_1", "fe_def": "与实体2具有等同关系的实体" },
      { "fe_name": "实体2", "fe_abbr": "ent_2", "fe_ename": "Entity_2", "fe_def": "与实体1具有等同关系的实体" },
      { "fe_name": "施动者", "fe_abbr": "agt", "fe_ename": "Agent", "fe_def": "判断实体集具有同等关系的人。" },
      { "fe_name": "方式", "fe_abbr": "manr", "fe_ename": "Manner", "fe_def": "修饰用来概括无法归入其他更具体的框架元素的任何语义成分，包括认知的修饰（如很可能，大概，神秘地），辅助描述（安静地，大声地），和与事件相比较的一般描述（同样的方式）。" },
      { "fe_name": "时间", "fe_abbr": "time", "fe_ename": "Time", "fe_def": "实体之间具有等同关系的时间" }
   ]
   }]
   ```
# <span id="datainfo">数据集信息</span>

   &emsp;&emsp;数据集提供方：山西大学智能计算与中文信息处理教育部重点实验室，山西太原 030000。  
   &emsp;&emsp;负责人：谭红叶 tanhongye@sxu.edu.cn。  
   &emsp;&emsp;联系人：闫智超 202022408073@email.sxu.edu.cn、李俊材 202122407024@email.sxu.edu.cn。  
   
# Baseline  
&emsp;&emsp;Baseline下载地址：[Github](https://github.com/SXUNLP/Chinese-Frame-Semantic-Parsing)   
&emsp;&emsp;Baseline表现：  
|task1_acc|task2_fe|task3_f1|task_score|
|---------|--------|--------|----------|
|65.1|87.55|54.07|67.42|

# 如何下载
   &emsp;&emsp;完成报名后，在阿里天池平台下载数据集，数据集的解压密码在报名成功后通过邮件发送。



# 评测赛程

## 报名方式  
   &emsp;&emsp;本次评测采用电子邮件进行报名，邮件标题为：“CCL2023-汉语框架语义解析评测-参赛单位”，例如：“CCL2023-汉语框架语义解析评测-山西大学”；附件内容为队伍的参赛报名表，报名表[点此下载](https://github.com/SXUNLP/Chinese-Frame-Semantic-Parsing/blob/main/%E6%B1%89%E8%AF%AD%E6%A1%86%E6%9E%B6%E8%AF%AD%E4%B9%89%E8%A7%A3%E6%9E%90%E8%AF%84%E6%B5%8B%E6%8A%A5%E5%90%8D%E8%A1%A8.docx)，同时报名表应更名为“参赛队名+参赛队长信息+参赛 单位名称”。请参加评测的队伍发送邮件至202122407024@email.sxu.edu.cn，并同时在阿里天池平台完成报名，完成报名后需加入评测交流群：[CCL2023汉语框架语义解析评测](https://qr.dingtalk.com/action/joingroup?code=v1,k1,AYA8FGKyANIwGH6OYXUxoZtcO5CBNNxBdh1es9GRREc=&_dt_no_comment=1&origin=11)

   &emsp;&emsp;报名截止前未发送报名邮件者不参与后续的评选。

## 赛程安排 
   1.	报名时间：4月1日-4月31日
   2.	训练、验证数据及baseline发布：4月10日
   3.	测试A数据发布：4月11日
   4.	测试A评测截止：5月29日 晚上11：00
   5.	测试B数据发布：5月31日
   6.	测试B最终测试结果：6月2日 晚上11：00
   7.	公布测试结果：6月10日前
   8.	提交中文或英文技术报告：2023年6月20日
   9.	中文或英文技术报告反馈：2023年6月28日
   10.	正式提交中英文评测论文：7月3日
   11.	公布获奖名单：2023年7月７日
   12.	评测报告及颁奖：8月3-5日


# 资助情况
   &emsp;&emsp;本次评测将评选出如下奖项。  
   &emsp;&emsp;由中国中文信息学会计算语言学专委会（CIPS-CL）为获奖队伍提供荣誉证书。
   |奖项|一等奖|二等奖|三等奖|
   |----|----|----|----|
   |数量|1名|待定| 待定 |
   |奖励|荣誉证书|荣誉证书|荣誉证书|

  
# 数据集协议

&emsp;&emsp;该数据集遵循协议： [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/?spm=5176.12282016.0.0.7a0a1517bGbbHL)。

# 引用

   &emsp;&emsp;由于版权保护问题，CFN数据集只免费提供给用户用于非盈利性科学研究使用，参赛人员不得将数据用于任何商业用途。如果用于商业产品，请联系谭红叶老师，联系邮箱 tanhongye@sxu.edu.cn。

# 评测网址
   评测网址：[https://tianchi.aliyun.com/CFSP](https://tianchi.aliyun.com/CFSP)  
   GitHub：[https://github.com/SXUNLP/Chinese-Frame-Semantic-Parsing](https://github.com/SXUNLP/Chinese-Frame-Semantic-Parsing)


# 注意事项

1.	由于版权保护问题，CFN数据集只免费提供给用户用于非盈利性科学研究使用，参赛人员不得将数据用于任何商业用途。如果用于商业产品，请联系柴清华老师，联系邮箱 charles@sxu.edu.cn。 
2.	每名参赛选手只能参加一支队伍，一旦发现某选手以注册多个账号的方式参加多支队伍，将取消相关队伍的参赛资格。
3.	数据集的具体内容、范围、规模及格式以最终发布的真实数据集为准。针对测试集，参赛人员不允许执行任何人工标注。
4.	参赛队伍可在参赛期间随时上传测试集的预测结果，阿里天池平台每天可提交3次，系统会实时更新当前最新榜单排名情况，严禁参赛团队注册其它账号多次提交。
5.	允许使用公开的代码、工具、外部数据（从其他渠道获得的标注数据）等，但需要保证参赛结果可以复现。
6.	参赛队伍可以自行设计和调整模型，但需注意模型参数量最多不超过1.5倍BertLarge（510M）。
7.	算法与系统的知识产权归参赛队伍所有。要求最终结果排名前10的队伍提供算法代码与系统报告（包括方法说明、数据处理、参考文献和使用开源工具、外部数据等信息）。提交完毕将采用随机交叉检查的方法对各个队伍提交的模型进行检验，如果在排行榜上的结果无法复现，将取消获奖资格。
8.	参赛团队需保证提交作品的合规性，若出现下列或其他重大违规的情况，将取消参赛团队的参赛资格和成绩，获奖团队名单依次顺延。重大违规情况如下：  
 a. 使用小号、串通、剽窃他人代码等涉嫌违规、作弊行为；  
 b. 团队提交的材料内容不完整，或提交任何虚假信息；  
 c. 参赛团队无法就作品疑议进行足够信服的解释说明；  
9.	评测单位：山西大学、北京大学、南京大学。  
10.	评测负责人：谭红叶 tanhongye@sxu.edu.cn；联系人：闫智超 202022408073@email.sxu.edu.cn、李俊材 202122407024@email.sxu.edu.cn




# **参考文献**
[1] Fillmore, C & Atkins, B. Toward a Frame-based Lexicon: The Semantics of RISK and its Neighbors[A]. Frames, Fields and Contrasts: New Essays in Semantic and Lexical Organization[C]. Hillsdale: Erlbaum, 1992.
[2] Daniel Gildea and Daniel Jurafsky. 2002. Automatic labeling of semantic roles. Computational linguistics,28(3):245–288.  
[3] Shaoru Guo, Ru Li*, Hongye Tan, Xiaoli Li, Yong Guan. A Frame-based Sentence Representation for Machine Reading Comprehension[C]. Proceedings of the 58th Annual Meeting of the Association for Computational Linguistic (ACL), 2020: 891-896.  
[4] Shaoru Guo, Yong Guan, Ru Li*, Xiaoli Li, Hongye Tan. Incorporating Syntax and Frame Semantics in Neural Network for Machine Reading Comprehension[C]. Proceedings of the 28th International Conference on Computational Linguistics (COLING), 2020: 2635-2641.  
[5] Yong Guan, Shaoru Guo, Ru Li*, Xiaoli Li, and Hu Zhang. Integrating Semantic Scenario and Word Relations for Abstractive Sentence Summarization[C]. Proceedings of the 2021 Conference on Empirical Methods in Natural Language Processing (EMNLP) 2021: 2522-2529.  
[6] Yong Guan, Shaoru Guo, Ru Li*, Xiaoli Li, and Hongye Tan, 2021. Frame Semantic-Enhanced Sentence Modeling for Sentence-level Extractive Text Summarization[C]. Proceedings of the 2021 Conference on Empirical Methods in Natural Language Processing (EMNLP) 2021: 404-4052.  
[7] Hongyan Zhao, Ru Li*, Xiaoli Li, Hongye Tan. CFSRE: Context-aware based on frame-semantics for distantly supervised relation extraction[J]. Knowledge-Based Systems, 2020, 210: 106480.  
[8] Bishan Yang and Tom Mitchell. 2017. A joint sequential and relational model for frame-semantic parsing. In Proceedings ofthe Conference on Empirical Methods in Natural Language Processing. 1247–1256.  
[9] Swabha Swayamdipta, Sam Thomson, Chris Dyer, and Noah A. Smith. 2017. Frame-semantic parsing with softmax-margin segmental RNNs and a syntactic scaffold. CoRR abs/1706.09528 (2017).  
[10] Xudong Chen, Ce Zheng, and Baobao Chang. 2021. Joint multi-decoder framework with hierarchical pointer network for frame semantic parsing. In Findings of the Association forComputational Linguistics. Association for Computational Linguistics, 2570–2578.   
[11] Hao Peng, Sam Thomson, Swabha Swayamdipta, and Noah A. Smith. 2018. Learning joint semantic parsers from disjoint data. In Proceedings of the Conference of the North American Chapter of the Association for Computational Linguistics: Human Language Technologies. Association for Computational Linguistics, New Orleans, 1492–1502.   
[12] Su X., Li R.*, Li X., et al. A knowledge-guided framework for frame identification[C]. Proceedings of the 59th Annual Meeting of the Association for Computational Linguistic (ACL), 2021, pp.5230–5240.  
[13] Ce Zheng, Xudong Chen, Runxin Xu, Baobao Chang. A Double-Graph Based Framework for Frame Semantic Parsing. NAACL-2022.  
[14] Ce Zheng ,Yiming Wang, Baobao Changi, Query Your Model with Definitions in FrameNet: An Effective Method for Frame Semantic Role Labeling. AAAI-2023.  


