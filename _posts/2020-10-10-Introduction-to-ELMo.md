---
layout: post
title: Introduction to Embeddings from Language Models (ELMo)
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [NLP, ELMo]
mathjax: true
summary: Introduction to ELMo
---


# Embeddings from Language Models (ELMo)

### Overview


### Prerequisite for this article
- Understanding of Word Embeddings
- Understanding of Long Short Term Memory (LSTM)


### Definition
ELMo is a feature-based approach. In this approach, 
a pre-trained neural network produces word embeddings which are then used as features in NLP models.
In other words, it uses task-specific architectures that include
the pre-trained representations as additional features.

### Understanding how ELMo works / Model Architecture

ELMo word vectors are computed on top of a two-layer bidirectional language model (biLM).
This biLM model has two layers stacked together. Each layer has 2 passes -
forward pass and backward pass:
[image]

- The architecture above uses a character-level **convolutional neural network (CNN)** (see article xx)
to represent words of a text string into raw word vectors
- These raw word vectors act as inputs to the first layer of biLM
- The forward pass contains information about certain word and the context (other words) before that word
- The backward pass contains information about the word and the context (other words) after it
- This pair of information, from the forward and backward pass, forms the intermediate word vectors
- These intermediate word vectors are fed into the next layer of biLM
- The final representation (ELMo) is the weighted sum of the raw word vectors and the two intermediate word vectors.

As the input to the biLM is computed from characters rather than words, it captures the inner structure of the word.
For example, the biLM will be able to figure out that terms like *beauty* and *beautiful* are related 
at some level without even looking at the context they often appear in. Sounds incredible!

### How is ELMo different from other word embeddings?
Unlike traditional word embeddings such as word2vec and GLoVe, the ELMo vector assigned to a token
or word is actually a function of the entire sentence containing that word. Therefore, the same word 
can have different word vectors under different contexts.

Suppose we have a couple of sentences:
1. I **read** the book yesterday.
2. Can you **read** the letter now?

The verb "read" in the first sentence is in the past tense. And the same verb transforms into present tense
in the second sentence. This is a case of Polysemy wherein a word could have multiple meanings or senses.

Traditional word embeddings come up with the same vector for the word "read" in both sentences. 
Hence the system would fail to distinguish between the polysemous words. These word embeddings
just cannot grasp the context in which the word was used.

ELMo word vectors successfully address this issue. ELMo word representations take the entire input sentence
into equation for calculating the word embeddings. Hence, the term "read" would have different 
ELMo vectors under different context.

### Examples
- Machine Translation
- Language Modeling
- Text Summarization
- Named Entity Recognition
- Question-Answering Systems

### Challenges/Limitations


### Papers
- Deep contextualized word representations: https://arxiv.org/pdf/1802.05365.pdf

### Sources
- https://www.analyticsvidhya.com/blog/2019/03/learn-to-use-elmo-to-extract-features-from-text/
- https://towardsdatascience.com/bert-explained-state-of-the-art-language-model-for-nlp-f8b21a9b6270
- https://jalammar.github.io/illustrated-bert/
