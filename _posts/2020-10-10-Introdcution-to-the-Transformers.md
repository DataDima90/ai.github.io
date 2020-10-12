---
layout: post
title: Introduction to the Transformers in NLP
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [NLP, Transformers]
mathjax: true
summary: How do Transformers work in NLP?
---

# Understanding Transformers in NLP

The Transformer in NLP is a novel architecture that aims to solve seq2seq tasks (see post xx) while handling
long-range dependencies with ease. The idea is to handle the dependencies between input and output with attention
and recurrence completely.

The entire corpus can be split into fixed-length segments of manageable sizes.
Then, we train the Transformer model on the segments independently, ignoring all contextual
information from previous segments:

[image]

This architecture does not suffer from the problem of vanishing gradients. But the context fragmentation limits its
long-term dependency learning. During the evaluation phase, the segment is shifted to the right by only one position.
The new segment has to be processed entirely from scratch. This evaluation method is unfortunately quite compute-intensive.

# Model Architecture
[image]


# Papers for everyone who is interested in NLP
- Attention is All You Need
- Transformer-XL: Attentive Language Models Beyond a Fixed-Length Context

# Other sources
- https://www.analyticsvidhya.com/blog/2019/06/understanding-transformers-nlp-state-of-the-art-models/


# Challenges/Limitations
Transformer architectures can learn long-term dependency, but it comes with other limitations:
- Attention can only deal with fixed-length text strings, that means,
it can't stretch beyond a certain level due to the use of fixed-length context (input text segments).
The text has to be split into a certain number of segments or chunks before being fed into the system as input.
- This chunking of text causes **context fragmentation**. For example, if a sentence is split from the middle, then
a significant amount of context is lost (test is split without any other semantic boundary)


# Understanding Transformers-XL in NLP
During the training phase in Transformer-XL, the hidden state computed for the previous state is used as an additional
context for the current segment. This recurrence mechanism of Transformer-XL takes care of the limitations of
using a fixed-length context.

[image]

Now during the evaluation phase, the representations from the previous segments can be
reused instead of being computed from scratch (as in the case of the Transformer model). This is of course, increses
the computation speed manifold.

