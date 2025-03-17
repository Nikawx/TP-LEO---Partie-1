# 🌞 Installez un WikiJS en utilisant Docker

```

ClePublique@TEST:~$ sudo nano docker-compose.yml

version: "3"
services:
  postgres:
    image: postgres:14
    container_name: wikijs-db
    restart: always
    environment:
      POSTGRES_DB: wikijs
      POSTGRES_USER: wikijs
      POSTGRES_PASSWORD: mypassword
    volumes:
      - postgres-data:/var/lib/postgresql/data

  wikijs:
    image: requarks/wiki:latest
    container_name: wikijs
    restart: always
    depends_on:
      - postgres
    ports:
      - "8080:3000"
    environment:
      DB_TYPE: postgres
      DB_HOST: postgres
      DB_PORT: 5432
      DB_USER: wikijs
      DB_PASS: mypassword
      DB_NAME: wikijs

volumes:
  postgres-data:
```

## Lancer le conteneur docker-compose

```
docker-compose up -d
```
## curl le site

```
root@TEST:/home/ClePublique# docker-compose ps

  Name                 Command               State                         Ports
-------------------------------------------------------------------------------------------------------
wikijs      docker-entrypoint.sh node  ...   Up      0.0.0.0:8080->3000/tcp,:::8080->3000/tcp, 3443/tcp
wikijs-db   docker-entrypoint.sh postgres    Up      5432/tcp

root@TEST:/home/ClePublique# curl localhost:8080


<!DOCTYPE html>
<html lang="en">
<head>
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta charset="UTF-8">
    <meta name="viewport" content="user-scalable=yes, width=device-width, initial-scale=1, maximum-scale=5">
    
    <meta name="theme-color" content="#1976d2">
    <meta name="msapplication-TileColor" content="#1976d2">
    <meta name="msapplication-TileImage" content="/_assets/favicons/mstile-150x150.png">
    
    <title>Wiki.js Setup</title>
    
    <link rel="apple-touch-icon" sizes="180x180" href="/_assets/favicons/apple-touch-icon.png">
    <link rel="icon" type="image/png" sizes="192x192" href="/_assets/favicons/android-chrome-192x192.png">
    <link rel="icon" type="image/png" sizes="32x32" href="/_assets/favicons/favicon-32x32.png">
    <link rel="icon" type="image/png" sizes="16x16" href="/_assets/favicons/favicon-16x16.png">
    <link rel="mask-icon" href="/_assets/favicons/safari-pinned-tab.svg" color="#1976d2">
    <link rel="manifest" href="/_assets/manifest.json">
    
    <script>
        var siteConfig = { "title": "Wiki.js" };
    </script>
    
    <link type="text/css" rel="stylesheet" href="/_assets/css/setup.22871ffac1b643eed4d9.css">
    <script type="text/javascript" src="/_assets/js/runtime.js?1738531300"></script>
    <script type="text/javascript" src="/_assets/js/setup.js?1738531300"></script>
</head>
<body>
    <div id="root">
        <setup wiki-version="2.5.306"></setup>
    </div>
</body>
</html>

```

## tout est parfait j'ai maintenant accès au setup de WikiJs

via ce lien : http://98.70.74.68:8080/en/home

J'ai aussi le docker avec la bdd sur cette adresse : 98.70.74.68:5432

nous pouvons cunsulter la base de donnée avec cette commande :

```
 docker exec -it wikijs-db psql -U wikijs -d wikijs

 wikijs=# \dt;
             List of relations
 Schema |       Name       | Type  | Owner
--------+------------------+-------+--------
 public | analytics        | table | wikijs
 public | apiKeys          | table | wikijs
 public | assetData        | table | wikijs
 public | assetFolders     | table | wikijs
 public | assets           | table | wikijs
 public | authentication   | table | wikijs
 public | brute            | table | wikijs
 public | commentProviders | table | wikijs
 public | comments         | table | wikijs
 public | editors          | table | wikijs
 public | groups           | table | wikijs
 public | locales          | table | wikijs
 public | loggers          | table | wikijs
 public | migrations       | table | wikijs
 public | migrations_lock  | table | wikijs
 public | navigation       | table | wikijs
 public | pageHistory      | table | wikijs
 public | pageHistoryTags  | table | wikijs
 public | pageLinks        | table | wikijs
 public | pageTags         | table | wikijs
 public | pageTree         | table | wikijs
 public | pages            | table | wikijs
 public | renderers        | table | wikijs
 public | searchEngines    | table | wikijs
 public | sessions         | table | wikijs
 public | settings         | table | wikijs
 public | storage          | table | wikijs
 public | tags             | table | wikijs
 public | userAvatars      | table | wikijs
 public | userGroups       | table | wikijs
 public | userKeys         | table | wikijs
 public | users            | table | wikijs
(32 rows)
```

# 🌞 Vous devez :

SO !

J'ai pu faire un Dockerfile et un docker-compose.yml dans le répertoire Images.

```
root@TEST:/home/ClePublique/Images# cat docker-compose.yml


version: '3.8'

services:
  app:
    build: .
    ports:
      - "8888:8888"
    depends_on:
      - db
    environment:
      - REDIS_HOST=db
      - REDIS_PORT=6379

  db:
    image: "redis:alpine"
    container_name: redis_db
    ports:
      - "6379:6379"
```

```
root@TEST:/home/ClePublique/Images# cat docker-compose.yml
version: '3.8'

services:
  app:
    build: .
    ports:
      - "8888:8888"
    depends_on:
      - db
    environment:
      - REDIS_HOST=db
      - REDIS_PORT=6379

  db:
    image: "redis:alpine"
    container_name: redis_db
    ports:
      - "6379:6379"

```

```      
root@TEST:/home/ClePublique/Images# cat Dockerfile

FROM python:3.10

WORKDIR /app

COPY . .

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 8888

CMD ["python", "app.py"]
```

Dans le dossier Images il y a le programme Python que tu as fais

Par la suite fais un 
```
docker-compose up --build
```


## BINGOOOOOOOOO !

```
root@TEST:/home/ClePublique/Images# docker-compose up --build
Building app
DEPRECATED: The legacy builder is deprecated and will be removed in a future release.
            Install the buildx component to build images with BuildKit:
            https://docs.docker.com/go/buildx/

Sending build context to Docker daemon  13.82kB
Step 1/6 : FROM python:3.10
 ---> e83a01774710
Step 2/6 : WORKDIR /app
 ---> Using cache
 ---> 011a33c3a1ca
Step 3/6 : COPY . .
 ---> 738ff9ced8f3
Step 4/6 : RUN pip install --no-cache-dir -r requirements.txt
 ---> Running in 9e66789f802e
Collecting redis
  Downloading redis-5.2.1-py3-none-any.whl (261 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 261.5/261.5 kB 1.8 MB/s eta 0:00:00
Collecting flask
  Downloading flask-3.1.0-py3-none-any.whl (102 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 103.0/103.0 kB 15.0 MB/s eta 0:00:00
Collecting async-timeout>=4.0.3
  Downloading async_timeout-5.0.1-py3-none-any.whl (6.2 kB)
Collecting blinker>=1.9
  Downloading blinker-1.9.0-py3-none-any.whl (8.5 kB)
Collecting itsdangerous>=2.2
  Downloading itsdangerous-2.2.0-py3-none-any.whl (16 kB)
Collecting Werkzeug>=3.1
  Downloading werkzeug-3.1.3-py3-none-any.whl (224 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 224.5/224.5 kB 12.8 MB/s eta 0:00:00
Collecting Jinja2>=3.1.2
  Downloading jinja2-3.1.6-py3-none-any.whl (134 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 134.9/134.9 kB 15.1 MB/s eta 0:00:00
Collecting click>=8.1.3
  Downloading click-8.1.8-py3-none-any.whl (98 kB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 98.2/98.2 kB 26.0 MB/s eta 0:00:00
Collecting MarkupSafe>=2.0
  Downloading MarkupSafe-3.0.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (20 kB)
Installing collected packages: MarkupSafe, itsdangerous, click, blinker, async-timeout, Werkzeug, redis, Jinja2, flask
Successfully installed Jinja2-3.1.6 MarkupSafe-3.0.2 Werkzeug-3.1.3 async-timeout-5.0.1 blinker-1.9.0 click-8.1.8 flask-3.1.0 itsdangerous-2.2.0 redis-5.2.1
 ---> Removed intermediate container 9e66789f802e
 ---> 07acf5d22a64
Step 5/6 : EXPOSE 8888
 ---> Running in ae78b9d4457b
 ---> Removed intermediate container ae78b9d4457b
 ---> 95564188ee21
Step 6/6 : CMD ["python", "app.py"]
 ---> Running in 5267d37e046d
 ---> Removed intermediate container 5267d37e046d
 ---> 1ab7739ab0cb
Successfully built 1ab7739ab0cb
Successfully tagged images_app:latest
Creating redis_db ... done
Creating images_app_1 ... done
Attaching to redis_db, images_app_1
redis_db | 1:C 17 Mar 2025 13:35:02.235 # WARNING Memory overcommit must be enabled! Without it, a background save or replication may fail under low memory condition. Being disabled, it can also cause failures without low memory condition, see https://github.com/jemalloc/jemalloc/issues/1328. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
redis_db | 1:C 17 Mar 2025 13:35:02.235 * oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
redis_db | 1:C 17 Mar 2025 13:35:02.235 * Redis version=7.4.2, bits=64, commit=00000000, modified=0, pid=1, just started
redis_db | 1:C 17 Mar 2025 13:35:02.235 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
redis_db | 1:M 17 Mar 2025 13:35:02.235 * monotonic clock: POSIX clock_gettime
redis_db | 1:M 17 Mar 2025 13:35:02.240 * Running mode=standalone, port=6379.
redis_db | 1:M 17 Mar 2025 13:35:02.241 * Server initialized
redis_db | 1:M 17 Mar 2025 13:35:02.243 * Ready to accept connections tcp
app_1  |  * Serving Flask app 'app'
app_1  |  * Debug mode: off
app_1  | WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
app_1  |  * Running on all addresses (0.0.0.0)
app_1  |  * Running on http://127.0.0.1:8888
app_1  |  * Running on http://172.20.0.3:8888
app_1  | Press CTRL+C to quit

```

## Detection de ma connection :

```
app_1  | 90.47.49.230 - - [17/Mar/2025 13:37:45] "GET / HTTP/1.1" 200 -
app_1  | 90.47.49.230 - - [17/Mar/2025 13:37:45] "GET /favicon.ico HTTP/1.1" 404 -
```


## J'ai curl depuis mon Powershell Windows comme mon Image est en cours d'utilisation

```
PS C:\Users\1180024> curl http://98.70.74.68:8888/


StatusCode        : 200
StatusDescription : OK
Content           : <h1>Add key</h1>
                    <form action="/add" method = "POST">

                    Key:
                    <input type="text" name="key" >

                    Value:
                    <input type="text" name="value" >

                    <input type="submit" value="Submit">
                    </form>

                    <h1>Check key</h1>
                    ...
RawContent        : HTTP/1.1 200 OK
                    Connection: close
                    Content-Length: 340
                    Content-Type: text/html; charset=utf-8
                    Date: Mon, 17 Mar 2025 13:39:18 GMT
                    Server: Werkzeug/3.1.3 Python/3.10.16

                    <h1>Add key</h1>
                    <form ac...
Forms             : {, }
Headers           : {[Connection, close], [Content-Length, 340], [Content-Type, text/html; charset=utf-8], [Date, Mon,
                    17 Mar 2025 13:39:18 GMT]...}
Images            : {}
InputFields       : {@{innerHTML=; innerText=; outerHTML=<INPUT name=key>; outerText=; tagName=INPUT; name=key},
                    @{innerHTML=; innerText=; outerHTML=<INPUT name=value>; outerText=; tagName=INPUT; name=value},
                    @{innerHTML=; innerText=; outerHTML=<INPUT type=submit value=Submit>; outerText=; tagName=INPUT;
                    type=submit; value=Submit}, @{innerHTML=; innerText=; outerHTML=<INPUT name=key>; outerText=;
                    tagName=INPUT; name=key}...}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 340
```

### PS : j'ai pas compris le site que tu as fais avec les ADD KEY et CHECK KEY :D