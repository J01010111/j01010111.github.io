---                                                                                                                                             
layout: post
title:  "Abuso y explotación del sudoers para escalar privilegios"                                                                                        
summary: "En este artículo te enseño qué es el fichero sudoers y como escalar privilegios a través del mismo"
author: r4z0r                                                                                                                                   
date: '2021-05-15 11:15:23 +0530'                                                                                                               
category: ['linux','PrivEsc']                                                                                  
thumbnail: /assets/img/posts/escalada-de-privilegios/sudoers/fondo.jpg
keywords: linux, escalada de privilegios, priv esc, sudoers, SUDOERS                                               
permalink: /blog/escalada-de-privilegios-a-traves-de-sudoers/                                                                                         
---

Una vez que logramos acceso al sistema y tenemos el rol de un usuario con bajos privilegios uno de las cosas más comúnes que nos toca realizar es escalar privilegios. **Pero Juan, ¿para qué tengo que escalar privilegios si ya estoy dentro del sistema? Pues bien, justamente para tener acceso a lugares donde ese usuario no tiene o realizar ciertas acciones como borrar archivos o crear, por ejemplo en linux, si somos root tenemos acceso a prácticamente todo el sistema, mientras tanto un usuario juancito con pocos privilegios no puede realizar ciertas acciones como crear, eliminar y administrar cuentas de otros usuarios por ejemplo**. 

Existen muchas técnicas para escalar privilegios y una de ellas que podemos realizar es el Abuso y explotación del sudoers. Pero primero pasemos a explicar que es el fichero sudoers.

 
## El fichero sudoers


Sudoers es un fichero que contiene una lista de los usuarios que pueden ejecutar el comando sudo y cuáles son los alcances de sus privilegios con el mismo. Cuando un usuario ejecuta un comando precedido por sudo, el sistema busca en el archivo en la ruta /etc/sudoers y los archivos en /etc/sudoers.d para comprobar la información allí descrita y otorgar o denegar el permiso para usar el comando.

A continuación te voy a mostrar la estructura básica del fichero sudoers:

```bash

quién dónde = (como_quién) qué

# Por ejemplo, un usuario con privilegios administrativos absolutos —como root— tendrá la siguiente estructura:

root    ALL=(ALL)       ALL
```

Considerando que ALL significa "todos", esto quiere decir que la regla aplica al usuario root, en todos los hosts, root puede ejecutar comandos como todos los usuarios y se pueden ejecutar todos los comandos.

**Algo que aprendí al escribir este artículo es que el fichero sudoers debe modificarse con el comando visudo, no es obligación pero si es una recomendación de seguridad. Esto sucede debido a que visudo asegura que una sola persona con los permisos adecuados esté editando el archivo al mismo tiempo. Además de esto, tampoco  permite reescribir los cambios y salir si hay algún error en la edición del archivo asegurando una sintáxis correcta en la misma.**

Conociendo esto pasemos a lo que vinimos, abusar del sudoers para escalar privilegios.

## Abuso y explotación del sudoers

Lo primero que debemos realizar entonces el ejecutar visudo, automáticamente se nos va a abrir lo siguiente.

![sudoers](/assets/img/posts/escalada-de-privilegios/sudoers/1.png "sudoers")

Como podemos ver, así se debería ver de forma normal al fichero sudoers. Ahora vamos a pasar a modificarlo para mostrar el ejemplo. En este caso vamos a agregar permisos al binario find.

![modificando-sudoers](/assets/img/posts/escalada-de-privilegios/sudoers/2.png "modificando sudoers")

Lo que nos dice la linea 21 agregada es que el usuario r4z0r va a poder ejecutar el binario find sin proporcionar contraseña. Ya con este permiso asignado ahora podemos dirigirnos al sitio de [GTFOBins](https://gtfobins.github.io/) .

En estio sitio van a encontrar muchas formas de escalar privilegios, a través de capabilities, sudo, suid y muchos otras técnicas que nos pueden ayudar en el día a día como pentesters.

![gtfobins](/assets/img/posts/escalada-de-privilegios/sudoers/3.png "GTFobins")

Se observa que la página nos dice que si el binario tiene permisos sudo para ejecutarse se puede escalar privilegios, por lo cual procedemos a ejecutarlo…

![root](/assets/img/posts/escalada-de-privilegios/sudoers/4.png "root")

Se puede ver que ahora ejecutando el código proporcionado por la página mencionada, elevamos privilegios y somos usuarios root sin proporcionar contraseña. En el caso que estén frente a un reto CTF o cualquier situación que se les presente la necesidad de escalar privilegios pueden verificar qué binarios tienen permisos asignados en el sudoers con el siguiente código:

![sudo -l](/assets/img/posts/escalada-de-privilegios/sudoers/5.png "sudo -l")

Con **sudo -l**, listamos todos los binarios que estén asignados en el sudoers con permisos asignados, entonces dando un ejemplo de un reto CTF, lo primero que pueden realizar cuando quieran escalar privilegios es hacer un sudo -l, verificar si hay binarios que se puedan ejecutar en modo root sin proporcionar contraseña, posteriormente verificar en [GTOBins](https://gtfobins.github.io/) si el binario se puede explotar para escalar privilegios y pa’ dentro! 😃

Para entrar más a detalle con este comando te dejo el siguiente enlace: [man sudoers](https://linux.die.net/man/5/sudoers)

Hasta acá vimos cómo abusarnos del fichero sudoers y cómo escalar privilegios a través del binario find, existen muchos otros binarios con los que se puede elevar privilegios sólo basta con filtrar en [GTFOBins](https://gtfobins.github.io/)  por binarios sudo y con eso se nos listaría todos los que presentan oportunidad para elevar privilegios a través de la misma.

Espero que te haya gustado este artículo y que puedas haber comprendido. Compártelo con tus amigos y sigue aprendiendo de este hermoso mundo del hacking, te veo luego .

[R4z0r](https://juankaenel.github.io) 👨‍💻



