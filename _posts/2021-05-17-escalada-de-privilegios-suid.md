---                                                                                                                                             
layout: post
title:  "Escalada de privilegios en Linux a través de los permisos SUID"                                                                                        
summary: "En este artículo te enseño qué son los permisos SUID y como escalar privilegios a través de las mismas."
author: r4z0r                                                                                                                                   
date: '2021-05-17 11:15:23 +0530'                                                                                                               
category: ['linux','PrivEsc']                                                                                  
thumbnail: /assets/img/posts/escalada-de-privilegios/suid/suid.jpg
keywords: linux, escalada de privilegios, priv esc, SUID                                               
permalink: /blog/escalada-de-privilegios-a-traves-de-suid/                                                                                         
---

Bienvenido a éste artículo donde estaré enseñándote cómo escalar privilegios a través de los permisos SUID, ¡ponte cómodo, prendé tu Parrot o Kali y a darle!

Primero, antes de arrancar vamos a explicar que son los permisos SUID. 

## ¿Qué son los permisos SUID?

Los permisos SUID son un tipo especial de permisos que cuando son asignados, estos tienen todos los mismos permisos que el usuario que lo creo, es decir los mismos permisos que su propietario.

Son también llamados permisos setuid y se asignan sumándole 4000 a la representación octal de los permisos de un fichero, otorgándole además, permiso de ejecución al propietario del mismo.

Cuando el permiso setuid está prendido o activado en un fichero, el usuario que ejecute el mismo tendrá durante la ejecución los mismos privilegios que el propietario del fichero.
Por ejemplo, si el usuario root(administrador) crea un fichero y le activa el permiso SUID, todo usuario que lo ejecute dispondrá de privilegios de administrador hasta que el programa finalice.

Es aquí cuando podemos aprovecharnos de los permisos SUID como atacantes y es también aquí de vital importancia si eres administrador de sistemas corroborar qué binarios tienen asignados éste tipo de permiso.

Pasemos ahora al parrot donde mostraré el ejemplo en modo práctico:

Abrimos nuestro navegador y nos dirigimos a la página de GTFObins. Acá vamos a filtrar por permisos SUID.

![SUID](/assets/img/posts/escalada-de-privilegios/suid/1.png "SUID GTFObins")

En éste ejemplo vamos a trabajar con el binario **env**, así que lo buscamos y vemos cómo explotar al mismo.

![SUID 2](/assets/img/posts/escalada-de-privilegios/suid/2.png "SUID GTFObins 2")

Como nos ilustra la imágen debemos ejecutar el siguiente código:

- **env /bin/sh -p**

Ahora pasemos a la terminal, lo primero que hacemos es asegurar que somos root para poder asignarle los permisos SUID (esto es sólo para mostrar el ejemplo ya que en un reto CTF o auditoría, para poder explotar esta vulnerabilidad nosotros debemos encontrar el binario que tenga activado este permiso y poder explotarlo)

![asignando suid](/assets/img/posts/escalada-de-privilegios/suid/3.png "asiganmos suid")

Una vez que verificamos que somos root, ubicamos la ruta donde se encuentra el binario **env** y a la par hacemos un xargs para que se nos haga un listado largo de este fichero (xargs permite ejecutar un comando a la par).

Posteriormente con chmod **4**755 le asignamos el permiso SUID a través del “4” y 755 para los permisos de propietario, grupo y otros. Ahora pasemos a verificar que el permiso SUID se activó corroborando el bit “s”.

![suid asignado](/assets/img/posts/escalada-de-privilegios/suid/4.png "suid asignado")

Podemos ver que el bit “s” está prendido, ahora ya tenemos nuestro permiso **SUID** activado.

El último paso es ejecutar el comando que nos permita pasar de un usuario normal a **root** sin proporcionar contraseña en este caso abusandonos del permiso **SUID** del fichero **env**.

![env](/assets/img/posts/escalada-de-privilegios/suid/5.png "env")

Como se observa, el usuario R4z0r ejecutando el código anteriormente mencionado puede escalar privilegios y convertirse en usuario root sin proporcionar contraseña. Todo esto gracias a que el permiso SUID otorga permisos del propietario que lo creo en este caso el usuario root, por lo cual el usuario r4z0r puede ejecutar de forma temporal este binario como usuario root  y escalar privilegios.

Bien, si has llegado hasta acá ¡¡felicitaciones!! Pero ahora nos falta algo, ¿cómo encontramos los binarios que posean permisos SUID?

Para poder encontrar binarios con permisos SUID podemos hacerlo de las siguientes formas:

![find](/assets/img/posts/escalada-de-privilegios/suid/6.png "find suid")

A través de:

- **find / -perm -4000 2>/dev/null**

O también existe la otra opción:

![find suid 2](/assets/img/posts/escalada-de-privilegios/suid/7.png "find suid 2")

- **find / -type f -u+s 2>/dev/null**

Ambos comandos ejecutan la misma acción, encontrar binarios con permisos SUID, buscando desde la raíz “/”. Con 2>/dev/null redirigimos la salida de errores hacia el agujero negro, es una forma de que no se nos liste errores por pantalla. El número 4000 hace referencia a los permisos SUID.

Bien ahora sí, ya contás con todas las herramientas necesarias para poder escalar privilegios a través  del abuso de los permisos SUID.

Espero que te haya gustado este artículo y que puedas haber comprendido. Compártelo con tus amigos y sigue aprendiendo de este hermoso mundo del hacking, te veo luego.

[R4z0r](https://juankaenel.github.io) 👨‍💻



