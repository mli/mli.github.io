---
layout: post
author: Mu Li
categories: GPU
comments: true
title: GPU集群折腾手记——2015
---

<div class="message">
- 2/6/2016 文末更新了购买推荐
</div>

整个2015年都在买买买。。。买GPU。于是整整一年都在各处比较，下单，拆，装，维护。为了省点钱煞费苦心，荒废了很多其他重要事情。所以想把经验教训写下来供各位DIY玩家参考。

我们买GPU的目的是用来做**科学计算**，例如针对深度学习。另外两个GPU主要用途——游戏，挖矿——则不在此文讨论范围。而且此文针对性价比敏感人士，对于土豪人群，推荐直接上大厂整体GPU集群解决方案，可省去大量力气。

## 买什么样的GPU

对于很多科学计算而言，性能主要决定于GPU的浮点运算能力。特别是对深度学习任务来说，主要是**单精度**浮点运算（未来可能默认用更低的精度，例如16bit）。而其他因素，例如CPU速度，内存大小，通讯带宽，对目前大多数深度学习任务而言，只要过了某个合理的阈值不够成性能瓶颈就行。

对于GPU而言，简单来看有三个重要参数，**浮点运算能力**，**价格**与**功耗**。下面两个图比较了Nvidia Tesla，Geforce 700和900系列各卡的这三个参数（前两个参考了wikipedia，后一个是查询了amazon/newegg上的当前价格）。

![](https://raw.githubusercontent.com/mli/mli.github.io/master/imgs/gpu-watt.png){: style="width:500px; display:block; margin-left:auto; margin-right:auto" :}
![](https://raw.githubusercontent.com/mli/mli.github.io/master/imgs/gpu-price.png){: style="width:500px; display:block; margin-left:auto; margin-right:auto" :}


整体来看浮点运算和功耗成正比。在价格上，如不考虑土豪用Tesla系列话，N厂在消费级别卡上是一分钱一分货。目前的购买建议是优先考虑Titan X，但如果不是特别需要其12GB内存的话，则考虑浮点运算性价比更好的980 TI（6GB 内存）。但如果嫌980 TI功耗比过高的话，则考虑980. 除非有特别的理由，不推荐980之下的卡了。因为达到同样的计算能力，使用更便宜的卡会增加机器数量，可能导致其他配套成本（例如CPU，电源，主板）和维护成本的增加。（下面将会有血与泪的教训）。

<div class="message">
GPU更新换代快，不宜大规模采购超出现在需求的机器。例如Nvidia下一代号称3D内存可能会带来新的加速，通常认为性价比更高的AMD也将会推出新的编译器来更好支持科学计算。
</div>

选好了GPU后便可以配套其他了，与正常装机无异。但有两点需要额外考虑。一是供电。一块GPU很可能有200w，4卡则800w了，加上CPU内存，电源至少要个1kw才行。二是散热。GPU发热巨大，将多块GPU并排放会导致很严重的散热问题。此外，如果大量安装GPU机器，机房供电和散热也会是需要考虑。

## 折腾手记

下面我具体分享下15年折腾过的机器。按照时间顺序一一介绍：

| GPU | CPU | 内存 | 尺寸 | 单价 ($) |
| --- | --- | ---: | ---: | ---: |
| 2 x GTX 980 | 2 x E5-2680 v2 | 128G | 2U | 12k |
| 2 x GTX 970 | i5-3437 | 32G | mid tower | 1.3k |
| 4 x GTX 980 | E5-1650 | 64G | super tower | 6.5k |
| 4 x GTX 750 TI | i7-4790 | 16G | full tower | 1k |
| 4 x K40 | 2 x E5-2670 | 64G | 4U | free |
| 4 x Titan X | 2 x E5-2630 v3 | 128G | 1U | 11k |

### 双GTX 980集群

此集群是土豪系统组买的。配了当时最好的CPU之一，双40GB网络，光是交换机就花了10W刀。买回来后半年没人拆封，但被alex意外发现了。。。（下图是alex拆了一台在看配置）

![](https://raw.githubusercontent.com/mli/mli.github.io/master/imgs/alex_2u.jpg){: style="width:400px; display:block; margin-left:auto; margin-right:auto"  }

于是我们厚脸皮的跑去说“来，我们帮你们装上，顺便出钱升级电源和买GPU，到时候共享给我们用一下就好了”。最后是alex从ebay上的某香港卖家买电源，dave去newegg买了卡。

这个集群是我个人最爱，安装简单，2天全搞定。网络快到飞起，很适合做分布式运算。（下图是集群背部，红线是光钎网线）

![](https://raw.githubusercontent.com/mli/mli.github.io/master/imgs/mass_back.jpg){: style="width:400px; display:block; margin-left:auto; margin-right:auto"  }

因为是从CPU集群改装而来，CPU过于的好了，只能插双卡（因为不是为GPU设计，所以即使是2U机器也最多插双卡，而且是掰断了中间某个部件的情况下），内存对于双卡来说过于大了，网络也是过于土豪，根本用不完。而且经常长时间跑大任务，曾把电源给烧了。。。

![](https://raw.githubusercontent.com/mli/mli.github.io/master/imgs/fired.jpg){: style="width:300px; display:block; margin-left:auto; margin-right:auto" }

### 双GTX 970集群

情人节那天alex惊喜的掏出一板巧克力一样的东西，说“看，intel送的！”。当时我就震惊了。后来发现原来是一打CPU，用冰箱里制冰块一样的板子装着，一个坑填一块CPU，活像一板巧克力。但送的是n年前被淘汰的CPU，做CPU集群太弱，于是我们准备去折腾个便宜的GPU集群。

下面是前三台订单，基本什么都是挑的最便宜的

![](https://raw.githubusercontent.com/mli/mli.github.io/master/imgs/dual970order.png){: style="width:600px; display:block; margin-left:auto; margin-right:auto"  }

之后又陆续买了几台，然后忽悠别组老师也投资买了几台。于是好一阵子组会都是装机大会。

![](https://raw.githubusercontent.com/mli/mli.github.io/master/imgs/upgrade_cpu.jpg){: style="width:400px; display:block; margin-left:auto; margin-right:auto"  }

![](https://raw.githubusercontent.com/mli/mli.github.io/master/imgs/build_hydrogen.jpg){: style="width:400px; display:block; margin-left:auto; margin-right:auto"  }

这个集群的优点是便宜，十台机器1W刀左右搞定（一个phd学生一年要花费老板10W+刀）。但由于是分多批买的，每次买都是挑当时最便宜的硬件，例如电源有4种（后来我们发现电源接GPU线是不通用的）。虽然GPU都是970，但来自4个不同的牌子，msi，evga, pny， gigabyte. 便宜硬件出错率高，不同牌子的硬件的维护是个噩梦。另外一个问题是机箱占地方，大小基本等价一个4U的机器了，而且就插了2卡，而4U机器现在基本可以插到8卡了。一开始没想到会买一大批，结果现在它占了我们小机房四分之一的空间。

![](https://raw.githubusercontent.com/mli/mli.github.io/master/imgs/hydrogen.jpg){: style="width:400px; display:block; margin-left:auto; margin-right:auto"  }

### 四卡GTX 980

折腾得最多的机器，但仍然是最爱。当时装了一堆便宜机器后，想去弄个高大上点的：

![](https://raw.githubusercontent.com/mli/mli.github.io/master/imgs/beast_order.png){: style="width:600px; display:block; margin-left:auto; margin-right:auto"  }

alex手艺高超的把大把GPU，内存，硬盘晒进了一个mid tower，装完是这样的：

![](https://raw.githubusercontent.com/mli/mli.github.io/master/imgs/build_beast.jpg){: style="width:400px; display:block; margin-left:auto; margin-right:auto"  }

结果发现散热是个大问题。一机箱太满，二机箱风扇不给力，三显卡风扇不给力。之后做了无数次测试，升级，期间烧坏了两快卡。目前机箱大了很多，从full tower变成了super tower，前后装满了风扇，硬盘都卸了，第一块坏的卡送修了，第二块懒得动了，现在跛着脚三卡在跑。

### 四卡GTX 750 TI

这是dave用来挖矿的机器。一开始是裸的，就是主板接电源插GPU一字排开趟在dave的地下室挖矿，顺便当地暖用。后面挖矿不赚钱了dave发现入不敷出了，于是我们装上了机箱重新利用一下。但发现这些机器用的游戏主板专为windows优化，而且又是来自不同厂家。花了整整两天才摸清怎么pxe启动装上centos（其实现在还是有一台没弄好）。

### 四卡K40机器

Intel送了一个4U的机器，Nvidia送了4快K40。半个小时搞定，一切都挺好。

### 四卡Titan X机器

刚买没几天。起因是上个月的NIPS我找N厂哭穷，人家见我长得眉清目秀（误）便送了一块卡。回来献给老板，老板因为国内某大厂赞助了些钱大手一挥补齐了剩下。往Supermicro 1U机器里塞了4快卡，内部空间寸土必争，花好几个小时才装好，一切都是刚好fit，特别治愈强迫症。

![](https://raw.githubusercontent.com/mli/mli.github.io/master/imgs/quad_titan_x.jpg){: style="width:600px; display:block; margin-left:auto; margin-right:auto"  }

因为没有什么剩余空间，且是长的串行结构散热（散热顺序是单GPU，双CPU，三GPU），所以风扇功率很大，噪音跟轰炸机一样。有点耗电，全力跑电流需要10A。不过其他没发现什么问题。用上它了之后小伙伴们就死活不下来去用的机器了。

## 总结

折腾硬件就跟其他很多事情一样，要么有钱，要么得有时间折腾。对一般玩家的建议是，如果只是上一两台，可以在newegg上多选多看；但如果是准备买数十台，建议还是用rack集群，模块化的设计不仅省空间，而且维护简单。不过最重要的还是，高兴就好。happy hacking!

最后感谢alex，dave和各位小伙伴。下图拍于16年元旦夜。

![](https://raw.githubusercontent.com/mli/mli.github.io/master/imgs/zhuangjixiaohuoban.jpg){: style="width:500px; display:block; margin-left:auto; margin-right:auto"  }

## 附录：购买推荐

### 2/6/16更新

使用下来对supermicro的4卡1U机器很是喜欢，一是占地小，二是性价比高，散热也没发现是大问题。下面推荐的4卡机器是之前买的便宜版，总价不到7K。相比之前的区别是

  - 使用了老型号的CPU。据说是因为facebook处理了一大批机器，所以这个型号的翻新便宜到爆（几个月前的事情，前天才发现。。）。但性能对于GPU机器来说足够了。
  - 因为CPU的缘故，可以使用老型号的主板。
  - 128G内存完全没必要，64G绰绰有余。因为神经网络耗内存的是中间结果，那些不需要传回主内存。

已经定了三台， 过些天再发体验报告.

|     | 型号 | 个数 | 总价 ($) | 购买 |
| --- | --- | --- | --- | --- |
| 机箱 | Supermicro SYS-1027GR-TQFT | 1 | 1477 | [wiredzone](http://www.wiredzone.com/supermicro-servers-1u-barebone-dual-processor-sys-1027gr-tqft-10021982) |
| GPU | GTX Titan X | 4 | 4488 | [amazon](http://www.amazon.com/EVGA-GeForce-GAMING-Graphics-12G-P4-2992-KR/dp/B00UXTN5P0/ref=sr_1_1?s=pc&ie=UTF8&qid=1454810016&sr=1-1&keywords=titan+x) |
| CPU | Intel E5-2670 | 2 | 200 | [dds](http://www.deepdiscountservers.com/intel-xeon-e5-2670-2-6ghz-3-3ghz-turbo-20mb-l3-cache-lga2011-115w-eight-core.html) |
| 内存 | Kingston 16GB | 4 | 403 | [wiredzone](http://www.wiredzone.com/kingston-components-memory-ddr3-kth-pl316k4-64g-32028349) |
| SSD | samsung 850 500GB | 1 | 153 | [amazon](http://www.newegg.com/Product/Product.aspx?Item=N82E16820147373) |
| HDD | samsung 2TB 5400 RPM | 2 | 188 | [amazon](http://www.newegg.com/Product/Product.aspx?Item=N82E16822178627) |
