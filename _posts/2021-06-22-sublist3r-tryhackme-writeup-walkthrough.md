---
layout: post
title:  "Sublist3r – TryHackMe – Writeup/Walkthrough "
summary: "Aprendemos a utilizar Sublist3r y resolvemos un desafío de la plataforma de TryHackMe"
date: '2021-06-22 10:35:23 +0530'
category: ['tryhackme','HackingTools','writeup','walkthrough']
thumbnail: /assets/img/posts/sublist3r/fondo.jpg
keywords: tryhackme, sublist3r, writeup, walkthrought 
permalink: /blog/sublist3r-tryhackme-writeup-walkthrough/
author: r4z0r
---

Buenas buenas para todos, hoy domingo toca día de estudiar y compartir conococimientos con todos ustedes, hoy les traigo un Writeup de Sublist3r, un desafío de TryHackMe que nos enseña a instalar y a utilizar esta excelente herramienta para encontrar subdominios sobre un objetivo.

Antes que nada, quiero agradecer a los instructores de Open-Sec por darme a conocer de esta herramienta y enseñarme a como utilizarla.

![sublist3r](/assets/img/posts/sublist3r/1.png)

## Tarea 1 – Instalación

Para la instalación de esta herramienta deben seguir estos pasos:

```bash
cd /opt
git clone https://github.com/aboul3la/Sublist3r.git
cd Sublist3r
pip3 install -r requirements.txt

```
## Tarea 2 – Switchs

La verdad que esta herramienta es súper sencilla de utilizar y tirando un -h ya se nos lista toda la ayuda.

![sublist3r help](/assets/img/posts/sublist3r/2.png)

**What switch can we use to set our target domain to perform recon on?**

**Bueno acá nos pregunta cuál switch, flag, o argumento como lo quieran llamar se utilizar para enumerar.**

	 Respuesta: -d

**How about setting which engines we’ll use for searching? (i.e. google, bing, etc)**

**Nos pregunta cuál es el switch para setear el motor de búsqueda a utilizar.**

	Respuesta: -e

**Saving our output is important both so we don’t have to run recon again but also so we can return to our returns and review them at a later time. What switch do we use to define an output file?**

**Nos dice que guardar nuestra salida o búsqueda es importante, que no seas cabeza dura y le apliques un switch para guardar la información y no vuelvas a realizar la búsqueda.😆 ¿Cuál es este switch, flag o argumento?**

	Respuesta: -o


**Sublist3r can sometimes take some time to run but we can speed through up the use of threads. Which switch allows us to set the number of threads?**

**Bueno acá hace mención a que esta herramienta permite fuerza bruta, y que en algunas ocasiones nos va a ser de mucha ayuda, ¿con que argumento seteamos el número de hilos?**

	Respuesta: -t

**Last but not least, we can also bruteforce the domains for our target. This isn’t always the most useful, however, it can sometimes find a key domain that we might have missed. What switch allows us to enable brute forcing?**

**Por si no sabías esta herramienta te permite realizar fuerza bruta, una genialidad ya que hay veces que no vamos a poder obtener los resultados que queremos pero podríamos realizar la creación de un diccionario específico para nuestro objetivo y con esta herramienta hacer fuerza bruta para detectar subdominios a través de nuestro diccionario, bueno, esto no dice lo de arriba pero decidí darte un ejemplo que te puede ser útil algún día. Ahora la pregunta importante es ¿Con qué argumento podemos indicarle que queremos utilizar fuerza bruta?**

	Respuesta: -b

## Tarea 3 – ¡Escanea!

Es hora de escanear, vamos a ejecutar Sublit3r contra el dominio de una empresa de destino y aprenderemos sobre algunos dominios comunes! También podemos ejecutar esto a través de la herramienta de reconocimiento en este [link](https://dnsdumpster.com/) o sino podemos descargar los resultados de la búsqueda del creador, cosa que sería trampa!!.

Ok, primer paso, realizar el escaneo contra nuestro objetivo en este caso nbc.com,  una empresa de noticias estadounidense bastante grande.

![ejecutando sublist3r](/assets/img/posts/sublist3r/3.png) 

```bash

python3 sublist3r.py -d nbc.com -o sub-output-nbc.txt

```

Recordar: -d para indicarle el dominio objetivo y con -o para guardar el resultado de nuestra búsqueda


**Email domains are almost always interesting and typically have an email portal (usually Outlook) located at them. Which subdomain is likely the email portal?**

**Los dominios de correo electrónico son casi siempre interesantes y, por lo general, tienen un portal de correo electrónico (generalmente Outlook) ubicado en ellos. ¿Qué subdominio es probablemente el portal de correo electrónico?**

![sublist3r nbc](/assets/img/posts/sublist3r/4.png)

	Respuesta: mail

**Administrative control panels should never be exposed to the internet! Which subdomain is exposed that shouldn’t be?**

**Los paneles de control administrativo nunca deben estar expuestos a Internet. ¿Qué subdominio está expuesto que no debería estarlo?**

Bien, para esta respuesta no encontraba la solución, al parecer ya ocultaron este subdominio, le cambiaron el nombre u otra acción, por lo cual recordé que el creador ya había obtenido los resultados de su búsqueda y nos daba la opción de descargarla, filtrando por la palabra “adm” obtuve la solución.

![sub output nbc](/assets/img/posts/sublist3r/5.png)

	Respusta: admin


**Company blogs can sometimes reveal information about internal activities, which subdomain has the company blog at it?**

**Los blogs de la empresa a veces pueden revelar información sobre actividades internas, ¿qué subdominio tiene el blog de la empresa?**

![blog nbc](/assets/img/posts/sublist3r/6.png)

	Respuesta: blog

**Two developer sites are exposed, which one is associated directly with web development?**

**Se exponen dos sitios para desarrolladores, ¿cuál está asociado directamente con el desarrollo web?**

![dev nbc](/assets/img/posts/sublist3r/7.png) 

**¿Which dns record might be a helpdesk portal?**

**¿Qué registro dns podría ser un portal de la mesa de ayuda?**


![dns nbc](/assets/img/posts/sublist3r/8.png)

	Respuesta: help

**Single sign-on is a feature commonly used in corporate domains, which dns record is directly associated with this feature? Include both parts of this subdomain separated by a period.**

**El inicio de sesión único es una característica que se usa comúnmente en los dominios corporativos, ¿qué registro dns está directamente asociado con esta característica? Incluya ambas partes de este subdominio separadas por un punto.**

La verdad que no sabía a que hacia mención single sing-on entonces me puse a investigar al respecto y luego encontré su utilidad y que lo abrevian con las siglas “sso”.

SSO es un sistema de autenticación de sesión único que permite a un usuario acceder a múltiples recursos y aplicaciones a través de un único Login.

Permite acceder a más de un servicio completando, una sola vez, todos los datos personales. Por ejemplo, cuando nos registramos en un sitio web con la cuenta de Google, esto nos  permite identificarnos una sola vez y mantener nuestra sesión abierta sin volver a loguearnos.

![sso nbc](/assets/img/posts/sublist3r/9.png) 	

	Respuesta: ssologin.stg

**NBC produced a popular sitcom about typical office work environment, which dns record might be associated with this show?**

**NBC produjo una comedia de situación popular sobre el entorno de trabajo de oficina típico, ¿qué registro de dns podría estar asociado con este programa?**

La verdad que me mataron con esa pregunta, así que recurrí a la ayuda.

![office nbc](/assets/img/posts/sublist3r/10.png)

Por lo visto la película se llamaba The Office, ok, vamos a ver el resultado.

![office-words nbc](/assets/img/posts/sublist3r/11.png)

	Respuesta: office-words


Espero que te haya gustado este artículo y hayas aprendido mucho. La verdad que está increíble esta herramienta, súper recomendada. Te veo luego.


[R4z0r](https://juankaenel.github.io)👨‍💻


