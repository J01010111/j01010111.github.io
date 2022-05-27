---
layout: post
title:  "Comandos en linux - GREP - Parte 1"
summary: "Grep es uno de los comandos más usados en linux, en este artículo aprenderemos las bases de este comando"
date: '2021-09-07 10:35:23 +0530'
category: linux
thumbnail: /assets/img/posts/comandos-en-linux-grep/grep.jpg
keywords: hackthebox, optimum, writeup, walkthrought
permalink: /blog/comandos-en-linux-grep-parte1/
author: r4z0r
---

Bienvenido a esta nueva serie de artículos en mi blog: comandos en Linux. Hoy toca hablar del comando grep, uno de los más utilizados en el mundo GNU/Linux sin dudas.

## Qué significa grep y qué podemos hacer con él?

Grep en inglés significa Globally Search For Regular Expression and Print out (Búsqueda global de expresiones regulares). Esta es una herramienta muy usada en sistemas GNU/Linux que consiste en buscar un patrón específico en un fichero (archivo) o un grupo de ficheros. En simples palabras, podemos encontrar cualquier texto dentro de uno o más ficheros.
Cómo usar grep

Sin pasarle argumentos grep puede ser utilizado para buscar un patrón dentro de uno o más ficheros. La sintaxis es la siguiente:

```bash
grep '<texto-a-buscar>' <fichero/ficheros>
```

Un dato a tener en cuenta es que las comillas dobles o simples se utilizan cuando tenemos más de una palabra.

El resultado que se obtiene al ejecutar el comando mencionado previamente, dependerá de las ocurrencias encontradas (en la línea que se encuentra) en dichos archivos.

Por ejemplo supongamos que tenemos un texto.txt que contiene lo siguiente:

```bash
Hola crack, esto es un ejemplo.
Bienvenido al himalaya, si no viste la película de Monster Inc y no lo pronunciaste como el monstruo de las nieves no tuviste infancia.
Voy a escribir himalaya nuevamente para filtrar esta palabra.
Saludos a todos desde el himalaya, solo la primer linea no lleva esta palabra, ahora vas a soñar con ir al himalaya.
```

Si ejecutamos lo siguiente vamos a obtener estos resultados.

```bash
# Comando a ejecutar
grep himalaya texto.txt
# Resultado
Bienvenido al himalaya, si no viste la película de Monster Inc y no lo pronunciaste como el monstruo de las nieves no tuviste infancia.
Voy a escribir himalaya nuevamente para filtrar esta palabra.
Saludos a todos desde el himalaya, solo la primer linea no lleva esta palabra, ahora vas a soñar con ir al himalaya.
Si ejecutamos lo siguiente vamos a obtener estos resultados.
```
¿Qué pasaría si cambiamos la palabra himalaya de la segunda fila y tercer fila por Himalaya?, es decir, le agregamos una mayúscula al comienzo.

```bash
# Comando a ejecutar
grep himalaya texto.txt
# resultado
Saludos a todos desde el himalaya, solo la primer linea no lleva esta palabra, ahora vas a soñar con ir al himalaya.
```

Como ven grep solo trae la coincidencia exacta del patrón a menos que le especifiquemos que ignore tanto mayúsculas y minúsculas, eso lo podemos hacer con la flag -i.

---

Ahora los ejemplos desde Kali, por si te quedó alguna duda.

![texto](/assets/img/posts/comandos-en-linux-grep/1.png)

Ahora agregamos mayúscula al comienzo de la palabra himalaya de la segunda y tercer fila.

![texto](/assets/img/posts/comandos-en-linux-grep/2.png)

Como ven solo nos trae la fila 4 porque esta coincide con nuestro patrón de búsqueda, pero si agregamos -i ahora le decimos que ignore mayúsculas y minúsculas (ignore case).

![texto](/assets/img/posts/comandos-en-linux-grep/3.png)

Bien ya vimos lo básico de grep, ahora pasemos a algo más pro.

## Argumentos

Esta herramienta presenta diversas opciones que pueden ser agregadas a la hora de su ejecución, también conocidas como flags o argumentos. Puedes listar los diferentes argumentos ejecutando la herramienta y pasándole –help pero también puede solicitar el manual con man grep. Esto aplica para todos los comandos de Linux, siempre solicitar la ayuda o el manual nos sirven para entender cómo funciona la herramienta.

Los argumentos más comunes de grep son los siguientes:

- **i (ignore case):** La búsqueda no va a distinguir entre mayúsculas y minúsculas. Es decir, si quieres buscar la palabra silla será lo mismo que SILLA.
- **c (count):** Solo imprimirá en pantalla el número de líneas que coinciden con el patrón buscado.
- **r (recursive):** Realiza la búsqueda recursiva en el directorio actual.
- **n (line number):** Imprime el número de línea seguida de la línea donde encontró el patrón buscado.
- **v (invert match):** Se nos muestran las líneas que no coinciden con el patrón que hemos buscado.

Hasta acá hemos visto los argumentos comunes (en un próximo artículo entraremos a jugar con expresiones regulares que esto vuelve a grep un arma muy poderosa).

 
Compártelo con tus amigos y sigue aprendiendo de este hermoso mundo del hacking, te veo luego.

[R4z0r](https://juankaenel.github.io)👨‍💻
