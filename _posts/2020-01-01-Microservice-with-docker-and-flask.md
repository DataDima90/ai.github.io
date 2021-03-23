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

- Our machine learning model: model.py 
- Our API using Flask (or fastAPI): app.py
- Our custom Docker Image for our web application with machine learning model: Dockerfile

## Building our API using Flask
The app.py is a python script which contains the API we built for our Machine Learning model using flask. We defined the API endpoint and the path, how we receive data from the web application, 

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
    app.run(port=8080, debug=False)
```

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


