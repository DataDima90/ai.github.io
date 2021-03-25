---
layout: post
title: How to build a production-ready Machine Learning Microservice
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [Microservice, Docker, Flask, Python]
mathjax: true
summary: Building a Machine Learning Microservice using Flask, Gunicorn, Nginx and Docker
---

# Building a Machine Learning Microservice using Flask, Gunicorn, Nginx and Docker

Deployment of machine learning models is the process of making trained models available in production. In this post we will learn how machine learning models c****an be deployed in a dockerized environment using Flask, Gunicorn and Nginx in Python environment. The adavantages of deploying machine learning models in a container as microservices are simple: It improves **scalability**, **fault isolation** and enhances **speed**. 

We will cover the following frameworks and topics:
- **Flask** is a popular Python framework for making web *APIs* which is popular amongst the machine learning community.
- **Gunicorn** is a Python WSGI HTTP server and a common choice for self-hosting Flask application in production. Flask has a built-in WSGI (Server Gateway Interface) web server, but it is not secure or efficient and hence should NOT be used for production.
- **Nginx** is our web server, which will handle all requests and act as a load balancer for the application.
- **Docker** is a great way to make the API easy to deploy on any server. It is easily customizable to run with any configuration. Moreover, we can combine multiple instances of the docker container when we want to scale up our Flask API.

The whole code in this post is available on my GitHub.

Let's get started.

## Table of Contents

This Page is divded into following parts:

1. [Prerequisites](#pre)
1. [Building API using Python and Flask](#api)
2. [Using Gunicorn WSGI](#guni)
3. [Dockerization](#docker)

<a name="pre"></a>
## 1. Prerequisites

```bash
pip install flask
```

<a name="api"></a>
## 1. Building our API using Python and Flask

If we want to make our trained machine learning model available to other people, we have to use and write APIs. **Flask** is one of the most popular framework for making APIs easily and fast.

The app.py is a python script which contains the API we built for our Machine Learning model using flask. We defined the API endpoint and the path, how we receive data from the web application, how the data is being processed and how predictions are being returned as a response.

```python
# api/app.py

from flask import Flask
import pickle

# Create an instance of the Flask class with the default __name__
app = Flask(__name__)

# Path of our saved trained model as pickle file
MODEL_PATH = "models/model.pkl"


# URL of our API for our ml model
@app.route('/prediction')
def prediction():
    # Get all necessary features
    sample = [1.0, 2.0, 3.0, 4.0]

    # Load our ml model
    model = pickle.load(open(MODEL_PATH, 'rb+'))

    # Predict the species
    species = model.predict([sample]).tolist()[0]

    # Convert species class into class name
    if species == 0:
        s = "Setosa"
    elif species == 1:
        s = "VersiColor"
    else:
        s = "Virginica"

    return {"prediction": s}


if __name__ == '__main__':
    # Set localhost with port 8080
    app.run(host="0.0.0.0", port="8080")

```




<a name="guni"></a>
## 2. Test






<a name="docker"></a>
## 3. Dockerization

Docker is available across various platforms whether if you are using a Linux, Mac or a Windows computer, you can follow the installation guide here.





## Using Gunicorn WSGI for production
Gunicorn is a necessary componenent for getting Flask into production. 
WSGI stands for "Web Server Gateway Interface". A WSGI is the middleman between our Flask application and our web server. Flask has a build-in WSGI but it is not build for production.Instead, Flask is designed to be used with other WSGI-compliant web server. For this post, (and in the template) we will use Gunicorn. It's a solid piece of kit. However, it isn't the only option, there's **twisted** and **uWSGI** too, for example. 



Installing Gunicorn by typing command:

```bash
pip install gunicorn
```

Next, we create a wsgi.py file and add the following code:

```python
from app import app


if __name__ == '__main__':
    app.run(use_reloader=True, debug=True)
```
The wsgi.py is typically referred to as a WSGI entrypoint. What is WSGI, you ask? It stands for 'Web Server Gateway Interface', and – in short – it's a specification defining how a web server can interact with Python applications.

Now, instead of starting our API by running python app.py, we will use now this:

```bash
$ gunicorn -w 3 -b :8080 -t 30 --reload wsgi:app
```

When we run this command, we start app.py using the app from the wsgi.py. We are also asking Gunicorn to setup 3 worker threads, set the port to 8080, set timeout to 30 secondes, and allow hot-reloading the app.py file, so that it rebuilds immediatly if the code in app.py changes.



## Nginx 
Nginx is our web server, which will handle all requests and act as a load balancer for the application.

In our nginx folder, we create two files. First file, nginx.conf, contains:

```python
worker_processes  3;

events { }

http {

  keepalive_timeout  360s;

  server {

      listen 8080;
      server_name api;
      charset utf-8;

      location / {
          proxy_pass http://api:5000;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      }
  }
}
```


Our Dockerfile for nginx:

```
FROM nginx:1.15.2

RUN rm /etc/nginx/nginx.conf
COPY nginx.conf /etc/nginx/
```

**Description:** Creates a Docker containter with the nginx image, then copies the nginx config to it.




- Docker is a great way to make the API easy to deploy on any server. 




## ML_Model
The **ML_Model** directory contains the ML model code and the pickle file generated after model is being trained which the API will make use of.

## requirements.txt
```python
scipy==1.6.1
pandas==1.2.3
numpy==1.20.1
Flask==1.1.2
scikit-learn==0.24.1
```


## Dockerfile
A Dockerfile is a text file that defines a Docker Image. You will use a Dockerfile to create your own custom Docker image when the base image you want to use for your project does not meet your required needs.

This is how our Dockerfile looks like:

```python
# We specify the parent base image which is the python version 3.8
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
CMD [ "app.py","run","--host"]
```
In our Dockerfile, we pulled the Docker base image which is python:3.8, updated the system dependencies, installed the packages in the requirements.txt file, ran the ML code to train the model and generate the pickle file which the API will use and lastly run the server locally.

Now let's build our Docker image from the Dockerfile we have created using this command:
```bash
docker build -t model:1.0 .
```


If successful you should see a result like this:

```bash
 * Serving Flask app "app" (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:8080/ (Press CTRL+C to quit)
```
To check if your docker container is running, use this command:

```bash
docker ps
```

WOW! We have successfully deployed our Machine Learning model using docker.

The code can be found here.

Source:
- https://hackernoon.com/a-guide-to-scaling-machine-learning-models-in-production-aa8831163846
- 

