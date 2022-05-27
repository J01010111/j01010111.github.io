---                                                                                                                                                                          
layout: post                                                                                                                                                                 
title:  "eJPT Review - Mi experiencia y recomendaciones"                                                                                                                                        
summary: "¿Vale la pena esta certificación, qué tan difícil es, es valorada?"                                                                                                                              
author: r4z0r                                                                                                                                                              
date: '2021-09-22 14:35:23 +0530'          
category: ['certificaciones','youtube']                                                                                                                                                             
thumbnail: /assets/img/posts/ejpt.webp                                                                                                                                        
keywords: ejpt, elearn, elearsecurity, certificación, youtube, certification, review, experiencia, examen
permalink: /blog/ejpt-mi-experiencia-y-recomendaciones/                                                                                                                                  
--- 


Hola! Bienvenido a este nuevo artículo en mi blog, hoy voy a hablar sobre el **[eJPT](https://elearnsecurity.com/product/ejpt-certification/)(eLearnSecurity Junior Penetration Tester)**, una certificación para iniciantes en el mundo del hacking, te voy a dejar mi experiencia y recomendaciones.

**Por si quieres ver en formato de video, te paso el que subí a mi canal de YouTube, de paso te pasas a ver el contenido que creo 😃**

<iframe width="560" height="315" src="https://www.youtube.com/embed/Dm7uwGlYd6w" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


## ¿Qué es EJPT?

Como te mencioné anteriormente **[EJPT](https://elearnsecurity.com/product/ejpt-certification/)** es una certificación para todas aquellas personas que se están adentrando en el mundo del pentesting, actualmente se pusieron muy de moda estas certificaciones impartidas por **[eLearnSecurity](https://elearnsecurity.com/)** y son muy demandadas por las empresas (así que si estás buscando una oportunidad y ya tienes esta certificación vas a sumar muchos puntos).

## Curso gratuito para la certificación

Este proveedor de certificaciones y plataforma educativa en ciberseguridad brinda un **[curso gratis](https://my.ine.com/CyberSecurity/learning-paths/a223968e-3a74-45ed-884d-2d16760b8bbd/penetration-testing-student)** relacionada con esta certificación que está muy bien, suele ser un poco aburrida ya que la mayoría es mediante PDFs y algunas partes la verdad no están bien entendibles, sobre todo la parte del ruteo que es la parte fundamental para aprobar el examen, creo que ahí se quedaron cortos con la explicación, pero que va, es gratis, no hay que quejarse por cosas gratis, hay que sacarle el mejor provecho y recurrir a internet o las comunidades en lo que no nos queda claro que ahí está todo. Personalmente antes de rendir decidí tomar las clases del curso exceptuando la de programación cosa que no iba a necesitar para el examen, lo recomiendo a este curso ya que aprendí cosas nuevas y me sirvieron para finalizar exitosamente con el examen. Otro punto importante del curso son las Black Boxes que te ofrecen al final de forma de práctica, la verdad que están bien para aprender unas cosas y reforzar en otras, incluso te proveen el Writeup de los retos para que te apoyes en caso que no puedas avanzar así que se los recomiendo.

## Costo del examen

Actualmente a la fecha de hoy 20 de septiembre de 2021 el costo del examen es de 200 dólares, y es de los más económicos hoy en día en lo que respecta a certificaciones así que es una excelente inversión.

## ¿Cómo es el examen?

Esto no es como en TryHackMe o HackTheBox que te dan una máquina y tienes que encontrar la flag en una o varias máquinas, no señor, acá se trata de otra cosa. Pero no te preocupes, si ya vienes practicando varios meses en estas plataformas el examen no te va a costar, incluso es más fácil que las black boxes que te dan.

Una vez que adquieras el voucher del examen y le des a iniciar con el examen te van a mandar un correo donde en el mismo va un PDF con la explicación del examen (no las preguntas, solo una breve explicación), y también te mandan una captura en Wireshark (súper importante). Vas a tener que configurar tu conexión VPN donde tendrás que asignarle un usuario y contraseña, una vez hecho eso e iniciado el laboratorio te puedes descargar la VPN y empezar el con el examen (ojo, vi muchas veces que la gente se preguntaba: ¿pero cuál es mi objetivo? ¡no me dieron ninguna ip para atacar!. Pues, ahí entra en juego la lógica y también la experiencia con el curso gratuito, vas a tener que ver en qué red estás mediante VPN y empezar a realizar la enumeración a ese rango. Volviendo a lo importante de este punto, **te dan unas 20 preguntas, de las cuales tienes que responder al menos 15 bien para aprobar, las preguntas son de opción múltiple asi que ahí tienes una ayudita.** Responder a estas preguntas implica que vas a tener que realizar una buena enumeración y visitar diferentes redes y equipos. No es necesario explotar todos los equipos para responder a estas preguntas ya tu vas ir viendo en base a las preguntas cuáles explotar. Otra cosa importante es que no usé pivoting, pensé que estaba más orientado a eso pero no fue necesario solo necesite **armar las rutas**. Esto fue lo que más me costó ya que nunca lo había hecho previamente (si lo hice en una parte del curso de esta certificación) así que dedicale un tiempo a estudiar y entender el ruteo porque es un requisito casi indispensable para aprobar. Volviendo a los requisitos del examen, **tienes 3 días para terminarlo y responder las preguntas**, así que súper tranqui en en ese sentido llegas re bien con el tiempo. Ok, entonces para resumir, **son 3 días, 20 preguntas de las cuales con 15 se aprueba el examen**, eso es lo importante.

## Consejos y Recomendaciones

– La enumeración es clave, esto no es lo mismo que en THM y HTB  como te mencioné anteriormente, aquí vas a  tener que aprender a realizar una buena enumeración para detectar los hosts en el rango de red que sea necesario y luego detectar, puertos, versiones y servicios para ver por donde entrar.

– Practica leer los paquetes de Wireshark, de ver como son las comunicaciones, los protocolos, consultas y respuestas, de esa forma vas a poder identificar hosts y redes que te van a permitir ir armando las rutas.

– Aprender a crear las rutas es otro punto importante, la idea acá es saber cual es el router o puerta de enlace y ya en base a eso vas armando las rutas necesarias (en el curso hay una guía y un laboratorio que te van a ayudar para esto).

– Anotar todo lo que vayas haciendo, esto me ayudó mucho y lo fui practicando con las Black Boxes del curso, la aplicación que utilizo es Joplin, pero acá tiro desde mi windows porque tengo dos pantallas, así me resulta súper cómodo ir documentando todo lo que voy haciendo, pero también tienes a CherryTree para Kali o Parrot entre otros.

– No te puedo decir qué vulnerabilidades tocan dentro del examen porque sería spoiler pero si haces el curso gratuito vas a darte cuenta que son unas pocas y son bastante sencillas de explotar.

– Descansa cada cierto tiempo, esto es fundamental ya que a veces no encuentras por donde ir, descansar unos minutos, horas o día te va a dejar la mente en claro sobre nuevas ideas.

– Tomate con calma el examen, si eres nuevo en esto así como yo, debes tomártelo con calma e ir a un ritmo tranquilo así no te saltas cosas importantes del examen, tienes 3 días así que tranquilo.

– Ve con actitud y confianza, esto aplica para todo lo que hagas en la vida, si no tienes actitud y no confías en ti va a ser difícil que las cosas te salgan, esto aplica a todo en la vida no solo a un examen.

## ¿Vale la pena la certificación?

La respuesta es sí, vale la pena, vas a aprender muchas cosas nuevas rindiendo esta certificación y es algo que están solicitando las empresas que tengan los pentesters así que no dudes en rendirla. La verdad que me encantó rendir esta certificación y haberla aprobado, el laboratorio y los recursos proporcionados por **[eLearnSecurity](https://elearnsecurity.com/)** fueron muy buenos y sin dudas la recomiendo muchísimo.

Espero que este artículo te pueda servir y que puedas aprobar, ve a por todas que si yo pude tu también puedes. Si tienes alguna duda puedes hablarme por mis redes que no tendré problema en echarte una mano.

Nos vemos en otro artículo, saludos!

[R4z0r](https://juankaenel.github.io)👨‍💻
