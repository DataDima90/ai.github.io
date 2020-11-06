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

### WHY was BERT needed?
One of the biggest challenges in NLP is the lack of enough training data. Overall there is enormous amount of text data 
available, but if we want to create task-specific datasets, we need to split that pile into the very many diverse fields. 
And when we do this, we end up with only a few thousand or a few hundred thousand human-labeled training examples. 
Unfortunately, in order to perform well, deep learning based NLP models require much larger amounts of data —  
they see major improvements when trained on millions, or billions, of annotated training examples. To help bridge 
this gap in data, researchers have developed various techniques for training general purpose language representation 
models using the enormous piles of unannotated text on the web (this is known as **pre-training**). These general purpose 
pre-trained models can then be **fine-tuned** on smaller task-specific datasets, e.g., when working with problems 
like question answering and sentiment analysis. This approach results in great accuracy improvements compared to 
training on the smaller task-specific datasets from scratch. BERT is a recent addition to these techniques for NLP 
pre-training; it caused a stir in the deep learning community because it presented state-of-the-art results in a 
wide variety of NLP tasks, like question answering.


### Background / What is the core idea behind BERT?
Basically, Language models are trying to "fill in the blank" based on context. For example, given

"The woman went to the store and bought a ______ of shoes."

a language model might complete this sentence by saying that the word "cart" would fill the blank 20%
of the time and the word "pair" 80% of the time.

BERT is a language model which is bidirectionally trained or might be more accurate to say that 
BERT is non-directional though. This means we can now have a deeper sense of language context and flow compared to the 
single-direction language models.

Instead of predicting the next word in a sequence, BERT makes use of a novel technique called **Masked LM** (MLM):
it randomly masks words in the sentence and then it tries to predict them. Masking means that the model
looks in both directions and it uses the full context of the sentence, both left and right surroundings, in order to
predict the masked word. Unlike the previous language models, it takes both the previous and next tokens into account 
**at the time**. The existing combined left-to-right and right-to-left LSTM based models were missing
this "same-time part". It mi 

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

The input representation for BERT: The input embeddings are the sum of the **token embeddings**, **the
segmentation embeddings** and the **position embeddings**.

Essentially, the Transformer stacks a layer that maps sequences to sequences, so the output is also 
a sequence of vectors with a 1:1 correspondence between input and output tokens at the same index. 
And as we learnt earlier, BERT does not try to predict the next word in the sentence. 
Training makes use of the following two strategies:

We can say that BERT is basically a trained Transformer Encoder stack.

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

While training the BERT loss function considers only the prediction of the masked tokens and ignores 
the prediction of the non-masked ones. This results in a model that converges much more slowly 
than left-to-right or right-to-left models.

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

Input = *[CLS] the man went to [MASK] store [SEP] he bought a gallon [MASK] milk [SEP]*
Label = *IsNext*

Input = *[CLS] the man [MASK] to the store [SEP] penguin [MASK] are flight ##less birds [SEP]*
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
- https://towardsml.com/2019/09/17/bert-explained-a-complete-guide-with-theory-and-tutorial/


BERT can be used either for extracting high quality language features from text data,
or for fine-tuning these models on a specific task.

### Example applications of BERTs in NLP tasks
- Question Answering (SQuAD)
- Natural Language Inference (MNLI)
- Sentiment Analysis
- others