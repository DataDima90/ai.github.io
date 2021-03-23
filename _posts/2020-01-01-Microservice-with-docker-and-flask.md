---
layout: post
title: Microservices
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [Microservice, Docker, Flask, Python]
mathjax: true
summary: Microservices with Docker and Flask
---

# Microservice

If you want to deploy your machine learning model into production then you can use a popular tool named **Docker** 
to run and also deploy your Machine Learning model.


For the machine learning we wanna deploy, we need the following things:

- API using Flask (or fastAPI)


```python
from flask import Flask, request

import numpy as np
import pickle
import json

flask_app = Flask(__name__)

# Path of the ML model

```