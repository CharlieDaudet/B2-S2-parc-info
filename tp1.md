# Part I : Docker basics

## 1.Install

🌞 Installer Docker votre machine Azure

```bash
┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ sudo apt update -y
┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ sudo apt install -y docker.io
┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ sudo systemctl enable docker --now
Synchronizing state of docker.service with SysV service script with /usr/lib/systemd/systemd-sysv-install.
Executing: /usr/lib/systemd/systemd-sysv-install enable docker
┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ sudo usermod -aG docker $charlotte

```
🌞 Utiliser la commande docker run


```bash
┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ docker run --name web -d -v /path/to/html:/usr/share/nginx/html -p 9999:80 nginx
e49e9a791cbf1a9094b67e9f870e0ec993ac63e073bd7c82c723858c341e2ec5

┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ docker ps -a
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS                      PORTS                                     NAMES
e49e9a791cbf   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute           0.0.0.0:9999->80/tcp, [::]:9999->80/tcp   web
```

🌞 Rendre le service dispo sur internet

```bash
┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ curl localhost:9999
<html>
<head><title>403 Forbidden</title></head>
<body>
<center><h1>403 Forbidden</h1></center>
<hr><center>nginx/1.29.6</center>
</body>
</html>
```

🌞 Custom un peu le lancement du conteneur

```bash
┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ cd tp_docker

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_docker]
└─$vim nginx.conf

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_docker]
└─$ vim index.html
┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_docker]
└─$ docker run -d \
  --name meow \
  --memory="512m" \
  -p 7777:7777 \
  -v $(pwd)/nginx.conf:/etc/nginx/conf.d/nginx.conf \
  -v $(pwd):/var/www/tp_docker \
  nginx
9b2afd3aafaff87e80b4bfbaf2cfb45900b210b4ff27ffc90fe9d668742e477b

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_docker]
└─$ curl localhost:7777
<h1> Meow !</h1>
<p>MEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEEOOOOOOOOOOOOOOOOOOOOOOOOOWWWWWWWWWWWWWWWWWWWWWWWW .</p>

```


## Part II : Images

🌞 Construire votre propre image
```bash
┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ mkdir ~/tp_apache

┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ cd tp_apache

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_apache]
└─$ vim apache2.conf


┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_apache]
└─$ vim index.html

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_apache]
└─$ vim Dockerfile

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_apache]
└─$ docker build . -t apache_custom
[+] Building 1.7s (9/9) FINISHED                                           docker:default
 => [internal] load build definition from Dockerfile                                 0.1s
 => => transferring dockerfile: 222B                                                 0.0s
 => [internal] load metadata for docker.io/library/debian:latest                     0.0s
 => [internal] load .dockerignore                                                    0.1s
 => => transferring context: 2B                                                      0.0s
 => [1/4] FROM docker.io/library/debian:latest                                       0.0s
 => [internal] load build context                                                    0.1s
 => => transferring context: 395B                                                    0.0s
 => CACHED [2/4] RUN apt update -y && apt install -y apache2                         0.0s
 => [3/4] COPY apache2.conf /etc/apache2/apache2.conf                                0.3s
 => [4/4] COPY index.html /var/www/html/index.html                                   0.3s
 => exporting to image                                                               0.4s
 => => exporting layers                                                              0.3s
 => => writing image sha256:75ae6e1f2d8ed3c81b25665f26146a968c5e84596f372a6a980a45c  0.0s
 => => naming to docker.io/library/apache_custom                                     0.0s

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_apache]
└─$ docker run -d --name super_apache -p 8080:80 apache_custom
9be6a42cdb5d2e963311c3ffb9196cc7553a1ce3584c3dacd31863a0e423d5d1

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_apache]
└─$ docker images
REPOSITORY      TAG       IMAGE ID       CREATED          SIZE
apache_custom   latest    75ae6e1f2d8e   42 seconds ago   226MB
<none>          <none>    07e7fba3db4c   12 minutes ago   226MB
nginx           latest    1a1e63136420   2 days ago       161MB
debian          latest    2158d138d975   3 days ago       120MB

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_apache]
└─$ curl localhost:8080
<h1>Image faite de mes blanches mains</h1>
<p>Meowwwwwwwwwwwwwwwwww</p>
```
🌞 Dans les deux cas, j'attends juste votre Dockerfile dans le compte-rendu

[Dockerfile](Dockerfile.txt)