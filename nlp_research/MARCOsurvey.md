# MARCO调研

| 统计项目 | dev | train |
| --------  | -----:  | :----: |
| total number of queries: |  101093 | 808731 |
| ground truth为no answer present的query数:  | **45457** |　**305361**　
|　全部文本label为0的query数:  |　86721 |　628039　|
| no answer present 且全label==0： | **45457** |　**305361**　|
| Gound Truth 在 label为1的文本中的query数：|  14372 | 180692 |
| Gound Truth 在 label为0的文本中的query数：|  584 | 7717 |
| Gound Truth 不在文本中的query数： | **86137**  | **620322** |
| Gound Truth 不在文本中且全部文本label=0：|  **86137** | **620322** |
| Gound Truth 不在文本中且no answer present： | 45457 | 305361 |
| Gound Truth 不在文本中&&label==0&&no answer present：| 45457 | 305361 |

**发现：**

---》所有gounrd truth 为 no answer present的都没有label 1

---》Gound Truth 不在文本中时，全部文本一律为label 0

---》No answer present的情况仅占ground truth不在文本中的情况的一半

---》对于no answer present的情形，肯定所有文章is selected都是0，有相当一部分比例的数据是no answer present

---》对于ground truth在文中能找到的情形，只有极小一部分在is selected为0的passage中

---》对于ground truth在文中找不到的情形，所有passage的is selected 一定为0
