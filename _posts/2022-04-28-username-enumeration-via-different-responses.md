---
layout: post
title:  "Enumeración de usuarios a través de diferentes respuestas"
summary: "Una vuln muy común de ver en pentesting es la enumeración de usuarios a través de diferentes respuestas de un servidor, en este artículo estaremos viendo de qué trata esta vuln y cómo explotarla."
date: '2022-04-28 11:30:23 +0530'
category: ['vulns','portswigger']
thumbnail: /assets/img/posts/username-enumeration/1/fondo.png
keywords: portswigger, username enumeration, enumeración de usuarios, web hacking, hacking web 
permalink: /blog/enumeracion-de-usuarios-a-traves-de-diferentes-respuestas/
author: r4z0r
---

Buenas, bienvenido a otro artículo, hoy toca hablar sobre algo muy importante que veo repetidamente cuando me toca hacer pentesting, la enumeración de usuarios a través de las respuestas de un servidor. 


## ¿De qué se trata esta vulnerabilidad?

Alguna vez intentaste loguearte a una aplicación y viste un mensaje, "este usuario no es válido", pues bien si el servidor te arrojó esa respuesta estas ante una posible enumeración de usuarios, ¿por qué?, pues porque la aplicación te está diciendo flaco este usuario no existe, probá con otro, y ahí es donde un atacante se aprovecha, arma un diccionario de posibles usuarios de la aplicación y empieza a probar lanzando por ejemplo un ataque de fuerza bruta contra el panel de login, a medida que va analizando las diferentes respuestas va a ir identificando nombres de usuarios válidos o inválidos, de eso prácticamente trata la vulnerabilidad de ver las diferentes respuestas del servidor y a partir de ahí sacar información válida, como por ejemplo nombres de usuario, contraseñas, dni, etc.

No solo a través de mensajes podemos aprovecharnos de esta vuln, también podemos ir viendo el tamaño de la respuesta, el código HTTP, el tiempo y otros factores que conlleva una respuesta, por ejemplo, si la app no devuelve un mensaje, pero vemos que con el username pepito nos arroja un código HTTP 200, pero con eminem nos responde con 302, una redirección, ahí algo sucede porque es diferente la respuesta y podríamos estar frente a un usuario válido.

Más adelante estaremos viendo formas de explotar más complicadas, pero vamos por parte dijo Jack el Destripador.

## Laboratorio de práctica

Bien, mucho bla bla y poco hacking, pasemos a explotar un laboratorio de [PortSwigger](https://portswigger.net/web-security), por si alguien no sabía esta página es de las mejores para practicar Hacking Web, sin dudas se aprende un montón, no solo hay laboratorios sino también teoría y cheatsheets útiles. Una cosa importante es que para resolver estos laboratorios tienen que estar registrados.

[Link del laboratorio](https://portswigger.net/web-security/authentication/password-based/lab-username-enumeration-via-different-responses)

1. Una vez que ingresen a este sitio, van a ver que tenemos dos recursos a usar para el laboratorio, un conjunto de contraseñas y usuarios, eso lo tienen que usar en este laboratorio asi que ténganlo a mano, luego le dan a acceder al lab y luego se dirigen a **My account**

    ![1](/assets/img/posts/username-enumeration/1/1.png)

2. Ingresamos al laboratorio y nos dirigimos hacia el panel de login, una vez aquí prueban por ejemplo, con el username "test" y password "test".

    ![2](/assets/img/posts/username-enumeration/1/2.png)
    
    Podemos ver que la respuesta es "Invalid username"

3. Abrimos burpsuite y seteamos la opción "intercept is on", en la pestañita proxy-intercept. Una vez seteado esta configuración se van al login y prueban lo mismo. Ahora burpsuite se pone en el medio y permite modificar la petición hacia el servidor.

    ![3](/assets/img/posts/username-enumeration/1/3.png)

4. Ahora toca llevar esa solicitud hacia el intruder, para eso le dan control+i sobre la solicitud, o click derecho y "send to intruder". Una vez en el intruder primero tienen que limpiar todo, seleccionando "clear", y luego le dan a "add" seleccionando el valor del username. Ahí es donde queremos que vaya iterando burpsuite ya que estaremos lanzando un ataque de fuerza bruta con el intruder.

    ![4](/assets/img/posts/username-enumeration/1/4.png)

5. Ok una vez seteado el lugar donde queremos meter nuestros payloads es hora de cargar nuestro diccionario, para eso copien todos los nombres de usuario que el laboratorio nos provee y le dan en "Paste", por cierto esto está en la pestañita de Payloads. 

    ![5](/assets/img/posts/username-enumeration/1/5.png)

6. Como última configuración tenemos que añadir una columna para ver como responde el servidor a las solicitudes, por ejemplo acá podemos meter todos los mensajes de "Invalid username" en una columna e ir viendo qué usuarios arrojan invalid. Para configurar esto se dirigen a la pestaña Options y abajo encontraran "Grep-Extract", seleccionan "Add" y luego le dan a "Refetch response" para obtener una respuesta a la petición que interceptamos, ahí bajamos y buscamos el mensaje que necesitamos y seleccionamos todo el mensaje, luego le damos ok y listo, ya tenemos seteada esa nueva columna.

    ![6](/assets/img/posts/username-enumeration/1/6.png)

    ![7](/assets/img/posts/username-enumeration/1/7.png)

7. Ahora le damos a "Start attack"

    ![8](/assets/img/posts/username-enumeration/1/8.png)

8. Una vez que comience el ataque van a observar como el intruder lanza peticiones iterando sobre el username. 

    ![9](/assets/img/posts/username-enumeration/1/9.png)

    Observar como todos los mensajes contienen "Invalid username", esto lo vemos gracias al Grep-Extract que agregamos, pero acá lo que importa es que hay uno que arroja "Incorrect password", ahí vemos una respuesta diferente, además que el tamaño también es diferente a las demás respuestas, por lo cuál concluimos que es un username válido.

9. Ya tenemos un username válido, ahora toca encontrar su contraseña, vamos al intruder nuevamente pero en esta ocasión seteamos el valor de campo password para que nuestros payloads se inyecten ahí y obviamente dejamos el username válido en el campo username.

    ![10](/assets/img/posts/username-enumeration/1/10.png)

10. Ahora agregamos nuestras contraseñas que nos provee el laboratorio, las copiamos y pegamos, pero antes asegurense de quitar los usernames que ya no los necesitamos. Lo demás dejamos como está y lanzamos el ataque.

    ![11](/assets/img/posts/username-enumeration/1/11.png)

11. Una vez lanzado el ataque acá la respuesta ya no es "Invalid username" sino "Incorrect password", porque con el usuario que estamos probando es válido, pero la aplicación lanza que no es correcta la contraseña para ese usuario. Nuevamente tenemos una respuesta diferente, en este caso el código de estado HTTP es 302, una redirección con lo cual probando ese username y password ya resolveríamos el laboratorio.

    ![12](/assets/img/posts/username-enumeration/1/12.png)
    
    ![13](/assets/img/posts/username-enumeration/1/13.png)


Bien, como conclusión lo importante acá es investigar sobre las respuestas del servidor, ver como va respondiendo a medida que probamos cosas diferentes, cambiando el username por ejemplo, luego viendo eso van a tener que crear un diccionario útil para esta ocasión, por ejemplo, si están auditando a una aplicación de Despegar.com, por darles un ejemplo, una buena idea sería hacer OSINT y buscar personas que trabajen en Despegar por ejemplo en LinkedIn, una vez que tengan los nombres y apellidos de unos cuantos empleados de Despegar ahora arman el diccionario por ejemplo con la primer letra de su nombre en mayúsculas seguido de su apellido en minúscula, por ejemplo, Pepito García, Pgarcía, es solo un ejemplo ahí tienen que ir metiendo varias combinaciones por ejemplo dejar todo minúscula, reemplazar vocales por números, etc. No va al caso del artículo lo de los diccionarios pero como bonus extra sirve jaja. Lo mismo aplica para la contraseña, deben crear su propio diccionario. Lo importante acá son las respuestas, lo demás pueden ir armándolo a medida que vayan auditando o buscando diccionarios conocidos ya creados en caso que no quieran crear los mismos cosa no recomendada.

  
Y bien, con eso damos por concluido el laboratorio de práctica para esta vulnerabilidad, PERO, este era muy fácil, proximamente estaremos explotando otros más difíciles.


Espero te haya gustado el artículo, si te sirvió te agradecería compartirlo, nos vemos luego!

[R4z0r](https://juankaenel.github.io)👨‍💻




