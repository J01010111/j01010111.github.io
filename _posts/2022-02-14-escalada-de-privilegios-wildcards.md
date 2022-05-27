---                                                                                                                                             
layout: post
title:  "Escalada de privilegios con inyección de Wildcard"                                                                                        
summary: "En este artículo te enseño que son los cron jobs, inyección de wildcard y cómo escalar privilegios a través de los mismos."
author: Agus                                                                                                                                   
date: '2022-02-15 11:15:23 +0530'                                                                                                               
category: ['linux','PrivEsc']                                                                                  
thumbnail: /assets/img/posts/escalada-de-privilegios/wildcards/wild.png
keywords: linux, escalada de privilegios, priv esc, wildcards                                               
permalink: /blog/escalada-de-privilegios-a-traves-de-wildcards/                                                                                         
---

La escalabilidad es una parte fundamental en una prueba de penetración, el proceso de explotar vulnerabilidades o configuraciones incorrectas con el objetivo de obtener acceso root permitirá al atacante tomar por completo el control del sistema e incluso mantener acceso. Es por esto, que un Penetration Tester debe tener la habilidad de detectarlas a tiempo para evitar este tipo de ataques mal intencionados.

Como es sabido, al momento de escalar privilegios contamos con múltiples formas de hacerlo, todo va a depender de nuestro objetivo y las fallas de configuración con las que este cuente. 

En esta ocasión vamos a tratar una falla de configuración bastante usual en los “cron jobs”, específicamente en los trabajos que ejecutan comandos o scripts con wildcards. 

---

## ¿Qué son los cron jobs?

Cuando trabajamos con sistemas GNU/Linux debemos recordar que estos frecuentemente se utilizan en servidores, entonces, resulta muy importante tener la capacidad de automatizar y programar ciertas tareas repetitivas como, por ejemplo, las copias de seguridad.

Cron nos permite ejecutar programas o scripts periódicamente y el registro de estas tareas programadas se van a almacenar en el archivo */etc/crontab*.

---

## ¿Qué son los wildcards?

Los wildcards son caracteres especiales que se emplean como comodines de búsqueda, hay varios tipos de wildcards, pero nos centraremos en el asterisco (*).

Para entender como funcionan veámoslo con dos ejemplos sencillos:

1. Este comando va a listar el contenido que empiece con el prefijo “a” y terminara con cualquier carácter.

    ![comando1](/assets/img/posts/escalada-de-privilegios/wildcards/1.png)

2. Así mismo podemos listar el contenido que empiece con el prefijo “a”, que en medio complete con caracteres y termine con la extensión sh.

    ![comando2](/assets/img/posts/escalada-de-privilegios/wildcards/2.png)


## ¿De qué se trata esta inyección?

La inyección abusa de la capacidad del wildcard, el problema ocurre cuando se ejecuta un comando con uno de estos y hay un archivo en el directorio con el nombre de un indicador, entonces, el comando malinterpreta el nombre del archivo como un indicador del propio comando.

Veamos como funciona esto con un ejemplo:

- Como se puede apreciar en las siguientes imágenes, cuento con dos archivos, uno llamado script.sh que es el que ejecutara el comando con el wildcard y otro archivo con el nombre de --version, el cual se usara para ejecutarse como un indicador del comando almacenado en el archivo script.

    ![comando3](/assets/img/posts/escalada-de-privilegios/wildcards/3.png)

- El contenido del archivo script.sh es el siguiente.

    ![comando4](/assets/img/posts/escalada-de-privilegios/wildcards/4.png)

Entonces, si ejecutamos el programa, veremos que el wildcard completa el comando con el nombre del archivo --version, pero como este tiene de nombre un indicador, ejecutara el comando con el indicador y entonces, habremos inyectado un comando a través de un wildcard.

![script](/assets/img/posts/escalada-de-privilegios/wildcards/5.png)


## Escalando privilegios - Wildcard Injection:


Ahora que entendemos de que se trata la vulnerabilidad vamos a explotarla, para esto, voy a usar la máquina de TryHackMe llamada **Linux PrivEsc**. ([https://tryhackme.com/room/linuxprivesc](https://tryhackme.com/room/linuxprivesc)) para poder mostrarles esta técnica de escalada de privilegios que va a aprovecharse de los trabajos de cron que utilicen wildcards.

1. Verificamos que podemos leer el archivo /etc/crontab y que tenemos permisos de lectura.:

    ![crontab](/assets/img/posts/escalada-de-privilegios/wildcards/6.png)

2. Leemos el contenido dentro del archivo y notamos que tenemos la variable de entorno PATH y dos scripts que se ejecutan mediante Cron:

    ![crontab 2](/assets/img/posts/escalada-de-privilegios/wildcards/7.png)

3. Ahora, revisaremos el archivo que se encuentra en la ruta /usr/local/bin/compress.sh:

    ![compress](/assets/img/posts/escalada-de-privilegios/wildcards/8.png)

   Este script accede al directorio /home/user y luego verifica el tamaño del archivo /tmp/backup.tar.gz y de los archivos que se encuentren en la carpeta con un wildcard. Es en este punto, en el que nos aseguramos de que se cumplen todos los parámetros para que podamos realizar la inyección.

4. Como ya nos encontramos en el directorio /home/user no hace falta que nos movamos, entonces desde acá creamos el script:

    ![printf](/assets/img/posts/escalada-de-privilegios/wildcards/9.png)

    **Linea 1:** Crea el archivo “*shell.sh”* que da permisos SUID al archivo */bin/bash*.

    **Linea 2:** Crea el archivo “--checkpoint-action=exec=sh shell.sh” que se inyectara como un indicador del comando tar y permitirá ejecutar el archivo “*shell.sh”*.

5. Como habíamos explicado, debemos esperar a que cron ejecute el script para que se haga efectiva nuestra inyección, para verificar  el cambio, haremos:

    ![ls bash](/assets/img/posts/escalada-de-privilegios/wildcards/10.png)

6. Y finalmente, una vez que nuestro archivo bash tenga permisos de ejecución SUID, podremos ejecutarlo con privilegios elevados y así obtener acceso root.

    ![root](/assets/img/posts/escalada-de-privilegios/wildcards/11.png)

## Video demostrativo:

Por último, me tomé el tiempo de hacer un breve video mostrándoles como se realiza la inyección. Recomiendo poner el video en x2.

<iframe width="560" height="315" src="https://www.youtube.com/embed/ZOXNegI4ZkY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

> “Reproducido de “Escalar privilegios con inyección de Wildcard”, por Agustin J. Tedone, 2022 (https://juankaenel.github.io/). Todos los derechos reservados por Copyright con permiso del autor.“

### Fuentes:

- Ahmed, Alexis. (2021). *Privilege Escalation Techniques.* Packt*.* [https://www.packtpub.com/product/privilege-escalation-techniques/9781801078870](https://www.packtpub.com/product/privilege-escalation-techniques/9781801078870)

- Gustavo. (2021). *Qué son las wildcards y para qué sirven*.  [https://apunteimpensado.com/wildcards-que-son-para-que-sirven/](https://apunteimpensado.com/wildcards-que-son-para-que-sirven/)

- Lee Vicky. (2020). *Becoming Root With Wildcard Injections on Linux.* [https://betterprogramming.pub/becoming-root-with-wildcard-injections-on-linux-2dc94032abeb](https://betterprogramming.pub/becoming-root-with-wildcard-injections-on-linux-2dc94032abeb)

Bueno hasta acá llego el artículo, espero te haya gustado y hayas aprendido mucho.

Te dejo mis redes sociales para que me conozcas:

- [Facebook](https://www.facebook.com/TedoneAgustin/)

- [Linkedin](https://www.linkedin.com/in/agustintedone/)

- [YouTube](https://www.youtube.com/c/AgustinTedone)

Un saludo grande, Agus😃
