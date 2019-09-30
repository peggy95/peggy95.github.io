# Tokenization
Pre-processing raw text data is basic and essential step for NLP tasks. Classic word representation cannot handle unseen word or rare word well. Character embeddings is one of the solution to overcome out-of-vocabulary (OOV). However, it may too fine-grained any missing some important information. Subword is in between word and character. It is not too fine-grained while able to handle unseen word and rare word.

## Difficulties
Usually in NLP, raw text or corpus will be pre-processed as tokens to generate the vocabulary which is going to be used while training and testing. At first, the tokenization method is just split sentences or sequences of corpus by white space.
```python
sentence = 'I love machine learning.'
tokens = sentence.split(' ')

print(tokens)
['I','love','machine','learning','.']
```
However, it is unlikely to put all tokens into our vocabulary since the number of tokens are huge and the memory and the time complexity when training models are not allowed. The common solution to this problem is to set a threshold. When the appreance of certain token is higher than this threshold, this token should be included. The rest low-frequency tokens will be encoded as `#UNK`. 

Obviously, if training and test set are not in similar distribution, that is there are a lot of low-frequency tokens in test set, the performance of the model trained by training set will be influenced. So, this kind of tokenization method is not that good.

## The better way to tokenize corpus
Some people suggested that the tokenization method is not supposed to be such simple and rude. There should be a better way to deal with those low-frequency words. In the field of machine translation, the common approaches are:
1. [Phrase-Based Backoff Models for Machine Translation of Highly Inflected Languages](https://www.aclweb.org/anthology/E06-1006): They proposed a backoff model for phrase-based machine translation that translates unseen word forms in foreign-language text by hierarchical morphological ab-stractions at the word and the phrase level.
2. Char-level tokenization. Since there might be unseen words, can we solve this problem by tokenizing sequences by a single letter or a single Chinese character? Because all words consist of characters.

Both 2 methods have pros and cons. The first one could improve the representation of low-frequency words by backoff table but it all depends on the quality of the backoff table. While although it solves unseen words by splitting them into characters in second method, the granularity of this approach is too fine.

## Byte Pair Encoding (BPE)
Inference: [Neural Machine Translation of Rare Words with Subword Units](https://arxiv.org/abs/1508.07909)

Neural machine translation (NMT) models typically operate with a fixed vocabulary, but translation is an open-vocabulary problem. Previous work addresses the translation of out-of-vocabulary words by backing off to a dictionary. In this paper, they introduce a simpler and more effective approach, making the NMT model capable of open-vocabulary translation by encoding rare and unknown words as sequences of subword units. This is based on the intuition that various word classes are translatable via smaller units than words, for instance names (via character copying or transliteration), compounds (via compositional translation), and cognates and loanwords (via phonological and morphological transformations). They discuss the suitability of different word segmentation techniques, including simple character n-gram models and a segmentation based on the `byte pair encoding` compression algorithm.

From [BPE - wikipedia](https://en.wikipedia.org/wiki/Byte_pair_encoding), you can know some basic info about BPE which is usually used for data compression. However, in NLP, this kind of tokenization gives us other benefits.
### Algorithm

1. Prepare a large enough training data (i.e. corpus)
2. Define a desired subword vocabulary size
3. Split word to sequence of characters and appending suffix `</w>` to end of word with word frequency. So the basic unit is character in this stage. For example, the frequency of “low” is 5, then we rephrase it to `l o w </w>: 5`
4. Generating a new subword according to the high frequency occurrence.
5. Repeating step 4 until reaching subword vocabulary size which is defined in step 2 or the next highest frequency pair is 1.

![Algorithm BPE](https://github.com/peggy95/peggy95.github.io/edit/master/blogs/BPE_blog_img/Algorithm_BPE.png "Algorithm BPE")
For example: the origin words and the corresponding frequency is: {'l o w e r ': 2, 'n e w e s t ': 6, 'w i d e s t ': 3, 'l o w ': 5}. The algorithm will act like:
```
Origin Vocabulary: {'l o w e r </w>': 2, 'n e w e s t </w>': 6, 'w i d e s t </w>': 3, 'l o w </w>': 5}

The subword with highest frequency: ('s', 't') 9
Vocabulary after merging: {'n e w e st </w>': 6, 'l o w e r </w>': 2, 'w i d e st </w>': 3, 'l o w </w>': 5}

The subword with highest frequency: ('e', 'st') 9
Vocabulary after merging: {'l o w e r </w>': 2, 'l o w </w>': 5, 'w i d est </w>': 3, 'n e w est </w>': 6}

The subword with highest frequency: ('est', '</w>') 9
Vocabulary after merging: {'w i d est</w>': 3, 'l o w e r </w>': 2, 'n e w est</w>': 6, 'l o w </w>': 5}

The subword with highest frequency: ('l', 'o') 7
Vocabulary after merging: {'w i d est</w>': 3, 'lo w e r </w>': 2, 'n e w est</w>': 6, 'lo w </w>': 5}

The subword with highest frequency: ('lo', 'w') 7
Vocabulary after merging: {'w i d est</w>': 3, 'low e r </w>': 2, 'n e w est</w>': 6, 'low </w>': 5}

The subword with highest frequency: ('n', 'e') 6
Vocabulary after merging: {'w i d est</w>': 3, 'low e r </w>': 2, 'ne w est</w>': 6, 'low </w>': 5}

The subword with highest frequency: ('w', 'est</w>') 6
Vocabulary after merging: {'w i d est</w>': 3, 'low e r </w>': 2, 'ne west</w>': 6, 'low </w>': 5}

The subword with highest frequency: ('ne', 'west</w>') 6
Vocabulary after merging: {'w i d est</w>': 3, 'low e r </w>': 2, 'newest</w>': 6, 'low </w>': 5}

The subword with highest frequency: ('low', '</w>') 5
Vocabulary after merging: {'w i d est</w>': 3, 'low e r </w>': 2, 'newest</w>': 6, 'low</w>': 5}

The subword with highest frequency: ('i', 'd') 3
Vocabulary after merging: {'w id est</w>': 3, 'newest</w>': 6, 'low</w>': 5, 'low e r </w>': 2}
```
In this way, we get a more appropriate vocabulary through BPE. 

## WordPiece
Inference: [Japanese and korean voice search](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=6289079)
[stackoverflow](https://stackoverflow.com/questions/55382596/how-is-wordpiece-tokenization-helpful-to-effectively-deal-with-rare-words-proble/55416944#55416944)
WordPiece is another word segmentation algorithm and it is similar with BPE. Schuster and Nakajima introduced WordPiece by solving Japanese and Korea voice problem in 2012. Basically, WordPiece is similar with BPE and the difference part is forming a new subword by likelihood but not the next highest frequency pair.
### Algorithm

1. Initialize the word unit inventory with all the characters in the text.
2. Build a language model on the training data using the inventory from 1.
3. Generate a new word unit by combining two units out of the current word inventory to increment the word unit inventory by one. Choose the new word unit out of all the possible ones that increases the likelihood on the training data the most when added to the model.
4. Goto 2 until a predefined limit of word units is reached or the likelihood increase falls below a certain threshold.the next highest frequency pair.

## Conclusion
WordPiece and BPE are two similar and commonly used techniques to segment words into subword-level in NLP tasks. In both cases, the vocabulary is initialized with all the individual characters in the language, and then the most frequent/likely combinations of the symbols in the vocabulary are iteratively added to the vocabulary.

This vocabulary may have some combinations that are not words, but this is a meaningful form, speeding up the learning of NLP and improving the semantic distinction between different words. Because a model might know the relationship between between “smart”, “smarter”, and “smartest” when the model saw “old”, “older”, and “oldest” in training process. This is not benefit from word-level tokenization method.

Meanwhile, OOV words are impossible if you use such segmentation methods. Any word which does not occur in the vocabulary will be broken down into subword units. 
