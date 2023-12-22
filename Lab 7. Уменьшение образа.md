# Практические задания

1. Cоздайте простое приложение на flask, подключите mongodb;
2. Уменьшите размер образа;
3. Используйте пременные окружения, для коннекта к db;
4. Исключите root из Dockerfile;
5. Проверьте работоспособность сервисов.
6. Запустите сборку приложения из 1 задания, в образе Dind. Разверните приложение локально.


**В данной лабораторной работе пришлось делать не по пунктам для экономии места и времени, поэтому каждый пункт будет описан более обширно, нежели как в заданиях**

1. **Простое приложение на Flask с подключением к MongoDB:**

Создадим простое приложение на Flask, которое будет подключаться к MongoDB.

Установим зависимости:

```bash
pip install Flask pymongo
```

Код приложения:

```python
from flask import Flask
from pymongo import MongoClient

app = Flask(__name__)

# Подключение к MongoDB
client = MongoClient("mongodb://mongodb_host:27017/")
db = client["mydatabase"]
collection = db["mycollection"]

@app.route('/')
def hello():
    # Добавляем данные в MongoDB
    collection.insert_one({'name': 'John', 'age': 30})
    return 'Data added to MongoDB!'

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
```

2. **Создание Dockerfile:**

Создадим Dockerfile для нашего приложения и уменьшим размер образа с помощью нескольких оптимизаций:

```Dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt

COPY . .

# Исключение root
RUN useradd -m myuser
USER myuser

CMD ["python", "app.py"]
```

3. **Использование переменных окружения для коннекта к БД:**

```Dockerfile
# ...

ENV MONGODB_HOST=mongodb_host
ENV MONGODB_PORT=27017

# ...

CMD ["python", "app.py"]
```

4. **Проверка работоспособности сервисов:**

Убедимся, что MongoDB запущена и доступна по адресу, используя curl `mongodb_host:27017`. После этого, соберем образ Docker из приложения с помощью `sudo docker build`.

5. **Запуск сборки в образе Docker-in-Docker (DinD):**

Для запуска сборки внутри контейнера DinD, выполните следующие команды:

```sh
sudo docker run -v /var/run/docker.sock:/var/run/docker.sock docker:dind
```

Затем внутри этого контейнера запустим билд:

```sh
sudo docker build -t my-flask-app .
```

После завершения сборки разворачиваем приложение локально с помощью команды:

```sh
sudo docker run -p 5000:5000 my-flask-app
```
