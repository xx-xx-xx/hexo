---
title: i-vector 对说话人确认的前置因子分析
date: 2021-08-05 20:15:29
categories: PaperReading
tags: SpeakerRecognition
---

身份向量identity vector

总体变化空间total variability space：定义了一个新的低维的依赖于说话人和信道的空间

两个说话人确认系统

- 基于支持向量机SVM，用余弦核来估计输入数据之间的相似性
- 直接用余弦相似度作为最终决策得分

三种信道补偿技术

- 类内协方差正则化 WCCN
- 线性判别分析 LDA
  - 最大化说话人之间的差异，最小化说话人内部的差异
- 扰动属性投影 NAP

当WCCN跟在LDA后面时结果最好，EER为1.12%，MinDCF为0.0094，与经典的**联合因子分析 JFA**相比，我们还在10s-10s条件下获得了4%的绝对EER改善。

### 1 Intro

- JFA在GMM下补偿信道差异
- GMM-SVM：NAP
  - 核是基于两个GMM之间的KL距离的线性近似。超向量线性核
- GMM平均超向量是通过最大后验 MAP 自适应将通用背景模型 UBM 平均超向量适配到说话人帧得到的
- 结合JFA和SVM，直接用JFA估计的说话人因子作为SVM的输入，用WCCN补偿的余弦核效果最好。
- JFA估计的被认为仅模拟信道效应的信道因子，也包括说话人的信息。
- 提出一种新的基于因子分析的说话人确认系统作为特征提取器，因子分析用来定义一个低维空间——总变化空间，在这个空间中，一个给定的声纹可以被total factor表示，也就是i-vector

### 2 JFA

