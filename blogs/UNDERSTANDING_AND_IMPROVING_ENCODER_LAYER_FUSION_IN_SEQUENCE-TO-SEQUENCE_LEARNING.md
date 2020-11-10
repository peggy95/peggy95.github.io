## Paper Reading: UNDERSTANDING AND IMPROVING ENCODER LAYER FUSION IN SEQUENCE-TO-SEQUENCE LEARNING
---
[Paper Detail](https://openreview.net/pdf?id=n1HD8M6WGn)
## Abstrct
在大部分的Seq2Seq任务中，已经有人证明使用所有的encoder layer的输出，比不单单只使用最后一层encoder的输出，效果会更好，这样使用encoder layer的方式称为EncoderFusion。
但是目前并不完全清楚EncoderFusion有效的原因。
有很多之前的工作认为原因可能是其利用了较低encoder layer的浅层和句法信息；
但我们却发现，最开始的embedding layer比那些中间的encoder layer更重要；最后一层的decoder layer在多个NLP任务上也都对embedding layer有更高的关注（注意力）。
基于这些观察，我们提出一种SurfaceFusion的方法，只把embedding layer和softmax layer融合起来。

## EncoderFusion
我们认为，随着层数的增加，模型会慢慢提取到更抽象的特征。合理利用抽象表征和浅层表征对大多NLP任务是有效的。
但，目前很多Seq2Seq模型中，decoder只接受最上层encoder的输出，也就是只利用了encoder提取到的抽象特征；即便这些抽象特征也是从下层提取出来的，但是仍旧可以说没有利用这些低层的encoder信息损害了最后的表达能力。

为了评估每一层encoder对最后输出的影响，有人提出了layer attention的思路，对于每一层的decoder都分配一个权重给所有encoder层；借此来看每一层encoder对于每一层decoder的影响。但是这样一个encoder对decoder的常数权重不足以用来评估。
所以我们提出一种fine-grained layer attention，让每一层的每个神经元都被分配一个权重值。
![layer_attention](https://github.com/peggy95/peggy95.github.io/blob/master/blogs/UNDERSTANDING_AND_IMPROVING_ENCODER_LAYER_FUSION_IN_SEQUENCE-TO-SEQUENCE_LEARNING_img/layer_attention.jpg "layer_attention")
### Visualization of layer attention 
我们做了在三个NLP任务上的实验来说明每一个decoder layer和encoder layer的贡献。图中的权重是所有纬度上的均值。
![layer_score](https://github.com/peggy95/peggy95.github.io/blob/master/blogs/UNDERSTANDING_AND_IMPROVING_ENCODER_LAYER_FUSION_IN_SEQUENCE-TO-SEQUENCE_LEARNING_img/layer_visual.png "layer_score")
可以从上图看出，所有任务上都发现，较高的decoder layer对embedding layer有更高的关注，说明底层的浅层表达也为模型提供了额外信息。
### Contribution of individual encoder layer
为了验证encoder的作用并不是靠增加noise提高的泛化，我们通过mask部分encoder层来定量分析Seq2Seq模型的表现。这里的mask并不会影响encoder截断的层层传导；只是在decoder的阶段把某些encoder layer的权重设置为0.
![contribution](https://github.com/peggy95/peggy95.github.io/blob/master/blogs/UNDERSTANDING_AND_IMPROVING_ENCODER_LAYER_FUSION_IN_SEQUENCE-TO-SEQUENCE_LEARNING_img/contribution.png "contribution")
可以看出把embedding和最后的encoder layer masked之后，效果损害严重。
