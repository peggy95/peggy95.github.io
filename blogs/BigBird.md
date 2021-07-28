# Paper Reading - Big Bird: Transformers for Longer Sequences

## [paper link](https://proceedings.neurips.cc/paper/2020/file/c8512d142a2d849725f31a9a7a361ab9-Paper.pdf)

提出问题：
1. 是否能够使用更少的内积计算来达到full attention的效果？
2. sparse attention是否保留了full的performance？
本文提出一种BigBird的sparse attention方式。将原本full attention对序列长度呈quadratic的复杂度变为linear级别。
1. 证明了该sparse attention的方式on a par with full attention；
2. 同时提出了global tokens，并理论证明其benefies；

具体做法：
1. global tokens会和所有tokens进行attention
2. 每个token都会和周边的w local neighboring tokens进行attention
3. 每个token也会和random的一些tokens进行attention

相关工作：
1. 第一类相关工作想的是利用sliding window的方式去将一次不可完成的full self attention转换成多次subset full self-attention： SpanBERT，ORQA, REALM, RAG; 问题是他其实是在用时间换取一次self attention的空间；这样依旧会有训练困难的问题；
2. 第二类相关工作则是在question full attention的重要性；是否可以不进行full attention，不计算那么多就拿到比肩的效果呢；
auto-regresive models；
稀疏模型，复杂度N 根号N
Reformer：依靠LSH（局部敏感hash）n log n的复杂度；
104: binary partition
73: block sparsity
Longformer： localized sliding window + mask + global mask
但问题是，以上这些方法无法在所有任务上获得SoTA，也无法同时处理编码和解码，并且没有理论上的保证。

建模方式
下图可以看作一次full attention的mask，行列分别表示q/k。被涂改的方块代表对应的q和对应的k可以计算inner product。
abc分别表示了三种可能的attention方式：random/sliding/global
![image](https://user-images.githubusercontent.com/32155956/127124583-b1f3824a-5648-4240-8150-298cfca46bb0.png)
1. 随机图
2. window：和相邻序列相关性更强；更紧凑的attention
