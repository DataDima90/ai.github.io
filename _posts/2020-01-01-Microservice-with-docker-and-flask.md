---
layout: post
title: How to deploy Machine Learning models as a Microservice using Flask and FastAPI
featured-img: shane-rounce-205187
image: shane-rounce-201587
categories: [Microservice, Docker, Flask, FastAPI, Python]
mathjax: true
summary: Microservices with Docker and Flask
---

# Microservice

If you want to deploy your machine learning model into production then you can use a popular tool named **Docker** 
to run and also deploy your Machine Learning model.


## Installing Docker
Docker is available across various platforms whether if you are using a Linux, Mac or a Windows computer, you can follow the installation guide here.

## Content

For the machine learning we wanna deploy, we need the following things:

- Our machine learning model: model.py 
- Our API using Flas fastAPI): app.py
- Our custom Docker Image for our web application with machine learning model: Dockerfile

# Step 2. Create API
Python is one of the most popular programming languages. Its popularity is fueled by the tools it offers. Flask, a web framework, is one such tool, which is popular amongst the machine learning community. It's also widely used for API development. But there's a new framework on the rise: FastAPI. Unlike Flask, FastAPI is an ASGI (Asynchronous Server Gateway Interface) framework. On par with Go and NodeJS, FastAPI is one of the fastest Python-based web frameworks.

## Flask
Flask is a Python-based framework that allows you to hook up websites with less amount of code. You can create a small-scale website with this as it allows customization at every step. Being a minimalistic package, only core components are bundled with this and all other extensions require explicit setup. Flask is used by many developers to host their APIs. API (Application Program Interface) is an interface that allows communication between multiple intermediaries meaning that one can access any type of data using any technology. For instance, you can access an API using Javascript which could be built using Python.

## Problems with Flask framework

### 1. No Data Validation
The problem with flask is that there is no data validation, meaning, that we can pass any type of data being it string, tuple, numbers, or any character. This can break the program often and you can imagine if an Machine Learning model getting wrong data types, the program will crash. You can create a data checker before passing the values further but it would add up additional work.

### 2. Decoder Errors
The error pages in Flask as simple HTML pages that can raise decoder errors when the API is being called in other applications. 

### 3. Other issues
There are other issues with Flask such as slow nature, no async, and web sockets support that can speed up the processes, and finally no automated docs generation system. You need to manually design the user interface for the usage and examples of the API. All these issues are resolved in the new framework.

## FastAPI
FastAPI is a modern framework that allows you to build APIs seamlessly without much effort. It has the ability to separate the server code from the business logic increasing code maintainability. As the name itself has fast in it, it is much faster as compared to the flask because it’s built over ASGI (Asynchronous Server Gateway Interface) instead of WSGI (Web Server Gateway Interface). It has a data validation system that can detect any invalid data type at the runtime and returns the reason for bad inputs to the user in the JSON format only which frees developers from managing this exception explicitly.

It generates the documentation on the go when you are developing the API which is the most requested thing from all the developers. Documentation is a great way for other developers to collaborate on a project as it presents them with everything that can be done with the necessary instructions. It also generates a nice GUI which solves everything that was missing in the flask.

**Our Recommendations:**

- Use **Flask** if you are not comfortable with the maturity-level of FastAPI, need to set up the whole web interface (Front-end and back-end) with server-side templating, or can not live without some of the community-matinained Flask extensions.
- 
- Use **FastAPI** if the main goal is to check if the model is working in the production environment or not, creating an API makes more sense because the rest of the things can be managed by other teams of developers and to clearly explain them the usage of the program you developed, FastAPI auto docs is a good solution.

## Building our API using Flask
The app.py is a python script which contains the API we built for our Machine Learning model using flask. We defined the API endpoint and the path, how we receive data from the web application, how the data is being processed and how predictions are being returned as a response.

```python
from flask import Flask, request, render_template
import pickle

app = Flask(__name__, template_folder='templates', static_folder='static')

# Global Variable
MODEL_PATH = "models/model.pkl"


@app.route('/')
def main():
    return render_template('index.html')


@app.route('/predict', methods=['POST'])
def index():
    # Get all necessary features
    features = [float(x) for x in request.form.values()]

    if None not in features:
        # Load our ml model
        model = pickle.load(open(MODEL_PATH, 'rb+'))

        # Predict the species
        species = model.predict([features]).tolist()[0]

        # Convert species class into class name
        if species == 0:
            s = "Setosa"
        elif species == 1:
            s = "VersiColor"
        else:
            s = "Virginica"

        return render_template('index.html', prediction_text='The species of the iris plant is {}'.format(s))


if __name__ == '__main__':
    # listen on port 8080
    app.run(host="0.0.0.0", port=8080, debug=False)
```

## Building our API using FastAPI

- Flask is a popular Python framework for making web APIs. 



## Using Gunicorn WSGI for production
Gunicorn is a necessary componenent for getting Flask into production
WSGI stands for "Web Server Gateway Interface". A WSGI is the middleman between our Flask application and our web server. Flask has a build-in WSGI but it is not build for production. Gunicorn is one of the few options of WSGI we can easily setup and use with Flask.

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

