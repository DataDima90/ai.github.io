---
layout: post
title: Convolutional Neural Network
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [Image Classification, Deep Learning]
mathjax: true
summary: Mathematical Theory
---


```python
import pandas as pd
```


![]({{ site.url }}/assets/img/posts/emile-perron-190221.jpg)
[![Test Logo](https://github.com/datadima90/datadima90.github.io/assets/img/posts/emile-perron-190221.jpg)]

<img src="https://github.com/datadima90/datadima90.github.io/assets/img/posts/emile-perron-190221.jpg">

#Convolutional Neural Networks (CNN)
CNNs are a bit different to fully connected neural networks:
1. The layers are organised in 3 dimensions: width, height and depth.
2. The neurons in one layer do not connect to all the neurons in the next layer but
only to a small region of it.
3. The final output will be reduced to a single vector of probability scores, 
organized along the depth dimension.


A CNN is composed of two major parts:
- Feature Extraction: Input is the image which we want to classify and the output is a set of features.
For instance, an image of a cat might have features like whiskers, two ears, four legs, etc.
A handwritten digit image might have features as horizontal and vertical lines or loops and curves.


- Classification: The fully connected layers will serve as a classifier on top of these extracted features.
They will assign a probability for the object on the image being what the algorithm predicts it is.


### Feature Extraction

#### Convolution

### Classification



### Limitations of CNNs
- There is no encoding of pose and orientation of the image.
- Orientation and placements of various parts of the objects in the image are not taken into account.
- Routing of high dimensional data to specific neurons to improve predictions.
