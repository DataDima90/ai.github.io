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

Deployment of machine learning models is the process of making trained models available in production. Here we will learn how machine learning models can be deployed in a dockerized environment using Flask, Gunicorn and Nginx in Python environment. The adavantages of deploying machine learning models in a container as microservices are simple: It improves **scalability**, **fault isolation** and enhances **speed**. 

We will cover the following frameworks and topics:
- **Flask** is a popular Python framework for making web *APIs* which is popular amongst the machine learning community.
- **Gunicorn** is a Python WSGI HTTP server and a common choice for self-hosting Flask application in production. Flask has a built-in WSGI (Server Gateway Interface) web server, but it is not secure or efficient and hence should NOT be used for production.
- **Nginx** is our web server, which will handle all requests and act as a load balancer for the application.
- **Docker** is a great way to make the API easy to deploy on any server. It is easily customizable to run with any configuration. Moreover, we can combine multiple instances of the docker container when we want to scale up our Flask API. Docker is available across various platforms whether if you are using a Linux, Mac or a Windows computer, you can follow the installation guide here.
- **Docker-Compose** is a great way for defining and running multi-container Docker applications. With Compose, we will use a YAML file to configure our application’s services. Then, with a single command, we create and start all the services from our configuration. 
- **Pytest** helps us to write better code because it allows us to write tests fast and reliable. MOreover it reduces boilerplate code to the minimum and still remains readable. In our case we wanna write unit tests for our Flask API. 

What we will do:

1) How to write a simple, easily extendable Flask API and make it modular using Flask Blueprints
2) How to make our Flask API for production using Gunicorn WSGI
3) How to set up the Flask API and Nginx inside docker
4) How tw assemble our Docker Containers using docker-compose
5) How to test our Flask API using pytest

The whole code in this post is available on my GitHub.

Let's get started.

## Table of Contents

This Page is divided into following parts:

1. [Prerequisites](#pre)
2. [Building API using Python and Flask](#api)
3. [Using Gunicorn WSGI for production](#guni)
4. [Dockerization of our API](#docker)
5. [Dockerization of our Nginx web server](#nginx)
6. [Assembling our Docker Containers using Docker-Compose](#compose)
7. [Testing our ML API using pytest](#testing)

<a name="pre"></a>
## 1. Prerequisites

Let's make sure we have Docker and `docker-compose` installed. 

**This is how our project looks like:**

```
flask-ml-api
| - api
    | - __init__.py
    | - endpoints
        | __init__.py
        | - prediction.py
    | - models
        | - model.pkl
    | - tests
        | - __init__.py
        | - test_prediction.py
    | - app.py
    | - wsgi.py
    | - requirements.py
    | - Dockerfile
    | - run.sh
| - nginx
    | - nginx.conf
    | - Dockerfile
| - docker-compose.yml
```

As you can see, the `api` and `nginx` live in different Docker containers. We will be using docker-compose to connect the two, so that **Nginx** will handle requests to the API via **Gunicorn**.

Install all packages we require with our `requirements.txt` file:

```bash
Flask==1.1.2
pytest==6.2.2
gunicorn==19.10.0
```

As you can see, we have already a trained model called `model.pkl` and saved in `models` folder. FYI: This model is a classifier with 3 classes and trained on iris dataset, i.e. it has 4 input features.

<a name="api"></a>
## 2. Building a simple, easily extendable API using Python and Flask

If we want to make our trained machine learning model available to other people, we have to use and write APIs. **Flask** is one of the most popular framework for building APIs with Python.

Let's see how we can build an endpoint:

```python
# api/endpoints/prediction.py

from flask import Blueprint
import pickle

prediction_api = Blueprint('prediction_api', __name__)

# load our ML model
MODEL = pickle.load(open("./models/model.pkl", 'rb+'))


@prediction_api.route('/prediction')
def prediction():
    # sample feature values
    sample = [1.0, 2.0, 3.0, 4.0]

    # predict the species
    species = MODEL.predict([sample]).tolist()[0]

    # convert species class into class name
    if species == 0:
        s = "Setosa"
    elif species == 1:
        s = "VersiColor"
    else:
        s = "Virginica"

    return {"prediction": s}
```

As you can see, we need to initializie our `prediction` API as a blueprint called `prediction_api`. After we have set up our first endpoint, we can add as many new endpoints as we like. In this way we can maintain each endpoint seperate in their own file easily. That's it, the prediction API is set up! 

Finally, we can simply import the blueprint `prediction_api` and use it in our Flask `app.py`:

```python
# api/app.py

from flask import Flask
from endpoints.prediction import prediction_api

# Create an instance of the Flask class with the default __name__
app = Flask(__name__)

# Register our endpoint
app.register_blueprint(prediction_api)

if __name__ == '__main__':
    # listen on port 8080
    app.run(host="0.0.0.0", port=8080, debug=True)

```
Now, when we start **Flask**, it will register the prediction blueprint and enable the `/prediction` endpoint. That's it!

Let's test our api. Go to your `/api` directory and run `app.py` to start the API on localhost:

```python
python app.py
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

You will find the localhost URL with port in the above terminal output of this command.

Our URL is: **GET** 0.0.0.0:8080/prediction

The request body looks like:

```
{
  "prediction": "Virginica"
}
```

To query and test the API you can use Postman.



<a name="guni"></a>
## 3. Using Gunicorn WSGI for production

Gunicorn is a necessary componenent for getting Flask into production. A WSGI is the middleman between our Flask application and our web server. Flask has a build-in WSGI but it is not build for production. Instead, Flask is designed to be used with other WSGI-compliant web server. In this template we will use Gunicorn. It's a solid piece of kit. However, it isn't the only option, there's **twisted** and **uWSGI** too, for example. 

Let's create a wsgi.py file and add the following code:

```python
# api/wsgi.py

from app import app


if __name__ == '__main__':
    app.run(use_reloader=True, debug=True)
```
The wsgi.py is typically referred to as a WSGI entrypoint.

Now, instead of starting our API by running python app.py, we will use now this:

```bash
$ gunicorn -w 3 -b :8080 -t 30 --reload wsgi:app
```

Instead of typing the above long code we can create a `run.sh` file and copy the above code:

```bash
# api/run.sh

gunicorn --workers 8 --bind 0.0.0.0:8080 wsgi:app --timeout 5  --log-level info
```

Now, we can run the code with following command:

```bash
bash run.sh
```

When we run this command, we start app.py using the app from the wsgi.py. We are also asking Gunicorn to setup 3 worker threads, set the port to 8080, set timeout to 30 secondes, and allow hot-reloading the app.py file, so that it rebuilds immediatly if the code in app.py changes.

<a name="docker"></a>
## 4. Dockerization of our API

A Dockerfile is a text file that defines a Docker Image. We will use a Dockerfile to create our own custom Docker image because the base image we want to use for our project does not meet our required needs.

This is how our Dockerfile looks like:

```python
# api/Dockerfile
FROM python:3.8

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
WORKDIR /src/api

# copy requirements.txt
COPY ./requirements.txt /src/api/requirements.txt

# install project requirements
RUN pip install --no-cache-dir -r requirements.txt

# copy project
COPY . .

# Generate pickle file
WORKDIR /src/api

CMD ["gunicorn", "-w", "3", "-b", ":5000", "-t", "30", "--reload", "api.wsgi:app"]
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

<a name="nginx"></a>
## 5. Dockerization of our Nginx web server

Nginx is our web server, which will handle all requests and act as a load balancer for the application.

In our nginx folder, we have two files. First file, nginx.conf, contains:

```python
# nginx/nginx.conf

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

```bash
# nginx/Dockerfile

FROM nginx:1.15.2

RUN rm /etc/nginx/nginx.conf
COPY nginx.conf /etc/nginx/
```


<a name="compose"></a>
## 6. Docker Compose
Finally, ware ready to create the most important file that will manage our two Docker containers

In the root folder of the project, ad following code in `docker-compose.yml`:

```bash
# docker-compose.yml

version: '3'

services:

  api:
    container_name: flask-ml-api
    restart: always
    build: ./src/api
    volumes: ['./src/api:/src/api']
    networks:
      - apinetwork
    expose:
      - "5000"
    ports:
      - "5000:5000"

  nginx:
    container_name: nginx
    restart: always
    build: ./nginx
    networks:
      - apinetwork
    expose:
      - "8080"
    ports:
      - "80:8080"

networks:
  apinetwork:
```

In our docker-compose we have two services (containers), one for our `api` folder and another for our `nginx` folder. Our network is arbitrarily named `apinetwork`

**Our `api` service:**
- name of our container is `flask-ml-api`
- `restart: always` allows our api to hot reload when files change
- we build the `/api` folder
- `volumes: ['./src/api:/src/api']` is necessary to keep our local and docker api folders in sync. This is also necessary step for hot-reloading when we update our `app.py` file.
- we makr this container to be part of the `apinetwork`, and expose port 5000, where `5000:5000` says that port 5000 in our Docker ocntainer will be same as 5000 on our local.

**Our `nginx` service:**

We do the same setup as our api, but we expose 8080, where `80:8080` means that port 8080 of our Docker container is accessed on port 80 locally.

**Our connection flow looks like this:**

1) We query on port 80 on our localhost, which is sent on port 8080 on our apinetwork to nginx (remember, nginx is listening on port 8080)
2) nginx transfers this request to port 5000 on the apinetwork (which is where Gunicorn will recieve the request)
3) Gunicorn passes this request to Flask


Let's run the following on our terminal to start docker-compose, setup the containers and start the API:

```bash
$ docker-compose build
$ docker-compose up
```
If successful, we should be able to access our API on localhost at port 80.

To query the API on **GET** `0.0.0.0/prediction` we can use Postman.

<a name="testing"></a>
## 7. Testing our ML API using pytest



WOW! We have successfully deployed our Machine Learning model as a production-ready Microservice using Flask, Gunicorn, Nginx and Docker.


Source:
- https://hackernoon.com/a-guide-to-scaling-machine-learning-models-in-production-aa8831163846
- 

