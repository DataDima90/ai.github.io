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
A Dockerfile is a text file that defines a Docker Image. You will use a Dockerfile to create your own custom Docker image when the base image you want to use for your project does not meet your required needs.

This is how our Dockerfile looks like:

```python
#We specify the parent base image which is the python version 3.8
FROM python:3.8

MAINTAINER Dima Wilhelm <dima.wilhelm@gmx.de>

# This prevents Python from writing out pyc files
ENV PYTHONDONTWRITEBYTECODE 1

# This keeps Python from buffering stdin/stdout
ENV PYTHONUNBUFFERED 1

# install system dependencies
RUN apt-get update \
    && apt-get -y install gcc make \
    && rm -rf /var/lib/apt/lists/*

# install dependencies
RUN pip install --no-cache-dir --upgrade pip

# set work directory
WORKDIR /src/app

# copy requirements.txt
COPY ./requirements.txt /src/app/requirements.txt

# install project requirements
RUN pip install --no-cache-dir -r requirements.txt

# copy project
COPY . .

# Generate pickle file
WORKDIR /src/app
RUN python model_iris.py

# set work directory
WORKDIR /src/app

# set app port
EXPOSE 8080

ENTRYPOINT [ "python" ]

# Run app.py when the container launches
CMD [ "app.py","run","--host","0.0.0.0"]
```
In our Dockerfile, we pulled the Docker base image which is python:3.8, updated the system dependencies, installed the packages in the requirements.txt file, ran the ML code to train the model and generate the pickle file which the API will use and lastly run the server locally.

Now let's build our Docker image from the Dockerfile we have createdusing this command:
```bash
docker build -t model:1.0 .
```


