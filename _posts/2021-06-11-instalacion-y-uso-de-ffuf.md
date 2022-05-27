---                                                                                                                                             
layout: post
title: "Instalación y uso de FFUF - Fuzzing" 
summary: "En esta ocasión te enseño como usar e instalar la herramienta FFUF para el Fuzzing"
author: r4z0r                                                                                                                                   
date: '2021-11-05 09:15:23 +0530'                                                                                                               
category: ['fuzzing','HackingTools']                                                                                  
thumbnail: /assets/img/posts/fuzzing/ffuf/fondo.jpg
keywords: fuzzing, hacking, hacking tools, ffuf, FFUF                                               
permalink: /blog/instalacion-y-uso-de-ffuf-fuzzing/                                                                                         
---


Hola!!😀 En este mini artículo estaremos viendo la instalación y uso de este increíble fuzeador creado en el lenguaje de Programación GO, FFUF.

Hace meses atrás siguiendo el canal de uno de los más reconocidos en el Pentesting, John Hammond utilizaba una herramienta llamada FFUF en uno de sus videos de YouTube, una vez que la ví y la probé fue amor a primera vista. Gracias a su sencillez, velocidad y su amplia variedad de opciones esta herramienta es mi favorita para realizar fuzzing y hoy te voy a mostrar cómo descargar y usarla.

## Instalación

Primero deben dirigirse al repositorio de [Github](https://github.com/ffuf/ffuf) y seguir los siguientes pasos:

![git](/assets/img/posts/fuzzing/ffuf/1.png "Repositorio de ffuf")

Copian esas lineas, lo que les va a permitir clonar el repositorio, moverse a la carpeta clonada, y realizan la compilación.

![download](/assets/img/posts/fuzzing/ffuf/2.jpg "Download")

```bash

cd /opt ; git clone https://github.com/ffuf/ffuf ; cd ffuf ; go get ; go build

```

![ffuf](/assets/img/posts/fuzzing/ffuf/3.png "Ffuf")

Como ven ya tenemos instalado ffuf. Ahora consultemos la ayuda con --help. Siempre les recomiendo que cuando no sepan utilizar una herramienta soliciten la ayuda con –help, o, man “herramienta”, man por cierto se refiere a manual.

![ffuf help](/assets/img/posts/fuzzing/ffuf/4.png "Ffuf --help")

Aquí se nos lista una gran variedad de opciones, que les dejo a ustedes a que la investiguen a fondo.

## Uso

¿Cómo usar la herramienta?, pues sencillo. Con -u le pasamos la URL víctima y -w el diccionario a utilizar. Asegurense de colocar la palabra FUZZ, donde quieren realizar el fuzzing.

![ffuf uso](/assets/img/posts/fuzzing/ffuf/5.png "Ffuf uso")

En este caso para la prueba utilicé metasploitable que está corriendo sobre mi red nat, y para el diccionario utilice common.txt, que es uno de los más efectivos. En cuestión 5 segundos ya terminó el escaneo, obviamente estamos en un entorno controlado, pero en un entorno real se utilizarían otros parámetros para no provocar DDoS o falsos positivos.

También pueden ir jugando con -t para agregarle hilos aunque no les recomiendo que se pasen de los 300 en retos CTFS (EN UN ENTORNO REAL NO ES RECOMENDABLE PASARSE LOS 10, TODO DEPENDE LA CRITICIDAD DEL HOST) ya que pueden saturar al servidor con peticiones y provocarían una denegación de servicios. Como notaron es súper sencillo su uso y lo mejor de todo es que es súper rápido a comparación de otros fuzeadores. Me gustaría más adelante traerles una comparación con los diferentes fuzeadores conocidos.

Buenísimo si llegaste hasta acá espero que te haya gustado esta herramienta y la puedas probar. Después me cuentas como te ha ido con la herramienta y si ya vas a dejar de utilizar Wffuz y gobuster todavía 😂.

Compártelo con tus amigos y sigue aprendiendo de este hermoso mundo del hacking, te veo luego.
 
[R4z0r](https://juankaenel.github.io) 👨‍💻



