**问题**

将passage做ranking后送入Reader后，相比baseline，bleu-1，2，3，4都有明显提升，但rouge-l却下降了

**猜想**

数据分布不一致：
由于先前训练Ranker时没有及逆行cross-validation，所以相当于部分随后ranking并被送入Reader作为训练的数据，实际参与过Ranker的训练，
因此这些数据的ranking结果往往偏好，即有更多的‘is_selected’ passages准确出现在top-2 passages中。
然而dev dataset中的数据完全没有参与过Ranker的训练，则有较少的‘is_selected’ passages 出现在top-2 passages中
采用前者训练，后者评测，所以rouge-l较低

**验证**

对training dataset做cross-validation：采用后50%数据训练Ranker，将前50%数据forward做ranking。
同时也用这一训练好的Ranker再给后50%的数据和dev dataset训练。
统计在这三种情况中，ranked top-2 passages有多少篇为真的‘is_selected’
全部top-2 passages数目为All，其中‘is_selected’ passages数目为Hit，

*precision = Hit/All*

结果如下：

**CKNRM**

Data: train

后50%训练 前50%forward：

前50%：

Hit: 136820, All: 808732, precision: 0.16917841757219945

后50%：

Hit: 162695, All: 808730, precision: **0.2011734447837968**

Data:Dev

Hit: 32592, All: 202186, precision: 0.16119810471546003

KNRM

Data: train

后50%训练，前50%forward：

前50%：

Hit: 121247, All: 808732, precision: 0.1499223475762057

后50%：

Hit: 138170, All: 808730, precision: **0.17084811989168203**

Data: Dev

Hit: 28584, All: 202186, precision: 0.14137477372320537

**结果**

确实，没有参与训练的那部分train data的precision和dev data的基本一致，
而那部分参与了Ranker训练的数据的precision要高3%~4%
