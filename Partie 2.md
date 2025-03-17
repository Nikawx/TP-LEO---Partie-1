# 🌞 Construire votre propre image
```
root@TEST:/home/ClePublique# sudo mkdir debianapachesupremacy

root@TEST:/home/ClePublique# cd debianapachesupremacy

root@TEST:/home/ClePublique/debianapachesupremacy# nano Dockerfile
```
## Construction

```
FROM debian:latest

RUN apt update && apt upgrade -y && apt install -y apache2

COPY apache2.conf /etc/apache2/apache2.conf

COPY index.html /var/www/html/index.html

EXPOSE 80

CMD ["apache2", "-DFOREGROUND"]

```

root@TEST:/home/ClePublique/debianapachesupremacy# nano apache2.conf
```
Listen 80

LoadModule mpm_event_module "/usr/lib/apache2/modules/mod_mpm_event.so"
LoadModule dir_module "/usr/lib/apache2/modules/mod_dir.so"
LoadModule authz_core_module "/usr/lib/apache2/modules/mod_authz_core.so"

DirectoryIndex index.html
DocumentRoot "/var/www/html/"

ErrorLog "/var/log/apache2/error.log"
LogLevel warn
```

root@TEST:/home/ClePublique/debianapachesupremacy# nano index.html

```
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>Mon serveur Apache Dockerisé</title>
</head>
<body>
    <h1>🚀 Serveur Apache sous Docker ! 🚀</h1>
    <p>Bienvenue sur mon serveur web personnalisé.</p>
</body>
</html>
```

docker build . -t debianapachesupremacy

```
Sending build context to Docker daemon  17.41kB
Step 1/3 : FROM ubuntu
 ---> a04dc4851cbc
Step 2/3 : RUN apt update -y
 ---> Using cache
 ---> cfdd43f0bbd2
Step 3/3 : RUN apt install -y nginx
 ---> Using cache
 ---> cf1844711798
Successfully built cf1844711798
Successfully tagged debianapachesupremcyomg:latest
```

``