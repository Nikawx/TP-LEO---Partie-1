# 🌞 Installer Docker votre machine Azure

J'ai récupéré la machine que nous avions utilisé lors du test de Azure Cloud.

Connection en SSH :

```
ssh ClePublique@98.70.74.68

ClePublique@TEST:~$ sudo apt update && sudo apt upgrade -y

ClePublique@TEST:~$ sudo apt install docker.io

ClePublique@TEST:~$ sudo usermod -aG docker $(whoami)

ClePublique@TEST:~$ sudo systemctl start docker

ClePublique@TEST:~$ sudo systemctl enable docker

ClePublique@TEST:~$ sudo systemctl status 

Docker Status Enabled & Active
```

# 🌞 Utiliser la commande docker run

```
docker run --name web -d -v /path/to/html:/usr/share/nginx/html -p 9999:80 nginx
```


```
root@TEST:/home/ClePublique# curl localhost:80

<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

# 🌞 Rendre le service dispo sur internet

```
Direction Azure Cloud

Paramètres Réseau

Créer une règle de port

Règle de port d'entrée

Changer les plages de ports de destination de 8080 à 80

Ajouter la règle.

Hop nous avons accès au site internet depuis mon Windows sur cette ip : http://98.70.74.68/

```

# 🌞 Custom un peu le lancement du conteneur

```
sudo nano /etc/nginx/conf.d/custom.conf

server {
    listen 7777;
    root /etc/nginx/html;
    index index.html;
}
```

```
sudo nano /etc/nginx/html/index.html

<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>Bienvenue sur MEOW !</title>
</head>
<body>
    <h1>🐱 Serveur Nginx personnalisé MEOW 🐱</h1>
</body>
</html>
```

## Lancement du Docker

```
docker run -d \ -p 7777:7777 \ --name meow \ --memory=512m -v /etc/nginx/conf.d/custom.conf:/etc/nginx/conf.d/default.conf \ -v /etc/nginx/html:etc/nginx/html \ nginx
```

# 🌞 Call me

C'est fait !
Nom Discord : Nikawx.


