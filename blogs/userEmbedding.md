# 本文旨在记录论文阅读后的一些感悟。

## Practice on Long Sequential User Behavior Modeling for Click-Through Rate Prediction

参考：[知乎:广告推荐系统排序（三）MIMN长期行为建模Long Sequential Behavior Mode](https://zhuanlan.zhihu.com/p/163511597)

论文对线上系统面临的挑战解释的很清楚，结合实际也提出了一些经验教训：
* 小时级别的更新用户表达也能取得不错的效果，解释说是模型泛化；
* 不包含大促期间的数据效果更好；
* 用120天的行为进行预训练warm up
* 增量更新，每日备份，做好roll back准备。

未解决问题：
* 模型迭代方案，一旦模型更新，120天warm up需要重新来一次；
