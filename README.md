# Lawyer LLaMA

通过指令微调，Llama 模型在通用领域展现出了非常好的表现。但由于缺少合适的数据，少有人探究Llama在法律领域的能力。为了弥补这一空白，我们提出了Lawyer LLaMA，一个在法律领域数据上进行了额外训练的模型。

Lawyer LLaMA 首先在大规模法律语料上进行了continual pretraining，让它系统的学习中国的法律知识体系。
在此基础上，我们借助GPT 3.5收集了一批对中国法律职业资格考试题目的分析和对法律咨询的回答，利用收集到的数据对模型进行指令微调，让模型习得将法律知识应用到具体场景中的能力。

我们的模型能够：

1. **掌握中国法律理论：** 能够正确的理解民法、刑法、行政法、诉讼法等常见领域的法律概念。例如，掌握了刑法中的犯罪构成理论，能够从刑事案件的事实描述中准确的识别犯罪主体、犯罪客体、犯罪行为、主观心理状态等犯罪构成要件。模型利用学到的法律概念与理论，能够很好回答中国法律职业资格考试中的大部分题目。

2. **应用于中国法律实务**：能够以通俗易懂的语言解释法律概念，并且进行基础的法律咨询，涵盖婚姻、借贷、合同、刑事等法律领域。

为了给中文法律大模型的的开放研究添砖加瓦，本项目将开源一系列**法律领域的指令微调数据** 和**基于LLaMA训练的中文法律大模型** 。


# 新闻

[2023/4/13] 开源了由GPT 3.5生成的指令微调数据，包含2k条中国法考题解答及5k条法律咨询回复。

# 更新计划

[ ] 开源更大规模的指令微调数据

[ ] 开源法律咨询多轮对话数据

[ ] 开源通用法律领域的预训练模型

[ ] 开源特定法律领域的咨询模型

# 训练数据

## 法律领域纯文本语料

我们收集了部分公开的法律数据用于模型的continual training，包括法条，裁判文书等。

## 通用指令微调数据

1. [Alpaca](https://github.com/tatsu-lab/stanford_alpaca) 52k 英文。

2. [BELLE](https://github.com/LianjiaTech/BELLE) 52K 中文。

BELLE提供了2M的中文指令微调数据，我们从中随机采样了52K数据，和Alpaca混合后，用于通用领域的指令微调。

## 法律指令微调数据

### 法律职业资格考试
我们将[JEC-QA法考数据集](https://jecqa.thunlp.org/)中的试题输入ChatGPT，让其生成对每个答案的解析。

    **Instruction:**

    根据《物权法》，下列关于物权变动的特别规定，表述正确吗？

    因继承或者受遗赠取得物权的，自继承或者受遗赠开始时发生效力

    请给出详细的推理过程之后再给出答案。

    **Output：**

    这个表述是正确的。

    根据《物权法》第X条规定：“因继承或者受遗赠取得物权的，自继承或者受遗赠开始时发生效力。”这意味着，当一个人通过继承或受遗赠获得物权时，这个物权的效力是从继承或受遗赠开始时就生效的，而不是从物权转移的时间开始生效。这是因为，继承或受遗赠是一种法定的方式来获得物权，因此，物权的效力应该从这个法定的时间点开始生效。

### 法律咨询
我们从[开放数据集](https://opendata.pku.edu.cn/dataset.xhtml?persistentId=doi:10.18170/DVN/OLO4G8)中收集了一些咨询问题，输入ChatGPT，让ChatGPT扮演律师解答问题。

    **Instruction:**

    是这么个回事，关于我儿子今年2月27日背着我们大人买了一台轻便女式摩托车麻烦你帮忙看怎么解决？

    **Output:**

    首先，根据法律规定，未满18岁的未成年人是无法独立签订合同的，因此你的儿子在未经你的同意的情况下购买摩托车是无效的。

    其次，根据《中华人民共和国道路交通安全法》的规定，未满18周岁的人不得驾驶摩托车，因此你的儿子也无权驾驶这辆摩托车。

    建议你尽快联系卖家，说明情况并要求退货。如果卖家不同意，你可以向消费者协会或相关部门投诉，寻求法律帮助。同时，你也需要对你的儿子进行教育，让他了解道路交通安全法规，避免再次发生类似的情况。

# 模型参数

在符合LLaMA使用条款的情况下，我们将于近期公开基于LLaMA 7B训练的中文法律模型。

# **系统效果**

我们选取了LLaMA 7B作为基座模型，使用法律领域语料进一步预训练，并在通用领域和法律领域指令微调。

以下对比了该模型和ChatGPT对于部分输入样例的回复：

## 法律概念理解

**Legal Concept:** 委托代理

| **Lawyer LLaMA (7B)**                                                                                                           | **ChatGPT (175B)**                                                                                                                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 委托代理是指委托人将自己的权利和义务委托给代理人，代理人代表委托人进行某项法律行为。委托代理是一种法律关系，代理人必须按照委托人的要求行事，并向委托人报告进展情况。委托代理的法律效力与委托人的权利和义务相同，代理人必须遵守法律法规，保护委托人的合法权益。 | 委托代理是指一方（即委托人）授权另一方（即代理人）代表其执行特定的法律行为或采取特定的法律行动。委托代理通常涉及商业或法律事务，例如买卖房屋或签订合同等。在这种情况下，委托人必须给代理人明确的授权，以便代理人能够代表委托人采取行动。<br/>委托代理可以是口头协议或书面协议，具体取决于委托人和代理人之间的协议。委托代理需要遵守一定的法律规定，例如代理人必须履行忠实、诚信的义务，不能利用委托代理行为牟取不当利益或泄露委托人的保密信息等。<br/>委托代理在商业和法律事务中很常见，因为它可以让委托人将特定的任务交给代理人去完成，从而减轻自己的负担和节约时间和资源。同时，它也可以为代理人提供商业机会，例如代表多个客户进行买卖房屋或签订合同等。 |

<!-- **下面放更多样例，默认折叠，可展开。或者单独放到另一个markdown里？** -->

## 刑事案件中的犯罪构成理论 -- 四要件

**过失致人死亡罪:** 请抽取以下案例中的犯罪构成要件 ‘绩溪县人民检察院起诉书指控：2013年11月1日8时40分许，被告人曹某某驾驶皖P54611自卸货车行至家朋乡汪店村阳戈溪村河边装石块，被告人曹某某将货车停放于一斜坡处，拉上手刹下即下车去驾驶铲车往货车内装石块，约二十分钟后货车溜车下滑，滑行中撞上在坡底河边洗衣服的洪某某，当日下午，洪某某经抢救无效死亡。经鉴定，洪某某系因事故致创伤性失血性休克死亡。’

| **Lawyer LLaMA (7B)**                                                                                                                                                                                                                     | **ChatGPT (175B)**                                                                                                                                                                                      |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 犯罪构成要件：<br/>1. 犯罪主体：被告人曹某某。<br/>2. 犯罪行为：驾驶货车在卸货行驶过程中，将货车停放于一斜坡处，拉上手刹下即下车去驾驶铲车往货车内装石块，约二十分钟后货车溜车下滑，滑行中撞上在坡底河边洗衣服的洪某某，当日下午，洪某某经抢救无效死亡。<br/>3. 结果：洪某某死亡。<br/>4. 主观方面：被告人曹某某的行为是有意识的，即他知道自己的行为存在危险，但仍然故意滑行，撞上了洪某某。<br/>综上所述，被告人曹某某的行为构成过失杀人罪。 | 这是一起交通肇事案件，犯罪构成要件包括：<br/>肇事：被告人曹某某驾驶自卸货车行至家朋乡汪店村阳戈溪村河边装石块，将货车停放于一斜坡处，拉上手刹下即下车去驾驶铲车往货车内装石块，货车因此失去控制溜车下滑，最终撞上在坡底河边洗衣服的洪某某。<br/>重大事故：该起交通事故导致洪某某死亡，经鉴定，洪某某系因事故致创伤性失血性休克死亡。<br/>因此，被告人曹某某构成交通肇事罪，属于过失致人死亡罪。 |



## 法考

**Question:** 假设甲国为《保护文学艺术作品伯尔尼公约》的成员国，乙国为非成员国。依该公约的规定，下列作品可以享有国民待遇?
**Gold Answer:** A B C

**A** 甲国公民在甲国和乙国同时出版的文学作品

| **Lawyer LLaMA (7B)**                                                                                                                                                                                                                                     | **ChatGPT (175B)**                                                                                                                                                                                        |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 根据《保护文学艺术作品伯尔尼公约》的规定，成员国应该给予其他成员国的公民和非成员国的公民在成员国境内创作的文学艺术作品国民待遇。也就是说，如果一个人在甲国创作了一部文学作品，那么他在乙国出版该作品也应该享有甲国的版权保护。<br/>但是，如果该作品的作者是乙国的公民，那么他不属于甲国的版权保护范围。因此，如果甲国的公民在甲国和乙国同时出版了文学作品，那么该作品只能享有甲国的版权保护，而不能享有乙国的版权保护。<br/>因此，答案是：甲国公民在甲国和乙国同时出版的文学作品可以享有甲国的国民待遇。 | 根据《保护文学艺术作品伯尔尼公约》的规定，成员国应该对其他成员国的公民在该公约规定保护范围内的作品享有国民待遇。但对于非成员国的作品，不一定能享有国民待遇。<br/>根据题目的描述，甲国是《保护文学艺术作品伯尔尼公约》的成员国，而乙国是非成员国。因此，甲国公民在甲国和乙国同时出版的文学作品应该能够在甲国享有国民待遇，但在乙国不一定能够享有国民待遇。<br/>总之，该选项的判断是“部分正确，部分错误” |

**B** 乙国公民首先在甲国出版的文学作品

| **Lawyer LLaMA (7B)**                                                                                                                                                             | **ChatGPT (175B)**                                                                                                                                                                         |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 根据《保护文学艺术作品伯尔尼公约》的规定，成员国应该给予其他成员国的公民和非成员国的公民在成员国境内创作的文学艺术作品国民待遇。这意味着，如果乙国公民在甲国创作了一部文学作品，那么该作品应该受到甲国的保护，并且甲国应该给予该作品国民待遇，即与甲国公民的作品享有同等的保护。<br/>因此，如果乙国公民在甲国出版了一部文学作品，那么该作品可以享有国民待遇。 | 根据《保护文学艺术作品伯尔尼公约》的规定，成员国应该给予其他成员国的作者和作品同等的保护待遇，即国民待遇原则。这意味着，如果一个国家的作品在另一个国家出版或发行，它应该得到与后者本国作品相同的保护待遇。<br/>然而，该公约并未规定非成员国的作品可以享有国民待遇。因此，根据伯尔尼公约的规定，乙国公民首先在甲国出版的文学作品不能享有国民待遇。<br/>因此，选项是错误的。 |

**C** 在甲国有住所的乙国公民的文学作品

| **Lawyer LLaMA (7B)**                                                                                                                                                                                                                                     | **ChatGPT (175B)**                                                                                                                                                                                                                                            |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 根据《保护文学艺术作品伯尔尼公约》的规定，成员国应该给予其他成员国的公民和非成员国的公民在成员国境内创作的文学艺术作品国民待遇。这意味着，如果乙国公民在甲国创作了一部文学作品，那么该作品应该受到甲国的保护，并享有与甲国公民的作品相同的待遇。<br/>因此，如果乙国公民在甲国有住所，那么他的在甲国创作的文学作品可以享有国民待遇。这是因为，乙国公民在甲国有住所，就像甲国公民在乙国有住所一样，他应该享有与甲国公民相同的权利和待遇。<br/>因此，答案是：乙国公民在甲国创作的文学作品可以享有国民待遇。 | 根据《保护文学艺术作品伯尔尼公约》的规定，其成员国应当向其他缔约国公民的作品，给予与本国公民作品同等的国民待遇。因此，在甲国中，如果有乙国公民居住并创作了文学作品，那么该作品应该可以享有甲国的国民待遇，即与甲国公民创作的作品受到相同的保护。<br/>然而，乙国并非该公约的成员国，因此乙国公民在自己的国家创作的作品可能无法得到与甲国公民作品相同的保护。此外，甲国可能还有其他国内法规定，限制外国公民的著作权保护范围，需要具体情况具体分析。<br/>因此，对于该选项的判断为：可能正确，但需要具体情况具体分析 |

**D** 乙国公民在乙国发表的文学作品

| **Lawyer LLaMA (7B)**                                                                                                                                                                                                                                                                                                  | **ChatGPT (175B)**                                                                                                                                                                  |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 根据《保护文学艺术作品伯尔尼公约》的规定，成员国应该给予其他成员国的公民和非成员国的公民在成员国境内发表的文学艺术作品国民待遇。这意味着，如果乙国公民在乙国发表了文学作品，那么该作品可以享有与乙国公民的作品相同的保护，包括获得版权和受到财产权利的保护。<br/>而对于乙国公民在乙国发表的文学作品，则不具备国民待遇，因为乙国不是《保护文学艺术作品伯尔尼公约》的成员国。但是，如果乙国公民在乙国发表的文学作品在乙国被认定为优先申请，那么该作品可以在乙国获得版权和财产权利保护。<br/>因此，答案是：乙国公民在乙国发表的文学作品不可以享有国民待遇，但如果该作品在乙国被认定为优先申请，那么可以在乙国获得版权和财产权利保护。 | 根据《保护文学艺术作品伯尔尼公约》的规定，成员国应当给予其他成员国的著作权人及其作品同自己国民的待遇。因此，甲国应该给予乙国公民在甲国发表的文学作品同自己国民的待遇。但是，乙国公民在乙国发表的文学作品是否能够享有国民待遇，则取决于乙国是否加入了该公约。<br/>因为题目中指出乙国为非成员国，因此该国的公民在该公约下并没有享有国民待遇的权利。所以，选项错误。 |



## 咨询

**Question:** 男婚内出轨，怎样告女方？？？？？？？

| **Lawyer LLaMA (7B)**                                                                                                                                                                                                                                                                                                   | **ChatGPT (175B)**                                                                                                                                                                                                                                                                                                                                                                                         |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 如果男方在婚姻关系中出轨，女方可以通过以下方式来表达自己的不满和愤怒：<br/>1. 与男方沟通：女方可以与男方沟通，表达自己的不满和愤怒，并要求男方停止出轨行为。<br/>2. 寻求婚姻咨询：女方可以寻求专业的婚姻咨询师或心理医生的帮助，以了解如何处理自己的情绪和如何保护自己的权益。<br/>3. 寻求法律援助：女方可以寻求法律援助，了解自己的权利和法律程序，以便采取适当的行动来保护自己的权益。<br/>4. 提起离婚：如果男方的出轨行为已经严重影响了婚姻关系，女方可以考虑提起离婚。在离婚过程中，女方可以要求分割财产和抚养子女的权利。<br/>无论采取哪种行动，女方都应该保持冷静和理智，以便更好地保护自己的权益。 | 作为律师，我必须提醒您，婚内出轨是一项敏感的问题，需要认真考虑和权衡。如果您想要采取法律行动，以下是一些可能有用的信息：<br/>1. 确定出轨是否构成违法行为：在某些国家或地区，婚内出轨可能被视为违法行为，可能构成通奸罪或与婚姻法相关的罪行。您应该检查您所在地区的相关法律，以确定出轨是否构成违法行为。<br/>2. 收集证据：如果出轨构成违法行为，您需要收集证据来支持您的指控。这可能包括获取出轨方的通讯记录、照片、视频等物证，或者聘请私人侦探来获取证据。<br/>3. 寻求法律援助：如果您打算采取法律行动，最好咨询一位有经验的律师。律师可以帮助您理解相关的法律，并提供建议和帮助。<br/>请注意，采取法律行动可能会对您的家庭关系产生负面影响。在采取任何行动之前，请考虑一下这是否值得。您可能需要与配偶进行对话，并尝试解决问题，或者寻求婚姻咨询等其他形式的帮助来修复您的婚姻关系。 |



# 致谢

本项目的开放过程中，获得了以下项目的帮助，在此表示感谢。

https://jecqa.thunlp.org/

https://opendata.pku.edu.cn/dataset.xhtml?persistentId=doi:10.18170/DVN/OLO4G8

https://github.com/tatsu-lab/stanford_alpaca

https://github.com/LianjiaTech/BELLE



本项目贡献人员：

黄曲哲*，陶铭绪*，安震威*，张晨*，姜聪，陈智斌，伍子睿，冯岩松

\* Equal Contribution

本项目是在冯岩松教授的指导下进行的。

# **局限性和使用限制**

本项目内容仅供用于学术研究，不得用于商业以及其他会对社会带来危害的用途。使用涉及第三方代码的部分时，请严格遵循相应的开源协议。

本项目中使用的数据由GPT3.5生成，未经严格验证，可能会存在错误内容，在使用时请注意甄别。

本项目中的模型输出并非专业法律咨询结果，可能会包含错误内容。如需法律援助，请从专业人士处获得帮助。

# 引用

如果您使用了本项目的内容，或者认为本项目对您的研究有帮助，请引用本项目。

```Plaintext
@misc{Lawyer-LLama,
  author = {Quzhe Huang, Mingxu Tao, Zhenwei An, Chen Zhang, Cong Jiang, Zhibing Chen, Zirui Wu, Yansong Feng},
  title = {Lawyer Llama},
  year = {2023},
  publisher = {GitHub},
  journal = {GitHub repository},
  howpublished = {\url{https://github.com/AndrewZhe/lawyer-llama}},
}
```

