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

```bash

┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ docker run --rm -v /:/host_root alpine cat /host_root/etc/shadow
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
589002ba0eae: Already exists
Digest: sha256:25109184c71bdad752c8312a8623239686a9a2071e8825f20acb8f2198c3f659
Status: Downloaded newer image for alpine:latest
root:!:20480:0:99999:7:::
daemon:*:20480:0:99999:7:::
bin:*:20480:0:99999:7:::
sys:*:20480:0:99999:7:::
sync:*:20480:0:99999:7:::
games:*:20480:0:99999:7:::
man:*:20480:0:99999:7:::
lp:*:20480:0:99999:7:::
mail:*:20480:0:99999:7:::
news:*:20480:0:99999:7:::
uucp:*:20480:0:99999:7:::
proxy:*:20480:0:99999:7:::
www-data:*:20480:0:99999:7:::
backup:*:20480:0:99999:7:::
list:*:20480:0:99999:7:::
irc:*:20480:0:99999:7:::
_apt:*:20480:0:99999:7:::
nobody:*:20480:0:99999:7:::
systemd-network:!*:20480:::::1:
dhcpcd:!:20480::::::
mysql:!:20480::::::
tss:!:20480::::::
strongswan:!:20480::::::
systemd-timesync:!*:20480:::::1:
_gophish:!:20480::::::
iodine:!:20480::::::
messagebus:!:20480::::::
tcpdump:!:20480::::::
miredo:!:20480::::::
_rpc:!:20480::::::
redis:!:20480::::::
mosquitto:!:20480::::::
redsocks:!:20480::::::
stunnel4:!*:20480::::::
sshd:!:20480::::::
dnsmasq:!:20480::::::
Debian-snmp:!:20480::::::
sslh:!:20480::::::
postgres:!:20480::::::
avahi:!:20480::::::
speech-dispatcher:!:20480::::::
_gvm:!:20480::::::
usbmux:!:20480::::::
cups-pk-helper:!:20480::::::
nm-openvpn:!:20480::::::
inetsim:!:20480::::::
nm-openconnect:!:20480::::::
geoclue:!:20480::::::
lightdm:!:20480::::::
statd:!:20480::::::
saned:!:20480::::::
polkitd:!*:20480::::::
rtkit:!:20480::::::
colord:!:20480::::::
charlotte:$y$j9T$T8GuWj.oJ2P7O0UpcXeAt1$AjO2eAudhe3guRh3zmhgxWvgk3j5rVw4opbprOog.W8:20480:0:99999:7:::

```
🌞 Utilisez Trivy
```bash 
┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ alias trivy="docker run --rm -v /var/run/docker.sock:/var/run/docker.sock -v \$HOME/.cache:/root/.cache/ aquasec/trivy:0.49.1"

┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ trivy image apache_custom --severity HIGH,CRITICAL
2026-03-29T18:12:18.266Z        INFO    Vulnerability scanning is enabled
2026-03-29T18:12:18.267Z        INFO    Secret scanning is enabled
2026-03-29T18:12:18.267Z        INFO    If your scanning is slow, please try '--scanners vuln' to disable secret scanning
2026-03-29T18:12:18.267Z        INFO    Please see also https://aquasecurity.github.io/trivy/v0.49/docs/scanner/secret/#recommendation for faster secret detection
2026-03-29T18:12:49.610Z        INFO    Detected OS: debian
2026-03-29T18:12:49.610Z        INFO    Detecting Debian vulnerabilities...
2026-03-29T18:12:49.698Z        INFO    Number of language-specific files: 0

apache_custom (debian 13.4)
===========================
Total: 1 (HIGH: 1, CRITICAL: 0)

┌───────────┬────────────────┬──────────┬──────────┬───────────────────┬───────────────┬─────────────────────────────────────────────────────┐
│  Library  │ Vulnerability  │ Severity │  Status  │ Installed Version │ Fixed Version │                        Title                        │
├───────────┼────────────────┼──────────┼──────────┼───────────────────┼───────────────┼─────────────────────────────────────────────────────┤
│ libexpat1 │ CVE-2026-25210 │ HIGH     │ affected │ 2.7.1-2           │               │ libexpat: libexpat: Information disclosure and data │
│           │                │          │          │                   │               │ integrity issues due to integer overflow...         │
│           │                │          │          │                   │               │ https://avd.aquasec.com/nvd/cve-2026-25210          │
└───────────┴────────────────┴──────────┴──────────┴───────────────────┴───────────────┴─────────────────────────────────────────────────────┘

/etc/ssl/private/ssl-cert-snakeoil.key (secrets)
================================================
Total: 1 (HIGH: 1, CRITICAL: 0)

HIGH: AsymmetricPrivateKey (private-key)
════════════════════════════════════════
Asymmetric Private Key
────────────────────────────────────────
 /etc/ssl/private/ssl-cert-snakeoil.key:1 (added by 'RUN /bin/sh -c apt update -y && apt inst')
────────────────────────────────────────
   1 [ -----BEGIN PRIVATE KEY-----*******************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************************-----END PRIVATE KEY-----
   2
────────────────────────────────────────
 
┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ trivy image --scanners vuln --severity HIGH,CRITICAL --timeout 15m requarks/wiki:2
2026-03-29T18:26:49.649Z        INFO    Vulnerability scanning is enabled
2026-03-29T18:27:33.917Z        INFO    Detected OS: alpine
2026-03-29T18:27:33.918Z        WARN    This OS version is not on the EOL list: alpine 3.23
2026-03-29T18:27:33.918Z        INFO    Detecting Alpine vulnerabilities...
2026-03-29T18:27:34.089Z        INFO    Number of language-specific files: 1
2026-03-29T18:27:34.090Z        INFO    Detecting node-pkg vulnerabilities...
2026-03-29T18:27:35.335Z        INFO    Table result includes only package filenames. Use '--format json' option to get the full path to the package file.

requarks/wiki:2 (alpine 3.23.3)
===============================
Total: 3 (HIGH: 2, CRITICAL: 1)

#J'ai pas mis tous le tableau pcq c'est quand même trèèèèèès long comme truc#


┌──(charlotte㉿efrei-xmg4agau1)-[~]
└─$ trivy image postgres:15-alpine --severity HIGH,CRITICAL
2026-03-29T18:31:19.977Z        INFO    Vulnerability scanning is enabled
2026-03-29T18:31:19.977Z        INFO    Secret scanning is enabled
2026-03-29T18:31:19.977Z        INFO    If your scanning is slow, please try '--scanners vuln' to disable secret scanning
2026-03-29T18:31:19.977Z        INFO    Please see also https://aquasecurity.github.io/trivy/v0.49/docs/scanner/secret/#recommendation for faster secret detection
2026-03-29T18:31:52.116Z        INFO    Detected OS: alpine
2026-03-29T18:31:52.117Z        WARN    This OS version is not on the EOL list: alpine 3.23
2026-03-29T18:31:52.117Z        INFO    Detecting Alpine vulnerabilities...
2026-03-29T18:31:52.150Z        INFO    Number of language-specific files: 1
2026-03-29T18:31:52.151Z        INFO    Detecting gobinary vulnerabilities...

postgres:15-alpine (alpine 3.23.3)
==================================
Total: 1 (HIGH: 1, CRITICAL: 0)

┌─────────┬────────────────┬──────────┬────────┬───────────────────┬───────────────┬─────────────────────────────────────────────────────────────┐
│ Library │ Vulnerability  │ Severity │ Status │ Installed Version │ Fixed Version │                            Title                            │
├─────────┼────────────────┼──────────┼────────┼───────────────────┼───────────────┼─────────────────────────────────────────────────────────────┤
│ zlib    │ CVE-2026-22184 │ HIGH     │ fixed  │ 1.3.1-r2          │ 1.3.2-r0      │ zlib: zlib: Arbitrary code execution via buffer overflow in │
│         │                │          │        │                   │               │ untgz utility                                               │
│         │                │          │        │                   │               │ https://avd.aquasec.com/nvd/cve-2026-22184                  │
└─────────┴────────────────┴──────────┴────────┴───────────────────┴───────────────┴─────────────────────────────────────────────────────────────┘
che.
2026-03-29T18:42:55.190Z        INFO    Detected OS: debian
2026-03-29T18:42:55.190Z        INFO    Detecting Debian vulnerabilities...
2026-03-29T18:42:55.660Z        INFO    Number of language-specific files: 0

nginx:latest (debian 13.4)
==========================
Total: 4 (HIGH: 4, CRITICAL: 0)

┌─────────────────────────┬────────────────┬──────────┬──────────┬───────────────────┬───────────────┬──────────────────────────────────────────────────────────────┐
│         Library         │ Vulnerability  │ Severity │  Status  │ Installed Version │ Fixed Version │                            Title                             │
├─────────────────────────┼────────────────┼──────────┼──────────┼───────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libexpat1               │ CVE-2026-25210 │ HIGH     │ affected │ 2.7.1-2           │               │ libexpat: libexpat: Information disclosure and data          │
│                         │                │          │          │                   │               │ integrity issues due to integer overflow...                  │
│                         │                │          │          │                   │               │ https://avd.aquasec.com/nvd/cve-2026-25210                   │
├─────────────────────────┼────────────────┤          │          ├───────────────────┼───────────────┼──────────────────────────────────────────────────────────────┤
│ libheif-plugin-dav1d    │ CVE-2025-68431 │          │          │ 1.19.8-1          │               │ libheif is an HEIF and AVIF file format decoder and encoder. │
│                         │                │          │          │                   │               │ Prior...                                                     │
│                         │                │          │          │                   │               │ https://avd.aquasec.com/nvd/cve-2025-68431                   │
├─────────────────────────┤                │          │          │                   ├───────────────┤                                                              │
│ libheif-plugin-libde265 │                │          │          │                   │               │                                                              │
│                         │                │          │          │                   │               │                                                              │
│                         │                │          │          │                   │               │                                                              │
├─────────────────────────┤                │          │          │                   ├───────────────┤                                                              │
│ libheif1                │                │          │          │                   │               │                                                              │
│                         │                │          │          │                   │               │                                                              │
│                         │                │          │          │                   │               │                                                              │
└─────────────────────────┴────────────────┴──────────┴──────────┴───────────────────┴───────────────┴──────────────────────────────────────────────────────────────┘
```
🌞 Utilisez l'outil Docker Bench for Security


```bash 

[INFO] 1 - Host Configuration
[WARN] 1.1  - Ensure a separate partition for containers has been created
[NOTE] 1.2  - Ensure the container host has been Hardened
[PASS] 1.3  - Ensure Docker is up to date
[INFO]      * Using 27.5.1+4 which is current
[INFO]      * Check with your operating system vendor for support and security maintenance for Docker
[INFO] 1.4  - Ensure only trusted users are allowed to control Docker daemon
[INFO]      * docker:x:136:charlotte
[WARN] 1.5  - Ensure auditing is configured for the Docker daemon
[WARN] 1.6  - Ensure auditing is configured for Docker files and directories - /var/lib/docker
[WARN] 1.7  - Ensure auditing is configured for Docker files and directories - /etc/docker
[WARN] 1.8  - Ensure auditing is configured for Docker files and directories - docker.service
[WARN] 1.9  - Ensure auditing is configured for Docker files and directories - docker.socket
[WARN] 1.10  - Ensure auditing is configured for Docker files and directories - /etc/default/docker
[INFO] 1.11  - Ensure auditing is configured for Docker files and directories - /etc/docker/daemon.json
[INFO]      * File not found
[INFO] 1.12  - Ensure auditing is configured for Docker files and directories - /usr/bin/docker-containerd
[INFO]      * File not found
[INFO] 1.13  - Ensure auditing is configured for Docker files and directories - /usr/bin/docker-runc
[INFO]      * File not found


[INFO] 2 - Docker daemon configuration
[WARN] 2.1  - Ensure network traffic is restricted between containers on the default bridge
[PASS] 2.2  - Ensure the logging level is set to 'info'
[PASS] 2.3  - Ensure Docker is allowed to make changes to iptables
[PASS] 2.4  - Ensure insecure registries are not used
[PASS] 2.5  - Ensure aufs storage driver is not used
[INFO] 2.6  - Ensure TLS authentication for Docker daemon is configured
[INFO]      * Docker daemon not listening on TCP
[INFO] 2.7  - Ensure the default ulimit is configured appropriately
[INFO]      * Default ulimit doesn't appear to be set
[WARN] 2.8  - Enable user namespace support
[PASS] 2.9  - Ensure the default cgroup usage has been confirmed
[PASS] 2.10  - Ensure base device size is not changed until needed
[WARN] 2.11  - Ensure that authorization for Docker client commands is enabled
[WARN] 2.12  - Ensure centralized and remote logging is configured
sh: 2751+4: bad number
[INFO] 2.13  - Ensure operations on legacy registry (v1) are Disabled (Deprecated)
[WARN] 2.14  - Ensure live restore is Enabled
[WARN] 2.15  - Ensure Userland Proxy is Disabled
[INFO] 2.16  - Ensure daemon-wide custom seccomp profile is applied, if needed
[PASS] 2.17  - Ensure experimental features are avoided in production
[WARN] 2.18  - Ensure containers are restricted from acquiring new privileges


[INFO] 3 - Docker daemon configuration files
[PASS] 3.1  - Ensure that docker.service file ownership is set to root:root
[PASS] 3.2  - Ensure that docker.service file permissions are set to 644 or more restrictive
[PASS] 3.3  - Ensure that docker.socket file ownership is set to root:root
[PASS] 3.4  - Ensure that docker.socket file permissions are set to 644 or more restrictive
[PASS] 3.5  - Ensure that /etc/docker directory ownership is set to root:root
[PASS] 3.6  - Ensure that /etc/docker directory permissions are set to 755 or more restrictive
[INFO] 3.7  - Ensure that registry certificate file ownership is set to root:root
[INFO]      * Directory not found
[INFO] 3.8  - Ensure that registry certificate file permissions are set to 444 or more restrictive
[INFO]      * Directory not found
[INFO] 3.9  - Ensure that TLS CA certificate file ownership is set to root:root
[INFO]      * No TLS CA certificate found
[INFO] 3.10  - Ensure that TLS CA certificate file permissions are set to 444 or more restrictive
[INFO]      * No TLS CA certificate found
[INFO] 3.11  - Ensure that Docker server certificate file ownership is set to root:root
[INFO]      * No TLS Server certificate found
[INFO] 3.12  - Ensure that Docker server certificate file permissions are set to 444 or more restrictive
[INFO]      * No TLS Server certificate found
[INFO] 3.13  - Ensure that Docker server certificate key file ownership is set to root:root
[INFO]      * No TLS Key found
[INFO] 3.14  - Ensure that Docker server certificate key file permissions are set to 400
[INFO]      * No TLS Key found
[PASS] 3.15  - Ensure that Docker socket file ownership is set to root:docker
[PASS] 3.16  - Ensure that Docker socket file permissions are set to 660 or more restrictive
[INFO] 3.17  - Ensure that daemon.json file ownership is set to root:root
[INFO]      * File not found
[INFO] 3.18  - Ensure that daemon.json file permissions are set to 644 or more restrictive
[INFO]      * File not found
[PASS] 3.19  - Ensure that /etc/default/docker file ownership is set to root:root
[PASS] 3.20  - Ensure that /etc/default/docker file permissions are set to 644 or more restrictive


[INFO] 4 - Container Images and Build File
[WARN] 4.1  - Ensure a user for the container has been created
[WARN]      * Running as root: tp_meow-db-1
[WARN]      * Running as root: tp_wikijs-db-1
[NOTE] 4.2  - Ensure that containers use trusted base images
[NOTE] 4.3  - Ensure unnecessary packages are not installed in the container
[NOTE] 4.4  - Ensure images are scanned and rebuilt to include security patches
[WARN] 4.5  - Ensure Content trust for Docker is Enabled
[WARN] 4.6  - Ensure HEALTHCHECK instructions have been added to the container image
[WARN]      * No Healthcheck found: [tp_meow-app:latest]
[WARN]      * No Healthcheck found: [redis:alpine]
[WARN]      * No Healthcheck found: [apache_custom:latest]
[WARN]      * No Healthcheck found: [nginx:latest]
[WARN]      * No Healthcheck found: [debian:latest]
[WARN]      * No Healthcheck found: [postgres:15-alpine]
[WARN]      * No Healthcheck found: [requarks/wiki:2]
[WARN]      * No Healthcheck found: [alpine:latest]
[WARN]      * No Healthcheck found: [aquasec/trivy:0.49.1]
[INFO] 4.7  - Ensure update instructions are not use alone in the Dockerfile
[INFO]      * Update instruction found: [tp_meow-app:latest]
[INFO]      * Update instruction found: [apache_custom:latest]
[NOTE] 4.8  - Ensure setuid and setgid permissions are removed in the images
[INFO] 4.9  - Ensure COPY is used instead of ADD in Dockerfile
[INFO]      * ADD in image history: [redis:alpine]
[INFO]      * ADD in image history: [postgres:15-alpine]
[INFO]      * ADD in image history: [requarks/wiki:2]
[INFO]      * ADD in image history: [alpine:latest]
[INFO]      * ADD in image history: [aquasec/trivy:0.49.1]
[INFO]      * ADD in image history: [docker/docker-bench-security:latest]
[NOTE] 4.10  - Ensure secrets are not stored in Dockerfiles
[NOTE] 4.11  - Ensure verified packages are only Installed


[INFO] 5 - Container Runtime
[PASS] 5.1  - Ensure AppArmor Profile is Enabled
[WARN] 5.2  - Ensure SELinux security options are set, if applicable
[WARN]      * No SecurityOptions Found: tp_meow-db-1
[WARN]      * No SecurityOptions Found: tp_wikijs-wiki-1
[WARN]      * No SecurityOptions Found: tp_wikijs-db-1
[PASS] 5.3  - Ensure Linux Kernel Capabilities are restricted within containers
[PASS] 5.4  - Ensure privileged containers are not used
[PASS] 5.5  - Ensure sensitive host system directories are not mounted on containers
[PASS] 5.6  - Ensure ssh is not run within containers
[PASS] 5.7  - Ensure privileged ports are not mapped within containers
[NOTE] 5.8  - Ensure only needed ports are open on the container
[PASS] 5.9  - Ensure the host's network namespace is not shared
[WARN] 5.10  - Ensure memory usage for container is limited
[WARN]      * Container running without memory restrictions: tp_meow-db-1
[WARN]      * Container running without memory restrictions: tp_wikijs-wiki-1
[WARN]      * Container running without memory restrictions: tp_wikijs-db-1
[WARN] 5.11  - Ensure CPU priority is set appropriately on the container
[WARN]      * Container running without CPU restrictions: tp_meow-db-1
[WARN]      * Container running without CPU restrictions: tp_wikijs-wiki-1
[WARN]      * Container running without CPU restrictions: tp_wikijs-db-1
[WARN] 5.12  - Ensure the container's root filesystem is mounted as read only
[WARN]      * Container running with root FS mounted R/W: tp_meow-db-1
[WARN]      * Container running with root FS mounted R/W: tp_wikijs-wiki-1
[WARN]      * Container running with root FS mounted R/W: tp_wikijs-db-1
[WARN] 5.13  - Ensure incoming container traffic is binded to a specific host interface
[WARN]      * Port being bound to wildcard IP: 0.0.0.0 in tp_wikijs-wiki-1
[WARN] 5.14  - Ensure 'on-failure' container restart policy is set to '5'
[WARN]      * MaximumRetryCount is not set to 5: tp_meow-db-1
[WARN]      * MaximumRetryCount is not set to 5: tp_wikijs-wiki-1
[WARN]      * MaximumRetryCount is not set to 5: tp_wikijs-db-1
[PASS] 5.15  - Ensure the host's process namespace is not shared
[PASS] 5.16  - Ensure the host's IPC namespace is not shared
[PASS] 5.17  - Ensure host devices are not directly exposed to containers
[INFO] 5.18  - Ensure the default ulimit is overwritten at runtime, only if needed
[INFO]      * Container no default ulimit override: tp_meow-db-1
[INFO]      * Container no default ulimit override: tp_wikijs-wiki-1
[INFO]      * Container no default ulimit override: tp_wikijs-db-1
[PASS] 5.19  - Ensure mount propagation mode is not set to shared
[PASS] 5.20  - Ensure the host's UTS namespace is not shared
[PASS] 5.21  - Ensure the default seccomp profile is not Disabled
[NOTE] 5.22  - Ensure docker exec commands are not used with privileged option
[NOTE] 5.23  - Ensure docker exec commands are not used with user option
[PASS] 5.24  - Ensure cgroup usage is confirmed
[WARN] 5.25  - Ensure the container is restricted from acquiring additional privileges
[WARN]      * Privileges not restricted: tp_meow-db-1
[WARN]      * Privileges not restricted: tp_wikijs-wiki-1
[WARN]      * Privileges not restricted: tp_wikijs-db-1
[WARN] 5.26  - Ensure container health is checked at runtime
[WARN]      * Health check not set: tp_meow-db-1
[WARN]      * Health check not set: tp_wikijs-wiki-1
[WARN]      * Health check not set: tp_wikijs-db-1
[INFO] 5.27  - Ensure docker commands always get the latest version of the image
[WARN] 5.28  - Ensure PIDs cgroup limit is used
[WARN]      * PIDs limit not set: tp_meow-db-1
[WARN]      * PIDs limit not set: tp_wikijs-wiki-1
[WARN]      * PIDs limit not set: tp_wikijs-db-1
[PASS] 5.29  - Ensure Docker's default bridge docker0 is not used
[PASS] 5.30  - Ensure the host's user namespaces is not shared
[PASS] 5.31  - Ensure the Docker socket is not mounted inside any containers


[INFO] 6 - Docker Security Operations
[INFO] 6.1  - Avoid image sprawl
[INFO]      * There are currently: 11 images
[INFO] 6.2  - Avoid container sprawl
[INFO]      * There are currently a total of 9 containers, with 4 of them currently running


[INFO] 7 - Docker Swarm Configuration
[PASS] 7.1  - Ensure swarm mode is not Enabled, if not needed
[PASS] 7.2  - Ensure the minimum number of manager nodes have been created in a swarm (Swarm mode not enabled)
[PASS] 7.3  - Ensure swarm services are binded to a specific host interface (Swarm mode not enabled)
[PASS] 7.4  - Ensure data exchanged between containers are encrypted on different nodes on the overlay network
[PASS] 7.5  - Ensure Docker's secret management commands are used for managing secrets in a Swarm cluster (Swarm mode not enabled)
[PASS] 7.6  - Ensure swarm manager is run in auto-lock mode (Swarm mode not enabled)
[PASS] 7.7  - Ensure swarm manager auto-lock key is rotated periodically (Swarm mode not enabled)
[PASS] 7.8  - Ensure node certificates are rotated as appropriate (Swarm mode not enabled)
[PASS] 7.9  - Ensure CA certificates are rotated as appropriate (Swarm mode not enabled)
[PASS] 7.10  - Ensure management plane traffic has been separated from data plane traffic (Swarm mode not enabled)

[INFO] Checks: 105
[INFO] Score: 17

```

Je le rajoute qd même comme ça je peux les avoirs plus tard ;) 

