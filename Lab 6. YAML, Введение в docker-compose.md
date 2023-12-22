# Практические задания

1. Cоздайте простое приложение на Vuejs, проксируйте все запросы на веб сервер nginx.

Для этой Лаб. работы вместо VueJS было принято решение сделать приложение на NuxtJS.
Для начала создадим проект:

```sh
npx create-nuxt-app my-nuxt-app
```

После настройки проекта, создадим docker-compose.yml файл со следующими параметрами:

```yaml
version: '3'
services:
  nuxt:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - '3000:3000'
    command: 'npm run dev'

  nginx:
    image: nginx:latest
    ports:
      - '8080:80'
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
    depends_on:
      - nuxt
```

А так же Dockerfile:

```
FROM node:14-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "run", "dev"]
```

В настройках проекта (В данном случае в nuxt.config.js), настраиваем сервер:

```javascript
server: {
    host: '0.0.0.0',
    port: 3000,
  },
```

После чего билдим  docker-compose:

```sh
sudo docker-compose up --build
```

После запуска получаем ответ:

```sh
nuxt_1   | ℹ Compiling Client
nuxt_1   | ✔ Client: Compiled successfully in 4.61s
nuxt_1   | ℹ Waiting for file changes
nuxt_1   | ℹ Memory usage: 200 MB (RSS: 277 MB)
nuxt_1   | ℹ Listening on: http://172.19.0.2:3000/
```

Перейдя на страницу, мы получим запущенное приложение. Работу его можем проверить через curl:

```sh
curl http://172.19.0.2:3000/
<!doctype html>
<html lang="en" data-n-head="%7B%22lang%22:%7B%221%22:%22en%22%7D%7D">
  <head >
    <meta data-n-head="1" charset="utf-8"><meta data-n-head="1" name="viewport" content="width=device-width, initial-scale=1"><meta data-n-head="1" data-hid="description" name="description" content=""><meta data-n-head="1" name="format-detection" content="telephone=no"><title>DockerComposeNuxt</title><link data-n-head="1" rel="icon" type="image/x-icon" href="/favicon.ico"><link rel="preload" href="/_nuxt/runtime.js" as="script"><link rel="preload" href="/_nuxt/commons/app.js" as="script"><link rel="preload" href="/_nuxt/vendors/app.js" as="script"><link rel="preload" href="/_nuxt/app.js" as="script">
  </head>
  <body >
    <div id="__nuxt"><style> #nuxt-loading { background: white; visibility: hidden; opacity: 0; position: absolute; left: 0; right: 0; top: 0; bottom: 0; display: flex; justify-content: center; align-items: center; flex-direction: column; animation: nuxtLoadingIn 10s ease; -webkit-animation: nuxtLoadingIn 10s ease; animation-fill-mode: forwards; overflow: hidden; } @keyframes nuxtLoadingIn { 0% { visibility: hidden; opacity: 0; } 20% { visibility: visible; opacity: 0; } 100% { visibility: visible; opacity: 1; } } @-webkit-keyframes nuxtLoadingIn { 0% { visibility: hidden; opacity: 0; } 20% { visibility: visible; opacity: 0; } 100% { visibility: visible; opacity: 1; } } #nuxt-loading>div, #nuxt-loading>div:after { border-radius: 50%; width: 5rem; height: 5rem; } #nuxt-loading>div { font-size: 10px; position: relative; text-indent: -9999em; border: .5rem solid #F5F5F5; border-left: .5rem solid black; -webkit-transform: translateZ(0); -ms-transform: translateZ(0); transform: translateZ(0); -webkit-animation: nuxtLoading 1.1s infinite linear; animation: nuxtLoading 1.1s infinite linear; } #nuxt-loading.error>div { border-left: .5rem solid #ff4500; animation-duration: 5s; } @-webkit-keyframes nuxtLoading { 0% { -webkit-transform: rotate(0deg); transform: rotate(0deg); } 100% { -webkit-transform: rotate(360deg); transform: rotate(360deg); } } @keyframes nuxtLoading { 0% { -webkit-transform: rotate(0deg); transform: rotate(0deg); } 100% { -webkit-transform: rotate(360deg); transform: rotate(360deg); } } </style> <script> window.addEventListener('error', function () { var e = document.getElementById('nuxt-loading'); if (e) { e.className += ' error'; } }); </script> <div id="nuxt-loading" aria-live="polite" role="status"><div>Loading...</div></div> <!-- https://projects.lukehaas.me/css-loaders --> </div><script>window.__NUXT__={config:{_app:{basePath:"\u002F",assetsPath:"\u002F_nuxt\u002F",cdnURL:null}}}</script>
  <script src="/_nuxt/runtime.js"></script><script src="/_nuxt/commons/app.js"></script><script src="/_nuxt/vendors/app.js"></script><script src="/_nuxt/app.js"></script></body>
</html>
```
