---
layout: post
author: Mu Li
comments: true
title: Nvidia新的Pascal值不值得买（升级）
---

Nvidia最近更新了三块基于Pascal的GPU，分别是面向高端用户的P100和正常用户的GTX 1080和GTX 1070. 很多用GPU做计算的小伙伴问新卡值不值得买。很明显是，无论从功耗，价格（下面两图比较了N厂各卡），还是内存大小和带宽来看，肯定是新卡更好。而且这是三个之中个人推荐是GTX 1080。原因是P100太贵目前不是一般人能买的，虽然1070和1080在价格，功耗，计算上基本是正比关系。但在付相同额外开销（CPU，内存，机器空间，后期维护）下，1080能得到的计算密度更高。

![](https://raw.githubusercontent.com/mli/mli.github.io/master/imgs/gpu-watt-2.png){: style="width:700px; display:block; margin-left:auto; margin-right:auto" :}
![](https://raw.githubusercontent.com/mli/mli.github.io/master/imgs/gpu-price-2.png){: style="width:700px; display:block; margin-left:auto; margin-right:auto" :}

下一个问题是，值不值得升级现有的卡到pascal。这个问题比较纠结，主要是因为nvidia有几个东西没有解释清楚。

## 更快的FP16还是int8

如果大家听过黄老大在今天GTC上的报告的话，已经会对P100的21 TFLOPs的半精度(FP16)计算能力有深刻印象。它的原理是吧两个FP16并成一个FP32来计算。事实上这也是我们对pascal的最大期望之一。不管是去年NIPS，在GTC前nvidia的一个小型deep learning meetup，还是GTC，N厂员工一直说FP16是未来，我们需要大力支持。所以我们MXNet的小伙伴也花了很多精力折腾FP16，毕竟是两倍性能提升啊。

![](https://raw.githubusercontent.com/mli/mli.github.io/master/imgs/gpu-price-2.png){: style="width:700px; display:block; margin-left:auto; margin-right:auto" :}

P100太贵玩不起， 但GTX还是可以的。所以新卡出来后MXNet小伙伴Eric第一时间从N厂化缘了一块GTX 1080，兴高采烈准备测一下FP16搞个大新闻。结果跑下来眼镜碎一地。

> 在titanx上fp16比fp32快10%
> 基本正常
> 在1080上fp16比fp32慢一倍
> fullyconnected慢100倍。。。
> 还可变的。。。

此时网上也是[议论纷纷](https://www.reddit.com/r/MachineLearning/comments/4lhrfj/fp16_performance_on_gtx_1080_is_artificially/)，甚至大家猜测是N厂故意限速了。

我们马上质询了下N厂，得到的答复是，GTX 1070/1080确实FP16不快，但是8-bit int (int8)很快哦，在30 TFLOPs以上，你们可以试试啊。汗一地。。。
