<h2 align="center">Лабораторная работа №4. Работа с портáми и томáми</h3>

<h3 align="center">I. Пошаговое создание контейнеров nginx и wsgi</h3>

1. docker run: Создание контейнера nginx
 

    docker run -d --name nginx-container -p 8081:80 nginx

2. docker run: Создание контейнера c wsgi-сервером, установка Flask
 

    docker run -it --name wsgi-container -p 8082:80 python:3.9-slim bash
    pip install flask

3.  Устанавливаем в контейнер текстовый редактор nano
     
    
        apt update
        apt install nano
    
4.  Создаем Flask проект
         
        
             nano app.py
             from flask import Flask
             app = Flask(__name__)
             
             @app.route("/")
             def hello():
                 return "Hello World!"
        
5.  Запускаем проект
             
    ![image](https://github.com/ruslan0003/lab4/blob/main/lab4-1.jpg)
            
             
            
<h3 align="center">II. Создание образа nginx из Dockerfile (docker build),<br>создание образа WSGI-сервером на Python</h3>
            
Содержимое файлов Dockerfile
             
            
                #NGINX
                FROM nginx:alpine
                
                #Python
                FROM python:3.9-slim
                WORKDIR /app
                COPY ./app.py /app/
                RUN pip install flask
                CMD ["flask", "run", "--host=0.0.0.0", "--port=80"]
            
Создание и проверка образов nginx и wsgi-сервера
             
            
                docker build -t my_nginx
            
![image](https://github.com/ruslan0003/lab4/blob/main/lab4-2.jpg)
            
             
            
                docker build -t my-wsgi
            
![image](https://github.com/ruslan0003/lab4/blob/main/lab4-3.jpg)
            
             
            
                docker images
            
![image](https://github.com/ruslan0003/lab4/blob/main/lab4-4.jpg)
            
             
            
<h3 align="center">III. Загрузка образов в Docker Hub</h3>
            
Создание метки и выгрузка образа nginx
             
            
                docker tag my_nginx ruslan0003/my-nginx:latest
                docker push ruslan0003/my-nginx:latest
            
![image](https://github.com/ruslan0003/lab4/blob/main/lab4-5.jpg)
            
             
Создание метки и выгрузка образа wsgi-сервера
             
            
                docker tag my-wsgi ruslan0003/my-wsgi:latest
                docker push ruslan0003/my-wsgi:latest
            
![image](https://github.com/ruslan0003/lab4/blob/main/lab4-6.jpg)
            
             
Отображение образов в Docker Desktop:
             
![image](https://github.com/ruslan0003/lab4/blob/main/lab4-7.jpg)
            
             
Ссылки на образы:
             
[https://hub.docker.com/repository/docker/ruslan0003/my-nginx/general](https://hub.docker.com/repository/docker/ruslan0003/my-nginx/general) 
             
[https://hub.docker.com/repository/docker/ruslan0003/my-wsgi/general](https://hub.docker.com/repository/docker/ruslan0003/my-wsgi/general) 
             
        
<h3 align="center">IV. Использование docker-compose при работе<br>с несколькими образами/контейнерами, томами</h3>
        
Содержимое файла docker-compose.yaml и результат выполнения команды docker-compose up:
         
        
            version: '3'
            services:
              nginx:
                image: ruslan0003/my-nginx:latest
                ports:
                  - "8081:80"
                depends_on:
                  - wsgi
              wsgi:
                image: ruslan0003/my-wsgi:latest
                ports:
                  - "8082:80"
        
![image](https://github.com/ruslan0003/lab4/blob/main/lab4-8.jpg)
