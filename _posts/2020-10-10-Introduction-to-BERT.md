---
layout: post
title: Introduction to Deep Bidirectional Transformers for Language Understanding (BERT)
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [NLP, ELMo]
mathjax: true
summary: Introduction to BERT
---


# Bidirectional Transformers (BERT)

### Overview
1. Definition





### Definition
In this article we introduce a language representation model called **BERT**, wich stands
for **B**idirectional **E**ncoder **R**epresentations from **T**ransformers.

It is a fine-tuning approach, introduces minimal task-specific parameters, and
is trained on the downstream tasks by simply fine-tuning all pre-trained parameters.

BERT is designed to pretrain deep bidirectional representations from unlabeled text
by jointly conditioning on both left and right context in all layers.

As a result, the pre-trained BERT model can be fine-tuned
with just one additional output layer to create state-of-the-art (SOTA) models
for a wide range of NLP tasks, such as question answering and 
language inference, without substantial task-specific architecture modifications.

### Background
Current techniques restrict the power of the pre-trained representations of the 
pre-trained representations, especially for fine-tuning approaches.

The major limitation is that standard language models are unidirectional, and this limits
the choice of architectures that can be used during pre-training.

BERT alleviates the mentioned unidirectionality constraint by using a 
"masked language model" (MLM) pre-training objective. The masked language model randomly masks
some of the tokens from the input, and the objective is to predict the original vocabulary id of the masked
word based only on its context. The MLM objective enables the representation to 
fuse the left and the right context, which allows us to 
pretrain a deep bidirectional Transformer. In addition, we also use a 
"next sentence prediction" task that jointly pretrains text-pair representations.


### How BERTS works / Model Architecture
BERT makes use of Transformer (the attention mechanism that allows to learn contextual 
relations between words (sub-words) in a text). A basic Transformer consists of an encoder to read the text input 
and a decoder to produce a prediction for the task. Since BERT’s goal is to generate a language representation model, 
it only needs the encoder part. The input to the encoder for BERT is a sequence of tokens, which are first converted 
into vectors and then processed in the neural network. For more details, look at the article about Transformers. 
But before processing can start, BERT needs the input to be massaged and decorated with some extra metadata: 

1. **Token embeddings:**
A [CLS] token is added to the input word tokens at the beginning of the first sentence and a [SEP] token is inserted
at the end of each sentence.
2. **Segment embeddings:**
A marker indicating Sentence A or Sentence B is added to each token. This allows the encoder to 
distinguish between sentences.
3. **Positional embeddings:**
A positional embedding is added to each token to indicate its position in the sentence.

![]({{ site.url }}/assets/img/posts/BERT.png)


#### Masked Language Model (MLM)
Before feeding word sequences into BERT, 15% of the words in each sequence are replaced with a [MASK] token.
The model then attempts to predict the original value of the masked words, based on the context
provided by the other, non-masked, words in the sequence. In technical terms, the prediction of the output words requires:
1. Adding a classification layer on top of the encoder output.
2. Multiplying the output vectors by the embedding matrix, transforming them into the vocabulary dimension.
3. Calculating the probability of each word in the vocabulary with softmax.


![]({{ site.url }}/assets/img/posts/BERT_MLM.png)

[image]

The BERT loss function takes into consideration only the prediction of the masked values and ignores the prediction
of the non-masked words. As a consequence, the model converges slower than directional models, a characterstic which is 
offset by its increased context awareness


An example for MLm: Assuming the unlabeled sentence is *my girlfriend is pretty*, and during the random masking procedure we 
chose the 4-th token (which corresponding to *pretty*), our masking procedure can be further illustrated by

- 80% of the time: Replace the word with the [MASK] token, e.g.,
*my girlfriend is pretty -> my girlfriend is [MASK]*

- 10% of the time: Replace the word with a random word, e.g., 
*my girlfriend is pretty -> my girlfriend is apple*

- 10% of the time: Keep the word unchanged, e.g., 
*my girlfriend is pretty -> my girlfriend is pretty.*
The purpose of this is to bias the representation towards the actual observed word.

The advantage of this procedure is that the Transformer encoder does not know 
which words it will be asked to predict or which have been replaced by random words,
so it is forced to keep a distributional contextual representation of *every*
input token. Additionally, because random replacement only occurs for 1.5% of all tokens
(i.e. 10% of 15%), this does not seem to harm the model's language
understanding capability.

Compared to standard LM training, the MLM only make predictions on 15% of 
tokens in each batch, which suggests that more pre-training steps may be 
required for the model to converge.

#### Next Sentence Prediction (NSP)

The next sentence prediction task can be illustrated in the following examples

Input = *[CLS] the man went to [MASK] store [SEP] he bought a gallon [MASK] milk [SEP] *
Label = *IsNext*

Input = *[CLS] the man [MASK] to the store [SEP] penguin [MASK] are flight ##less birds [SEP]
Label = *NotNext*


The first input token is supplied with a special [CLS] token for reasons that will become apparent later on.
CLS here stands for Classification.

### Papers
- BERT: Pre-training of Deep Bidirectional Transformers forLanguage Understanding: https://arxiv.org/pdf/1810.04805.pdf


### Other Sources
- https://towardsdatascience.com/bert-explained-state-of-the-art-language-model-for-nlp-f8b21a9b6270
- http://jalammar.github.io/illustrated-bert/
- https://keras.io/examples/nlp/masked_language_modeling/
- https://jalammar.github.io/a-visual-guide-to-using-bert-for-the-first-time/


### Examples for using BERT in NLP tasks
- Question Answering (SQuAD)
- Natural Language Inference (MNLI)
- Sentiment Sentence Classifcation
