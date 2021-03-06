---                                                                                                                                             
layout: post
title: "Instalaci贸n y uso de FFUF - Fuzzing" 
summary: "En esta ocasi贸n te ense帽o como usar e instalar la herramienta FFUF para el Fuzzing"
author: r4z0r                                                                                                                                   
date: '2021-11-05 09:15:23 +0530'                                                                                                               
category: ['fuzzing','HackingTools']                                                                                  
thumbnail: /assets/img/posts/fuzzing/ffuf/fondo.jpg
keywords: fuzzing, hacking, hacking tools, ffuf, FFUF                                               
permalink: /blog/instalacion-y-uso-de-ffuf-fuzzing/                                                                                         
---


Hola!!馃榾 En este mini art铆culo estaremos viendo la instalaci贸n y uso de este incre铆ble fuzeador creado en el lenguaje de Programaci贸n GO, FFUF.

Hace meses atr谩s siguiendo el canal de uno de los m谩s reconocidos en el Pentesting, John Hammond utilizaba una herramienta llamada FFUF en uno de sus videos de YouTube, una vez que la v铆 y la prob茅 fue amor a primera vista. Gracias a su sencillez, velocidad y su amplia variedad de opciones esta herramienta es mi favorita para realizar fuzzing y hoy te voy a mostrar c贸mo descargar y usarla.

## Instalaci贸n

Primero deben dirigirse al repositorio de [Github](https://github.com/ffuf/ffuf) y seguir los siguientes pasos:

![git](/assets/img/posts/fuzzing/ffuf/1.png "Repositorio de ffuf")

Copian esas lineas, lo que les va a permitir clonar el repositorio, moverse a la carpeta clonada, y realizan la compilaci贸n.

![download](/assets/img/posts/fuzzing/ffuf/2.jpg "Download")

```bash

cd /opt ; git clone https://github.com/ffuf/ffuf ; cd ffuf ; go get ; go build

```

![ffuf](/assets/img/posts/fuzzing/ffuf/3.png "Ffuf")

Como ven ya tenemos instalado ffuf. Ahora consultemos la ayuda con --help. Siempre les recomiendo que cuando no sepan utilizar una herramienta soliciten la ayuda con 鈥揾elp, o, man 鈥渉erramienta鈥?, man por cierto se refiere a manual.

![ffuf help](/assets/img/posts/fuzzing/ffuf/4.png "Ffuf --help")

Aqu铆 se nos lista una gran variedad de opciones, que les dejo a ustedes a que la investiguen a fondo.

## Uso

驴C贸mo usar la herramienta?, pues sencillo. Con -u le pasamos la URL v铆ctima y -w el diccionario a utilizar. Asegurense de colocar la palabra FUZZ, donde quieren realizar el fuzzing.

![ffuf uso](/assets/img/posts/fuzzing/ffuf/5.png "Ffuf uso")

En este caso para la prueba utilic茅 metasploitable que est谩 corriendo sobre mi red nat, y para el diccionario utilice common.txt, que es uno de los m谩s efectivos. En cuesti贸n 5 segundos ya termin贸 el escaneo, obviamente estamos en un entorno controlado, pero en un entorno real se utilizar铆an otros par谩metros para no provocar DDoS o falsos positivos.

Tambi茅n pueden ir jugando con -t para agregarle hilos aunque no les recomiendo que se pasen de los 300 en retos CTFS (EN UN ENTORNO REAL NO ES RECOMENDABLE PASARSE LOS 10, TODO DEPENDE LA CRITICIDAD DEL HOST) ya que pueden saturar al servidor con peticiones y provocar铆an una denegaci贸n de servicios. Como notaron es s煤per sencillo su uso y lo mejor de todo es que es s煤per r谩pido a comparaci贸n de otros fuzeadores. Me gustar铆a m谩s adelante traerles una comparaci贸n con los diferentes fuzeadores conocidos.

Buen铆simo si llegaste hasta ac谩 espero que te haya gustado esta herramienta y la puedas probar. Despu茅s me cuentas como te ha ido con la herramienta y si ya vas a dejar de utilizar Wffuz y gobuster todav铆a 馃槀.

Comp谩rtelo con tus amigos y sigue aprendiendo de este hermoso mundo del hacking, te veo luego.
 
[R4z0r](https://juankaenel.github.io) 馃懆鈥嶐煉?



