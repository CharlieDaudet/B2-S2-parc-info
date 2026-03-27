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

##  Part III : docker-compose

### 2. WikiJS

🌞 Installez un WikiJS en utilisant Docker

```bash
 ┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ mkdir ~/tp_wikijs

┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ cd ~/tp_wikijs

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_wikijs]
└─$ vim docker-compose.yml


─(charlotte㉿efrei-xmg4agau1)-[~/tp_wikijs]
└─$ docker compose up -d
WARN[0000] /home/charlotte/tp_wikijs/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
[+] Running 23/25
 ✔ db Pulled                                               46.0s
 ⠙ wiki [⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿⣿] 159.7MB / 160.1MB Pulling          180.3s
canceled

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_wikijs]
└─$ docker compose up -d
WARN[0000] /home/charlotte/tp_wikijs/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
[+] Running 14/14
 ✔ wiki Pulled                                            224.6s
   ✔ 589002ba0eae Already exists                            0.0s
   ✔ 4ef00bbbc751 Pull complete                            23.0s
   ✔ 14b92b0c10a5 Pull complete                            23.4s
   ✔ 8d660e9b10b3 Pull complete                            23.5s
   ✔ 6496fb731e7e Pull complete                            26.8s
   ✔ 4f4fb700ef54 Pull complete                            26.9s
   ✔ 7059767cede5 Pull complete                            30.7s
   ✔ a5163cfde245 Pull complete                           221.0s
   ✔ a4740ba8b0bc Pull complete                           222.6s
   ✔ acfec40bf122 Pull complete                           222.7s
   ✔ b8fc2b5bc8fe Pull complete                           222.9s
   ✔ fc5ef9e0b620 Pull complete                           223.0s
   ✔ c0f2279c4c73 Pull complete                           223.1s
[+] Running 3/3
 ✔ Network tp_wikijs_default   Created                      0.6s
 ✔ Container tp_wikijs-db-1    Started                      3.5s
 ✔ Container tp_wikijs-wiki-1  Started                      3.8s

```
🌞 Call me when it's done



### 3. Make your own meow

🌞 Vous devez :

```bash
┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ mkdir ~/tp_meow

┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ ls
Bureau     Images   passwords.txt    tp_apache  tp_wikijs
docker     Modèles  Public           tp_docker  Vidéos
Documents  Musique  Téléchargements  tp_meow

┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ cd tp_meow

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_meow]
└─$ vim Dockerfile

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_meow]
└─$ vim docker-compose.yml

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_meow]
└─$ ls
docker-compose.yml  Dockerfile

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_meow]
└─$ vim app.py

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_meow]
└─$ vim requirements.txt

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_meow]
└─$ mkdir templates

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_meow]
└─$ cd templates

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_meow/templates]
└─$ vim index.html

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_meow/templates]
└─$ cd ..

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_meow]
└─$ docker compose up --build -d
WARN[0000] /home/charlotte/tp_meow/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
[+] Building 44.8s (11/11) FINISHED               docker:default
 => [app internal] load build definition from Dockerfile    0.0s
 => => transferring dockerfile: 201B                        0.0s
 => [app internal] load metadata for docker.io/library/pyt  1.7s
 => [app internal] load .dockerignore                       0.1s
 => => transferring context: 2B                             0.0s
 => [app 1/5] FROM docker.io/library/python:3.9-slim@sha2  28.0s
 => => resolve docker.io/library/python:3.9-slim@sha256:2d  0.2s
 => => sha256:2d97f6910b16bd338d3060f261 10.36kB / 10.36kB  0.0s
 => => sha256:dad5b29e3506c35e0fd222736f4d 1.74kB / 1.74kB  0.0s
 => => sha256:085da638e1b8a449514c3fda83ff 5.40kB / 5.40kB  0.0s
 => => sha256:38513bd7256313495cdd83b3b0 29.78MB / 29.78MB  3.0s
 => => sha256:b3ec39b36ae8c03a3e09854de4ec 1.29MB / 1.29MB  0.7s
 => => sha256:fc74430849022d13b0d44b8969 13.88MB / 13.88MB  2.1s
 => => sha256:ea56f685404adf81680322f152d2cfec 251B / 251B  1.1s
 => => extracting sha256:38513bd7256313495cdd83b3b0915a63  13.1s
 => => extracting sha256:b3ec39b36ae8c03a3e09854de4ec4aa08  1.3s
 => => extracting sha256:fc74430849022d13b0d44b8969a953f84  8.9s
 => => extracting sha256:ea56f685404adf81680322f152d2cfec6  0.0s
 => [app internal] load build context                       0.2s
 => => transferring context: 2.03kB                         0.0s
 => [app 2/5] WORKDIR /app                                  1.0s
 => [app 3/5] COPY requirements.txt .                       0.2s
 => [app 4/5] RUN pip install --no-cache-dir -r requireme  12.1s
 => [app 5/5] COPY . .                                      0.4s
 => [app] exporting to image                                0.8s
 => => exporting layers                                     0.7s
 => => writing image sha256:aa832f7d252af62f72b98e437d440b  0.0s
 => => naming to docker.io/library/tp_meow-app              0.0s
 => [app] resolving provenance for metadata file            0.1s
[+] Running 4/4
 ✔ app                      Built                           0.0s
 ✔ Network tp_meow_default  Cr...                           0.6s
 ✔ Container tp_meow-db-1   Sta...                          1.7s
 ✔ Container tp_meow-app-1  St...                           3.0s

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_meow]
└─$

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_meow]
└─$ docker compose ps
WARN[0000] /home/charlotte/tp_meow/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion
NAME            IMAGE          COMMAND                  SERVICE   CREATED          STATUS          PORTS
tp_meow-app-1   tp_meow-app    "python app.py"          app       38 seconds ago   Up 36 seconds   0.0.0.0:8888->8888/tcp, :::8888->8888/tcp
tp_meow-db-1    redis:alpine   "docker-entrypoint.s…"   db        38 seconds ago   Up 37 seconds   6379/tcp

┌──(charlotte㉿efrei-xmg4agau1)-[~/tp_meow]
└─$ curl localhost:8888
<h1>Add key</h1>
<form action="/add" method = "POST">

Key:
<input type="text" name="key" >

Value:
<input type="text" name="value" >

<input type="submit" value="Submit">
</form>

<h1>Check key</h1>
<form action="/get" method = "POST">

Key:
<input type="text" name="key" >
<input type="submit" value="Submit">
</form>

Host : 85843524d6da

```



### Part IV : Docker security

🌞 Prouvez que vous pouvez devenir root



