# Docker笔记-使用Dockerfile构建Django镜像的简单示例


### Dockerfile

```
FROM python:3.8.2
ENV PYTHONUNBUFFERED 1
RUN mkdir /code
WORKDIR /code
COPY requirements.txt /code/
RUN pip install -r requirements.txt
COPY . /code/
```
RUN `docker build .`

### docker-compose.yml
```yml
version: '3'

services:
  web:
    build: .
    command: python manage.py runserver 0.0.0.0:8080
    volumes:
      - .:/code
    ports:
      - "8080:8080"
```

RUN `docker-compose up -d`
