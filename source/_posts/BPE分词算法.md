---
title: BPE分词算法
date: 2020-07-18 10:35:17
summary: 双字节编码, 把一个单词再拆分，使得我们的此表会变得精简，并且寓意更加清晰.
tags:
  - 算法
  - 分词
categories: nlp
---

# BPE(WordPiece)

##  前言

2018年最火的论文要属google的BERT，不过今天我们不介绍BERT的模型，而是要介绍BERT中的一个小模块WordPiece。

##  WordPiece原理

现在基本性能好一些的NLP模型，例如OpenAI GPT，google的BERT，在数据预处理的时候都会有WordPiece的过程。WordPiece字面理解是把word拆成piece一片一片，其实就是这个意思。

WordPiece的一种主要的实现方式叫做BPE（Byte-Pair Encoding）双字节编码。

BPE的过程可以理解为把一个单词再拆分，使得我们的此表会变得精简，并且寓意更加清晰。

比如"loved","loving","loves"这三个单词。其实本身的语义都是“爱”的意思，但是如果我们以单词为单位，那它们就算不一样的词，在英语中不同后缀的词非常的多，就会使得词表变的很大，训练速度变慢，训练的效果也不是太好。

BPE算法通过训练，能够把上面的3个单词拆分成"lov","ed","ing","es"几部分，这样可以把词的本身的意思和时态分开，有效的减少了词表的数量。

## BPE算法

BPE的大概训练过程：首先将词分成一个一个的字符，然后在词的范围内统计字符对出现的次数，每次将次数最多的字符对保存起来，直到循环次数结束。

我们模拟一下BPE算法。

我们原始词表如下：

{'l o w e r ': 2, 'n e w e s t ': 6, 'w i d e s t ': 3, 'l o w ': 5}

其中的key是词表的单词拆分层字母，再加代表结尾，value代表词出现的频率。

下面我们每一步在整张词表中找出频率最高相邻序列，并把它合并，依次循环。

```
原始词表 {'l o w e r </w>': 2, 'n e w e s t </w>': 6, 'w i d e s t </w>': 3, 'l o w </w>': 5}
出现最频繁的序列 ('s', 't') 9
合并最频繁的序列后的词表 {'n e w e st </w>': 6, 'l o w e r </w>': 2, 'w i d e st </w>': 3, 'l o w </w>': 5}
出现最频繁的序列 ('e', 'st') 9
合并最频繁的序列后的词表 {'l o w e r </w>': 2, 'l o w </w>': 5, 'w i d est </w>': 3, 'n e w est </w>': 6}
出现最频繁的序列 ('est', '</w>') 9
合并最频繁的序列后的词表 {'w i d est</w>': 3, 'l o w e r </w>': 2, 'n e w est</w>': 6, 'l o w </w>': 5}
出现最频繁的序列 ('l', 'o') 7
合并最频繁的序列后的词表 {'w i d est</w>': 3, 'lo w e r </w>': 2, 'n e w est</w>': 6, 'lo w </w>': 5}
出现最频繁的序列 ('lo', 'w') 7
合并最频繁的序列后的词表 {'w i d est</w>': 3, 'low e r </w>': 2, 'n e w est</w>': 6, 'low </w>': 5}
出现最频繁的序列 ('n', 'e') 6
合并最频繁的序列后的词表 {'w i d est</w>': 3, 'low e r </w>': 2, 'ne w est</w>': 6, 'low </w>': 5}
出现最频繁的序列 ('w', 'est</w>') 6
合并最频繁的序列后的词表 {'w i d est</w>': 3, 'low e r </w>': 2, 'ne west</w>': 6, 'low </w>': 5}
出现最频繁的序列 ('ne', 'west</w>') 6
合并最频繁的序列后的词表 {'w i d est</w>': 3, 'low e r </w>': 2, 'newest</w>': 6, 'low </w>': 5}
出现最频繁的序列 ('low', '</w>') 5
合并最频繁的序列后的词表 {'w i d est</w>': 3, 'low e r </w>': 2, 'newest</w>': 6, 'low</w>': 5}
出现最频繁的序列 ('i', 'd') 3
合并最频繁的序列后的词表 {'w id est</w>': 3, 'newest</w>': 6, 'low</w>': 5, 'low e r </w>': 2}
```

这样我们通过BPE得到了更加合适的词表了，这个词表可能会出现一些不是单词的组合，但是这个本身是有意义的一种形式，加速NLP的学习，提升不同词之间的语义的区分度。

1. 单词按照char分词， 单词结尾替换为某字符（\w or -）

2. 构造vocab：将相连的char组成pair，频次为word的cnt

3. 选取频次最高的max_pair，将vocab中的max_pair合并。 （频次最高的可能有多个pair，每次选择一个合并）
    参考代码：

```python
import re
def process_raw_words(words, endtag='-'):
    '''把单词分割成最小的符号，并且加上结尾符号'''
    vocabs = {}
    for word, count in words.items():
        # 加上空格
        word = re.sub(r'([a-zA-Z])', r' \1', word)
        word += ' ' + endtag
        vocabs[word] = count
    return vocabs


def get_symbol_pairs(vocabs):
    ''' 获得词汇中所有的字符pair，连续长度为2，并统计出现次数
    Args:
        vocabs: 单词dict，(word, count)单词的出现次数。单词已经分割为最小的字符
    Returns:
        pairs: ((符号1, 符号2), count)
    '''
    # pairs = collections.defaultdict(int)
    pairs = dict()
    for word, freq in vocabs.items():
        # 单词里的符号
        symbols = word.split()
        for i in range(len(symbols) - 1):
            p = (symbols[i], symbols[i + 1])
            pairs[p] = pairs.get(p, 0) + freq
    return pairs


def merge_symbols(symbol_pair, vocabs):
    '''把vocabs中的所有单词中的'a b'字符串用'ab'替换
    Args:
        symbol_pair: (a, b) 两个符号
        vocabs: 用subword(symbol)表示的单词，(word, count)。其中word使用subword空格分割
    Returns:
        vocabs_new: 替换'a b'为'ab'的新词汇表
    '''
    vocabs_new = {}
    raw = ' '.join(symbol_pair)
    merged = ''.join(symbol_pair)
    # 非字母和数字字符做转义
    bigram = re.escape(raw)
    p = re.compile(r'(?<!\S)' + bigram + r'(?!\S)')
    for word, count in vocabs.items():
        word_new = p.sub(merged, word)
        vocabs_new[word_new] = count
    return vocabs_new


raw_words = {"low": 5, "lower": 2, "newest": 6, "widest": 3}
vocabs = process_raw_words(raw_words)

num_merges = 10
print(vocabs)  # <class 'dict'>: {' l o w -': 5, ' l o w e r -': 2, ' n e w e s t -': 6, ' w i d e s t -': 3}
for i in range(num_merges):
    pairs = get_symbol_pairs(vocabs)
    # 选择出现频率最高的pair
    symbol_pair = max(pairs, key=pairs.get)
    vocabs = merge_symbols(symbol_pair, vocabs)
print(vocabs)
```

