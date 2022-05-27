---                                                                                                                                             
layout: post
title: "¿Qué es el fuzzing y cómo se usa en el pentesting?" 
summary: "Fuzzing es uno de los temas más importante en el pentesting, en este artículo te explico qué es y como se aplica en el mundo del hacking."
author: r4z0r                                                                                                                                   
date: '2021-06-08 19:15:23 +0530'                                                                                                               
category: ['fuzzing','HackingTools']                                                                                  
thumbnail: /assets/img/posts/fuzzing/que-es-el-fuzzing/fondo-fuzzing.jpg
keywords: fuzzing, pentesting, hacking, hacking tools, gobuster                                               
permalink: /blog/que-es-el-fuzzing/                                                                                         
---

¿Alguna vez te preguntaste qué es el Fuzzing y cómo se utiliza en el Pentesting? En éste artículo voy a responder a tus dudas, ponte cómodo y empecemos.

## ¿Qué es el Fuzzing?

El fuzzing es un conjunto de técnicas de Testing de Software de caja negra (no se conoce a la víctima) que consiste en encontrar fallas o bugs dentro del sistema a auditar. A menudo estas técnicas son automatizadas o semiautomatizadas, lo que implica proporcionar datos inválidos, inesperados o aleatorios a las entradas de un sistema. En el pentesting el fuzzing no está específicamente orientado a encontrar defectos o bugs  y reportarlas a los dueños del sistema sino que está más orientado a encontrar una brecha de entrada al sistema, como por ejemplo encontrar posibles archivos o directorios que nos resulten interesantes y se nos provea información importante para acceder al sistema.

Entonces pasando en claro, el fuzzing tiene muchísimas utilidades dentro del área del software, pero nosotros vamos a utilizar el fuzzing para encontrar directorios o archivos que no vemos a simple vista o que estén ocultos en nuestra víctima.

Como no me gusta dar teoría aburrida, vamos a pasar a un ejemplo práctico. Para este ejemplo voy a mostrarte una de mis herramientas favoritas, Gobuster. En artículos futuros te voy a traer unas cuantas herramientas de Fuzzing que te van a enamorar de un solo uso.

## Fuzing con Gobuster

![fuzz](/assets/img/posts/fuzzing/que-es-el-fuzzing/fuzzing.png "Gobuster") 

Este es un claro ejemplo de fuzzing de archivos/directorios. Nuestro objetivo aquí es [Metasploitable](https://sourceforge.net/projects/metasploitable/), una máquina virtual que presenta varias aplicaciones webs para practicar pentesting. Aquí lo importante es que entiendas como funciona la herramienta de fuzzing. Lo que realiza acá la herramienta Gobuster es tomar un diccionario, en este caso el **common.txt** uno de los que más utilizo, también recomiendo usar el (directory-list-2-3-medium.txt) el tipo de diccionario a utilizar va a depender del reto o la auditoría en cuestión, continuando, en base a ese diccionario que le pasamos con el parámetro **-u** va a realizar fuerza bruta al objetivo. Gobuster va a ir realizando peticiones GET y cada una de esas peticiones tendrán respuestas, que se ve en la parte de abajo. 
Observar que cada respuesta tiene un código de estado (No se muestran todas las respuestas, sino se nos llenaría la pantalla con las respuestas, acá lo que hace es filtrar solo algunas respuestas). Observar que cada respuesta que se ve en la imagen tiene un código de estado de respuesta HTTP. 
Les invito a que revisen el [link](https://developer.mozilla.org/es/docs/Web/HTTP/Status "Códigos de estados HTTP") para que entiendan como funcionan por detrás las peticiones y respuestas en páginas web.

Un claro ejemplo es el código de estado de respuesta 200, este nos indica una respuesta satisfactoria, el archivo/directorio se encuentra presente, es decir coincide con la palabra del diccionario que esté comparando en ese momento el fuzzeador. Si no se encuentra presente podríamos ver un código de estado de respuesta 404, de página no encontrada, éste no se ve en la imagen ya que lo filtra por defecto.

Y eso chicos es el fuzzing de directorios/archivos, realizar fuerza bruta con un fuzzeador, en este caso Gobuster. Existen muchísimas herramientas de Fuzzing que voy a traer en otros artículos ya que hablar de cada uno sería algo tedioso y no es la idea de este artículo.

Sin más, espero que te haya gustado este artículo y que te haya sido de gran utilidad. Compártelo con tus amigos y sigue aprendiendo del hermoso mundo del hacking, te veo luego.

[R4z0r](https://juankaenel.github.io) 👨‍💻



