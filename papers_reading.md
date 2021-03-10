# Basic techniques
There are some tricks.
## [Stochastic Shared Embeddings: Data-driven Regularization of Embedding Layers](https://arxiv.org/abs/1905.10630)
1. 在embedding layer后添加了SSE Layer: 在训练时随机的对两个embedding进行替换；在测试时关闭。
2. 如果对embedding有 graph的先验时，可以使用random walk的方法对embedding进行随机替换；
3. 如果对embedding没有先验知识，可以根据embedding个数N计算出概率被替换概率，由于缺少先验，我们认为每个embedding的被替换概率一致，且替换成其他embedding的概率一样；

why works: 
1. 类比其他正则化手段，比如dropout，有随机性；且也和label smooothing/bert中随机替换思路类似
