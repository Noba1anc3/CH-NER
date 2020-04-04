# 使用BiLSTM+CRF实现中文命名实体识别  
![Image text](https://img.shields.io/badge/Version-v1.0.0-lightgrey)
- **[设计文档](https://github.com/Noba1anc3/CH-NER/wiki/%E8%AE%BE%E8%AE%A1%E6%96%87%E6%A1%A3)**
- **[数据集 & 评估标准](https://github.com/Noba1anc3/CH-NER/wiki/%E6%95%B0%E6%8D%AE%E9%9B%86-&-%E8%AF%84%E4%BC%B0%E6%A0%87%E5%87%86)**
- **[评估结果分析](https://github.com/Noba1anc3/CH-NER/wiki/%E8%AF%84%E4%BC%B0%E7%BB%93%E6%9E%9C%E5%88%86%E6%9E%90)**
- **[清洗工具说明文档](https://github.com/Noba1anc3/CH-NER/wiki/%E6%B8%85%E6%B4%97%E5%B7%A5%E5%85%B7%E8%AF%B4%E6%98%8E%E6%96%87%E6%A1%A3)**

## 环境要求  
![Image text](https://img.shields.io/badge/Python-3.6-green?style=flat)
![Image text](https://img.shields.io/badge/Tensorflow-≥1.14.1-green?style=flat) 

## 模型
模型结构如下图所示：
![Image text](https://tva1.sinaimg.cn/large/00831rSTgy1gd7be7ohmzj30ue0u0ttr.jpg)

对于一个中文句子，这个句子中的每个字符都有一个属于集合 **{O，B-PER，I-PER，B-LOC，I-LOC，B-ORG，I-ORG}** 的标记。  
第一层，**look-up**层，旨在将每个字符表示从一个独热向量转换为字符嵌入。  
第二层，**Bi-LSTM**层，可以有效地利用过去和将来的输入信息，自动提取特征。  
第三层，**CRF**层，在一个句子中为每个字符标记标签。如果使用**Softmax**进行标记，由于**Softmax**层独立地标记每个位置，可能会得到非随机标记序列。例如:“I-LOC”不能跟在“B-PER”后面，但**Softmax**并不清楚。与**Softmax**相比，**CRF**层可以利用句子级的标签信息，对两个不同标签的转换行为进行建模。

## 数据集
数据集使用的是清理后的**MSRA**数据集，**WiKi**数据集，以及利用百度**LAC**标注工具标注的**LAC**数据集。以下是数据集展示
![Image text](https://tva1.sinaimg.cn/large/00831rSTgy1gd7bhaap8mj30zc06mq51.jpg)
**数据集：https://bhpan.buaa.edu.cn:443/link/0229CC6A5311D913AF03758FA10F96B3**

### 数据文件
目录`.data/`下包括
+ 预处理好的数据：`train_data`、`test_data`、`dev_data`
+ 一个词汇表文件`word2id`，它将每个字符映射到一个唯一的id
生成词汇表文件，请参考`data_process.py`中的代码。

### 数据形式
每个数据文件应采用以下格式：
![Image text](https://tva1.sinaimg.cn/large/00831rSTgy1gd7bmeltmfj31dw0g00tl.jpg)

如果想要使用自己的数据集，你需要：
+ 把你的语料库转换成上面的格式
+ 重新生成一个词汇表文件  

## 模型参数
相关模型参数存储在`config`目录下的配置文件中。  

## 运行  

### 前端展示  
```python
python3 run.py
```

### train  
```python
python3 main.py --mode=train
```

### test  
```python
python3 main.py --mode=test
```

### predict  
```python
python3 main.py --mode=predict
```

## 测试结果  

下图是测试集在本项目模型和百度LAC模型中的测试效果对比，其中数据集按照**8：1：1**划分为训练集、测试集、验证集。
![Image text](https://tva1.sinaimg.cn/large/00831rSTgy1gd7bn820ljj30xg05stdf.jpg)

为了进一步验证模型的鲁棒性，标注了法院文本数据集进行进一步的测试

**法院文本数据集链接：https://bhpan.buaa.edu.cn:443/link/1E091A8ACC627E01DD83D20C67435F06**

测试结果：
![Image text](https://tva1.sinaimg.cn/large/00831rSTgy1gd7bp04bk9j30y005cq7q.jpg)


## 相关文献
+ [Bidirectional LSTM-CRF Models for Sequence Tagging](https://arxiv.org/pdf/1508.01991v1.pdf)
+ [Neural Architectures for Named Entity Recognition](https://www.aclweb.org/anthology/N16-1030/)
+ [Character-Based LSTM-CRF with Radical-Level Features for Chinese Named Entity Recognition](https://link.springer.com/chapter/10.1007/978-3-319-50496-4_20)
+ [https://github.com/guillaumegenthial/sequence_tagging](https://github.com/guillaumegenthial/sequence_tagging)
