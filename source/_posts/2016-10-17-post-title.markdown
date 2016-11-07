---
layout: post
title: "关于专利的杂想"
date: 2016-10-17 16:21:47 +0800
comments: true
categories: 杂
---

#基于农业的本体知识库构建
* 应用场景
> 来源于一个农产品的网页上架的，自动填写信息的启发
>
> 例如：拿到一个土豆，通过图像识别出这个产品是土豆（可能需要添加几个标签去区分是土豆而不是地瓜）
> 
> 已经构建好的一个大的语义网,根据土豆这个词去search这个专有领域本体库中于这个相关连的概念（种类，功能，种植季节等）
> 
> 服务反馈？－－按照概念（以标签的形式表示，供给用户选择）反馈信息，填写好对应的内容，并且做一次相关度推荐<!--more-->

* 流程描述
  1. 网页抽取相对应的文本知识（wiki百科）得到本体中的概念
  2. 概念融合－1.依据相似度进行融合，设置经验阈值 2.本体推理－去找出关系，建立层级结构（树状或者网状）
  3. 得到基于农产品专有领域的语义网
  ![构造](http://ofw47ln5s.bkt.clouddn.com/16-10-31/1595354.jpg)

* reference

 ![农业本体和融合规则的知识融合框架](http://ofw47ln5s.bkt.clouddn.com/16-10-31/76112436.jpg)
 ![框架图](http://ofw47ln5s.bkt.clouddn.com/16-10-31/59209975.jpg)
 ![参考模型](http://ofw47ln5s.bkt.clouddn.com/16-10-31/32477478.jpg)
 
* website

  1. [自适应提取方法](http://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=5659203) 
  2. [基于文献的农业领域本体自动构建方法](http://www.cas.stc.sh.cn/jsjyup/pdf/2014/8/%E5%9F%BA%E4%BA%8E%E6%96%87%E7%8C%AE%E7%9A%84%E5%86%9C%E4%B8%9A%E9%A2%86%E5%9F%9F%E6%9C%AC%E4%BD%93%E8%87%AA%E5%8A%A8%E6%9E%84%E5%BB%BA%E6%96%B9%E6%B3%95%E7%A0%94%E7%A9%B6.pdf)
  3. [Concept Similarity Matching Based on Semantic
Distance](http://disi.unitn.it/~p2p/RelatedWork/Matching/Ge_SKG'08.pdf)
  4. [method](http://dl.ifip.org/db/conf/ifip12/ccta2012-1/JiangZYX12.pdf)


