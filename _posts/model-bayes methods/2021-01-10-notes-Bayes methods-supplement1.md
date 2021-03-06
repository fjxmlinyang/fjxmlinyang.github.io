---
title: 'notes-Bayes methods-supplement'
date: 2021-01-10
permalink: /posts/model-bayes methods/2021-01-10-notes-Bayes methods-supplement1
tags:
  - Models
  - Machine Learning
  - Optimization
---









这道题心算即可——
（1）先验比率是75% : (1-75%) = **3 : 1**；
（2）似然比率（Likelihood ratio）= 90% : 30% = **3**；
（3）两者相乘，得后验比率 = **9 : 1**；然后
（4）标准化（normalize），得后验概率 = 9 / (9+1) = **90%**。

【故事】
让我们先来做一道小学数学题。

问：假设如来佛和玉皇大帝要打架，如来拥有全宇宙战斗力的 75%，剩下的战斗力都归玉帝。那么，玉帝的战斗力占全宇宙的多少？
答：太简单了，100% - 75% = 25%

问：此时，如来和玉帝的战力比是多少？
答：容易，75% : 25% = 3 : 1。

问：好了，现在，假设太上老君发明了一种仙丹，能增强战斗力，如来佛和玉帝都偷吃了一颗。可是，如来和玉帝体质不同——如来吃了之后，战斗力增加了 90 倍；玉帝吃了之后，战斗力只增加 30 倍。请问，两人偷吃仙丹之后，如来佛和玉帝的战力比变成了多少？
答：这也不难——如来相对战斗力 = 3 x 90 = 270，玉帝相对战斗力 = 1 x 30 = 30。
因此，如来战力：玉帝战力 = 270 : 30 = 9 : 1

问：好了，如来、玉帝吃了仙丹之后，如来占全宇宙战斗力的多大比例？
答：如来战斗力占比 = 9 / (9 + 1) = 90%。

【解释】
我觉得，理解贝叶斯定理的最大障碍，不是原理，而是太多小数、分数、百分数，比来比去、极易混淆。如果全部都是整数，那就好理解多了。实现方法是，先把题目的小数化为整数（之比），作运算，最后再化为小数。

现在让我把上面的故事翻译一下，套到题目上。

(1) 你一开始有两个假说，良好（H1）和故障（H2），
H1和H2的先验概率比 = P(H1) : P(H2) = 3 : 1。【用P(良好) = 75%即可推出。】

(2)现在你拿到了一个证据E：第一天产品是合格的。这个证据的作用，在于改变两个假说的概率之比。

根据贝叶斯定理，这个「1 个零件合格」的证据会产生两个效果：
a. 会让 「机器良好」(H1) 的相对概率增加 90 倍 【因为 P(E|H1) = 90% 】，及
b. 会让 「机器故障」(H2) 的相对概率增加 30 倍 【因为 P(E|H2) = 30% 】。
【公式上看，应该是分别缩减到 0.9 倍和 0.3 倍，但是化为整数比较容易思考。】

(3) 根据(2)，我们有了证据 E 之后，良好和故障的概率之比变为 3 x 90 : 1 x 30 = 270 : 30 = 9 : 1。

(4) 根据现有条件，其实还算不出 P(良好|1个合格) 。要算出 P(良好|1个合格) 的具体数值，还须明确给出一个条件，即「良好」和「故障」已经包括所有的假设了：
P(良好|1个合格) + P(故障|1个合格) = 1。
然后联立刚才得到的
P(良好|1个合格) : P(故障|1个合格) = 9 : 1，可解出
P(良好|1个合格) = 90%。

这就是一开始的计算思路。

【公式】

![img](https://pic3.zhimg.com/50/37dda8677ffce67e6e7262cf275ff6bf_hd.jpg?source=1940ef5c)![img](https://pic3.zhimg.com/80/37dda8677ffce67e6e7262cf275ff6bf_1440w.jpg?source=1940ef5c)


对应刚才的推理的，是贝叶斯定理常见形式的一个变体——分别对H1、H2列式，两式相除即可得。
最左边一项是先验比率（如来和玉帝吃仙丹前的实力对比），
中间一项是似然比率（仙丹对两人的效用比），
最右边一项是后验比率（如来和玉帝吃了仙丹后的实力对比）。



上述运算过程，实际上用的是下式，与上式有微妙区别：

![img](https://pic1.zhimg.com/50/f988abf073e648be0e9c2aad8e855352_hd.jpg?source=1940ef5c)![img](https://pic1.zhimg.com/80/f988abf073e648be0e9c2aad8e855352_1440w.jpg?source=1940ef5c)


【拓展】
这个形式的好处是非常容易拓展，比如说，如果要考虑3个（独立的）证据Ea、Eb、Ec：

![img](https://pic2.zhimg.com/50/82ad08575102860210355fecd18c12a8_hd.jpg?source=1940ef5c)![img](https://pic2.zhimg.com/80/82ad08575102860210355fecd18c12a8_1440w.jpg?source=1940ef5c)


回到原题。若问，假设这个机器第一天不是生产了 1 个零件，而是生产了 3 个零件，而且 3 个都合格（零件合格的概率互相独立），那机器良好的概率是多少？
（用故事的语言就是，如果如来和玉帝连吞3颗仙丹，那么如来的战斗力占全宇宙的多少？）
答案：
P(良好|3个合格) : P(故障|3个合格) = (3 : 1) x 3 x 3 x 3 = 81 : 1。【3 个合格零件，所以乘3次】
假设 P(良好|3个合格) + P(故障|3个合格) = 1，则
P(良好|三个合格) = 81 / (81 + 1) = 98.8%

