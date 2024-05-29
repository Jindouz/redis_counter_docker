# Redis Counter Application

This project is a simple Flask web application that uses Redis to keep track of the number of visits to the homepage.

## Prerequisites

- Docker
- Docker Compose

## Project Structure


├── app.py  
├── Dockerfile  
├── docker-compose.yml  
├── requirements.txt  
└── README.md  


- `app.py`: The main Flask application file.
- `Dockerfile`: The Dockerfile to build the web application image.
- `docker-compose.yml`: Docker Compose configuration to set up the Redis and web services.
- `requirements.txt`: Python dependencies file.
- `README.md`: Project documentation.

## Getting Started

### 1. Clone the repository

```
git clone https://github.com/Jindouz/redis_counter_docker.git
```

### 2. Build and start the containers
```
docker-compose up --build
```
### 3. Access the application
Open your web browser and go to http://localhost:5000. You should see a message displaying the number of times the page has been viewed.

## Project Details
app.py  
This file contains the Flask application code:  

### Python

app.py  
```
from flask import Flask
from redis import Redis

app = Flask(__name__)
redis = Redis(host='redis', port=6379)

@app.route('/')
def hello():
    redis.incr('hits')
    counter = str(redis.get('hits'), 'utf-8')
    return "Welcome to RedisCounter!, This webpage has been viewed " + counter + " time(s)"

if __name__ == "__main__":
    app.run(host="0.0.0.0", debug=True)
```

### Dockerfile  
This file defines the Docker image for the web application:

dockerfile
```
FROM python:3.12-alpine
WORKDIR /code
COPY requirements.txt /code
RUN pip install -r requirements.txt --no-cache-dir
COPY . /code
CMD ["python", "app.py"]
```
### Docker Compose
This file defines the services for Docker Compose:  

docker-compose.yml  
```
services:
   redis:
     image: redislabs/redismod
     container_name: redis
     ports:
       - '6379:6379'
   
   web:
     build: .
     container_name: web
     ports:
       - "5000:5000"
     volumes:
       - .:/code
     depends_on:
       - redis
```
### Requirements
This file lists the Python dependencies:

requirements.txt   
```
Flask
redis
```

