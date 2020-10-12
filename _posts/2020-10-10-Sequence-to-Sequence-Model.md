---
layout: post
title: Sequence-to-Sequence Models
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [NLP, Sequence-to-Sequence]
mathjax: true
summary: Introduction to Seq2Seq Models
---


# Sequence-to-Sequence Models
Most of the data in the current world are in the form of sequences -
it can be a number sequence, text sequence, a video frame sequence or an audio sequence.

### Overview
- Definition
- Examples
- Description
- Sources


### Definition
Seq2Seq models in NLP are used to convert sequences of Type A to sequences of Type B.

Recurrent Neural Networks (RNN) based seq2seq models.


### Model Architecture
[image]

### Examples
The Seq2Seq models are used in a variety of NLP tasks, such as:

- Machine Translation: e.g. translation of English sentences to German sentences
- Text Summarization
- Speech Recognition
- Question-Answering System
- etc.

### Challenges/Limitations

- Dealing with long-range dependencies is still challenging
- The sequential nature of the model architecture prevents parallelization.
These challenges are addressed by Google Brain's Transformer concept

### Description of the model
Recurrent Neural Networks (RNN) based seq2seq models can be break down as follows:

- Both Encoder and Decoder are RNNs
- At every time step in the Encoder, the RNN takes a word vector (x_i) from the input sequence
and a hidden state (H_i) from the previous time step
- The hidden state is updated at each time step
- The hidden state from the last unit is known as the context vector.
This contains information about the input sequence.
- This context vector is then passed to the decoder and it's then used to generate the target sequence.


### Papers
-

### Other Sources
- https://www.analyticsvidhya.com/blog/2019/06/understanding-transformers-nlp-state-of-the-art-models/