---
title:【ios】关于UIImageView的一个坑
date: 2016-03-15 14:06:14
tags: ios
---

今天在处理UICollectionView的didSelectItemAtIndexPath时，发现怎么点击都没起作用（根本没进入didSelectItemAtIndexPath）。

<!-- more -->

调试了两个小时也没找到原因发火。。

最后在同事的帮助下发现UICollectionView的父视图是UIImageView。

UIImageView中有一个userInteractionEnabled方法，默认值为NO，影响了UICollectionView的点击事件。

将userInteractionEnabled设置为YES就完美解决了问题。




