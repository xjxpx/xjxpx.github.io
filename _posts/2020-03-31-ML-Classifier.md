---
title:      分类器常用的评价指标
date:       2020-03-31
author:     XPX
header-img: img/post-bg-desk.jpg
catalog: true
tags:
    - 机器学习
    - 评价指标
---
> [我的博客](http://xuepeixin.github.io)

分类器常用的评价指标有：

1. Accuracy
2. Precision
3. Recall
4. F1 Score
5. ROC Curve
6. PR Curve
7. AUC

### 混淆矩阵
在详细介绍指标之前，我们先了解二元分类器的混淆矩阵

||Positive|Negative|
|----|----|----|
|True|True Positive(TP)|True Negative(TN)|
|False|False Positive(FP)|False Negative(FN)|

以下几种组合分别表示：

- 真正例（True Positive，TP）：真实类别为正例，预测类别为正例。
- 假正例（False Positive，FP）：真实类别为负例，预测类别为正例。
- 假负例（False Negative，FN）：真实类别为正例，预测类别为负例。
- 真负例（True Negative，TN）：真实类别为负例，预测类别为负例。

接下来开始介绍各种指标

### 准确率（Accuracy）

$$ACC = \frac{TP+TN}{TP+TN+FP+FN}$$

准确率是我们最常见的评价指标，而且很容易理解，就是被分对的样本数除以所有的样本数，通常来说，正确率越高，分类器越好。

*准确率确实是一个很好很直观的评价指标，但是有时候准确率高并不能代表一个算法就好。比如某个地区某天地震的预测，假设我们有一堆的特征作为地震分类的属性，类别只有两个：0：不发生地震、1：发生地震。一个不加思考的分类器，对每一个测试用例都将类别划分为0，那那么它就可能达到99%的准确率，但真的地震来临时，这个分类器毫无察觉，这个分类带来的损失是巨大的。为什么99%的准确率的分类器却不是我们想要的，因为这里数据分布不均衡，类别1的数据太少，完全错分类别1依然可以达到很高的准确率却忽视了我们关注的东西。再举个例子说明下。在正负样本不平衡的情况下，准确率这个评价指标有很大的缺陷。比如在互联网广告里面，点击的数量是很少的，一般只有千分之几，如果用acc，即使全部预测成负类（不点击）acc也有 99% 以上，没有意义。因此，单纯靠准确率来评价一个算法模型是远远不够科学全面的。*

### 错误率（Error rate）

$$error~rate = \frac{FP+FN}{TP+TN+FP+FN}$$

错误率则与准确率相反，描述被分类器错分的比例, accuracy =1 - error rate。

### 灵敏度、召回率（Sensitive、Recall）

$$Sensitive = \frac{TP}{TP+FN}$$

也称为真阳性率、召回率(Recall)，表示的是所有正例中被分对的比例，衡量了分类器对正例的识别能力。

### 特异度（Specificity）

$$Specificity = \frac{TN}{TN+FP}$$
也称为真阴性率，表示的是所有负例中被分对的比例，衡量了分类器对负例的识别能力。

### 精确率（Precision）

$$Precision = \frac{TP}{TP+FP}$$

表示被分为正例的示例中实际为正例的比例。

### 综合评价指标（F-Measure）

*灵敏度或者召回率（Sensitive、Recall）与精确率（Precision）之间是矛盾的，比如对于正负样例都占一半的样本，我们的算法能够对每个样本计算出其为正例的概率，例如我们可以将阈值设置为0.2做到召回率为100%，即在将概率大于0.2的样本分类为正类后，所有正例都被分类正确，但前提是精确率为62.5%，即有37.5%的负例将被分类为正例。同时，我们的算法可以通过提高概率阈值的方式提高精确率，即减少负例被错误分类的数量，比如精确率可以提升到83.3%，但由于算法性能的限制，可能会出现将概率低于阈值正例分类为负，从而召回率可能会下降。*

这样就需要综合考虑他们，最常见的方法就是F-Measure（又称为F-Score）。

$$F=\frac{(\alpha^2+1)Precision*Recall}{\alpha^2(Precision+Recall)}$$

当参数α=1时，就是最常见的F1，也即
$$F1=\frac{2*Precision*Recall}{Precision+Recall}$$

### ROC曲线

横坐标：1-Specificity，伪正类率(False positive rate， FPR)，预测为正但实际为负的样本占所有负例样本的比例；
纵坐标：Sensitivity，真正类率(True positive rate， TPR)，预测为正且实际为正的样本占所有正例样本的比例。

<center>
    <img
    src="https://xpx-picbed.oss-cn-beijing.aliyuncs.com/blog/2020/03/评价指标AUC.png">
</center>

ROC 曲线有以下特性：

1. 曲线与FP_rate轴围成的面积（记作AUC）越大，说明性能越好，即图上L2曲线对应的性能优于曲线L1对应的性能。即：曲线越靠近A点（左上方）性能越好，曲线越靠近B点（右下方）曲线性能越差。
2. A点是最完美的performance点，B处是性能最差点。
3. 位于C-D线上的点说明算法性能和random猜测是一样的–如C、D、E点。位于C-D之上（即曲线位于白色的三角形内）说明算法性能优于随机猜测–如G点，位于C-D之下（即曲线位于灰色的三角形内）说明算法性能差于随机猜测–如F点。
4. 虽然ROC曲线相比较于Precision和Recall等衡量指标更加合理，但是其在高不平衡数据条件下的的表现仍然过于理想，不能够很好的展示实际情况。

### PR(Precision Recall)曲线

PR曲线展示的是Precision vs Recall的曲线，PR曲线与ROC曲线的相同点是都采用了TPR (Recall)，都可以用AUC来衡量分类器的效果。不同点是ROC曲线使用了FPR，而PR曲线使用了Precision，因此PR曲线的两个指标都聚焦于正例。类别不平衡问题中由于主要关心正例，所以在此情况下PR曲线被广泛认为优于ROC曲线。

<center>
    <img
    src="https://xpx-picbed.oss-cn-beijing.aliyuncs.com/blog/2020/03/评价指标PR曲线.jpg">
</center>

1. ROC曲线由于兼顾正例与负例，所以适用于评估分类器的整体性能，相比而言PR曲线完全聚焦于正例。
2. 如果有多份数据且存在不同的类别分布，比如信用卡欺诈问题中每个月正例和负例的比例可能都不相同，这时候如果只想单纯地比较分类器的性能且剔除类别分布改变的影响，则ROC曲线比较适合，因为类别分布改变可能使得PR曲线发生变化时好时坏，这种时候难以进行模型比较；反之，如果想测试不同类别分布下对分类器的性能的影响，则PR曲线比较适合。
3. 如果想要评估在相同的类别分布下正例的预测情况，则宜选PR曲线。
类别不平衡问题中，ROC曲线通常会给出一个乐观的效果估计，所以大部分时候还是PR曲线更好。
4. 最后可以根据具体的应用，在曲线上找到最优的点，得到相对应的precision，recall，f1 score等指标，去调整模型的阈值，从而得到一个符合具体应用的模型。