---                                                                                                                                             
layout: post
title:  "Escalada de privilegios en Linux a través de Capabilities"                                                                                        
summary: "En este artículo te enseño qué son las capabilities y como escalar privilegios a través de las mismas."
author: r4z0r                                                                                                                                   
date: '2021-05-21 11:15:23 +0530'                                                                                                               
category: ['linux','PrivEsc']                                                                                  
thumbnail: /assets/img/posts/escalada-de-privilegios/capabilities/capabilities.jpg
keywords: linux, escalada de privilegios, priv esc, capabilities                                               
permalink: /blog/escalada-de-privilegios-a-traves-de-capabilities/                                                                                         
---


Hola y bienvenido a este artículo donde estaremos viendo qué son las capabilities, cómo poder abusarnos y explotarlas para poder escalar privilegios a través de las mismas.

## Introducción

Para que un usuario pueda ejecutar algunas acciones específicas, es necesario que este usuario sin privilegios a veces adquiera temporalmente un perfil de superusuario para poder lograr ejecutar dichas acciones.

Esta funcionalidad se puede lograr asignando privilegios a través de **sudo o permisos setuid (SUID)** a un archivo ejecutable que permite al usuario adoptar el rol de propietario del archivo. Permisos que los estuvimos mencionando en artículos anteriores.

Para realizar la misma tarea de una manera más segura, un administrador de sistema puede utilizar capabilities donde estas desempeñan un papel eficaz en la seguridad de los sistemas operativos basados ​​en Linux.
	
## ¿Qué son las capabilities?

Son un tipo de permiso que permite dividir a los privilegios de root en distintos valores. Estos valores pueden ser asignados de forma independiente a procesos de modo que para realizar una operación privilegiada, ese proceso cuente únicamente con el permiso necesario sin tener que asumir la identidad de superusuario. Exactamente eso son las capabilities, permisos especiales que permiten ejecutar una acción privilegiada sin tener que asumir el rol del superusuario.

Existen muchos tipos de capabilities que los puedes ver en este **[sitio](https://wiki.gentoo.org/wiki/Hardened/Overview_of_POSIX_capabilities)**.

## ¿Cuál es la diferencia entre las capabilities y los permisos Setuid (SUID)?

Si bien ambos son permisos especiales y realizan casi las mismas acciones, la diferencia es que los permisos SUID permiten a un usuario que ejecute un binario con estos permisos asignados asumir temporalmente el rol del propietario de dicho binario. Si el propietario de ese binario, es el usuario root es decir el superusuario, cuando un usuario sin privilegios ejecute el binario puede ejecutarlo temporalmente como usuario root. Es ahí donde se pueden aprovechar de algunos binarios y escalar privilegios. Concepto que lo estuvimos viendo en el artículo anterior. """"" PONER ENLACE OTRO ARTÍCULO

Ahora pasando a las capabilities, un usuario no toma el rol de el propietario cuando ejecuta un binario que tenga asignada capabilites, ya que estos sólo le permiten ejecutar ciertas acciones privilegiadas pero no asumir el rol del superusuario.

## Algunos comandos utilizados con las capabilities:

- **getcap:** Lista las capabilities de un fichero.

- **setcap:** Asigna/borra capabilities a un fichero.

- **getpcaps:** Lista las capabilities de un proceso.

- **capsh:** Proporciona una interfaz de línea de comandos para probar y explorar el uso de capabilities.

## Comandos más específicos:

### Asignar la capabilitie cap_setuid a un binario específico

- **setcap cap_setuid+ep /ruta/del/binario**

### Remover capabilities

Especificamos con la flag -r para remover todas las capabilites del binario/fichero.

- **setcap -r /ruta/del/binario** 

### Buscar binarios con capabilities asignadas

Buscar de forma recursiva en todo el sistema de archivos cualquier fichero con capabilities:

- **getcap -r / 2>/dev/null**

Por cierto, si no conoces qué es **2>/dev/null** esto hace referencia a que se va a mandar toda la salida de errores que produce este comando a un agujero negro, de modo de no ver los errores en la terminal que produce éste comando.

Bien, ya aprendiste cómo asignar, remover y buscar capabilities. Ahora pasemos a lo más divertido e importante, cómo abusar y explotar las capabilities de un fichero para poder escalar privilegios.

---

## Abuso y explotación de las capabilities para escalar privilegios

Vamos a trabajar con la capabilitie **setuid**. Esta capabilitie nos va a permitir cambiar la **UID (identificador único de usuario)**, en este caso cambiar el UID a 0, donde este responde al UID del usuario **root**. Trataremos en el ejemplo el binario de python, nos dirigimos a la página que ya estuvimos viendo en artículos anteriores, [GTFObins](https://gtfobins.github.io/).

Filtramos por capabilities y buscamos el binario python.

![capabilities-gtfobins](/assets/img/posts/escalada-de-privilegios/capabilities/1.png "gtfobins")

![cap-setuid](/assets/img/posts/escalada-de-privilegios/capabilities/2.png "cap setuid")

Cómo se puede observar tenemos que ejecutar lo siguiente:

```python

sudo setcap cap_setuid+ep /usr/bin/python
python -c 'import os; os.setuid(0); os.system("/bin/sh")'

```

Una vez asignada la capabilitie debemos ejecutar el comando que nos permite setear el UID a 0 y posteriormente llamar a una shell. Esto nos va a permitir setear el id del usuario actual al ID del root, tomando así el rol del superusuario.

Pasemos al parrot y mostremos el ejemplo en el mismo.

![set-get](/assets/img/posts/escalada-de-privilegios/capabilities/3.png "python")

Estamos en modo root y asignamos y listamos la capabilite al binario python 3.9 lo podemos hacer también a través de sudo si tenemos permiso.

Ahora ejecutemos el comando que nos permite escalar privilegios siendo un usuario normal.

![root](/assets/img/posts/escalada-de-privilegios/capabilities/4.png "root")

Y como se ve en la imágen, ¡somos usuario root sin proporcionar contraseña!

Existen diversos binarios que nos permiten escalar privilegios a través de las capabilities, te invito a que las revises en la página de GTFObins.

En una situación de auditoría o CTF, podemos toparnos con capabilities asignadas a binarios donde estos nos permiten escalar privilegios. Para buscar dichos binarios con estas capabilities lo puedes hacer con el código anteriormente mencionado:

- **getcap -r / 2>/dev/null**

## Cierre y despedida del artículo

Ya con todos estos conceptos estás listo para protegerte de posibles vectores de ataques y también cómo escalar privilegios a través de las capábilities en sistemas Linux, ¡felicidades!

Espero que te haya gustado este artículo y que puedas haber aprendido sobre las capabilities y cómo abusar de ellas. Compártelo con tus amigos y sigue aprendiendo de este hermoso mundo del hacking, te veo luego

[R4z0r](https://juankaenel.github.io) 👨‍💻



