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

### ML_Model
The **ML_Model** directory contains the ML model code and the pickle file generated after model is being trained which the API will make use of.

### requirements.txt
```python
scipy==1.6.1
pandas==1.2.3
numpy==1.20.1
Flask==1.1.2
scikit-learn==0.24.1
```


### Dockerfile
This is how the Dockerfile looks like:
```python

```
In our Dockerfile, we pulled the Docker base image which is python:3.8, updated the system dependencies, installed the packages in the requirements.txt file, ran the ML code to train the model and generate the pickle file which the API will use and lastly run the server locally.

Now let's build our Docker image from the Dockerfile we have createdusing this command:
```bash
docker build -t model:1.0 .
```


