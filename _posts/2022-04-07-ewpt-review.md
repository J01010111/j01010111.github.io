---
layout: post
title:  "eWPT Review –  Mi experiencia y recomendaciones"
summary: "En este post te hablo sobre eWPT, una de las certificaciones más importantes en Pentesting Web. ¿Es difícil, vale la pena obtenerlo?"
date: '2022-04-07 05:30:23 +0530'
category: ['certificaciones']
thumbnail: /assets/img/posts/ewpt/ewpt.jpg
keywords: ewpt, elearnsecurity, certificaciones, review, examen, experiencia, certification, web hacking, hacking web 
permalink: /blog/ewpt-mi-experiencia-y-recomendaciones/
author: r4z0r
---

Buenas crack! Bienvenido a este artículo, hoy te voy a hablar sobre la certificación **[eWPT](https://elearnsecurity.com/product/ewpt-certification/)(eLearnSecurity Web Application Penetration Tester)**, una certificación para poner apruebas tus habilidades de pentesting web.

## ¿Qué es eWPT, de qué trata esta certificación?

eWPT, es una certificación que evalúa las habilidades de un pentester de aplicaciones web. El examen es una prueba basada en habilidades que requiere que los candidatos realicen una simulación de pentesting de aplicaciones web en el mundo real. Así mismo lo define la eLearn Security, una certificación basada en un entorno real que requiere que encuentres el mayor número de vulnerabilidades web y luego generes un reporte profesional de lo encontrado. 

## ¿Qué conocimientos se certificaran y evaluaran al obtener esta certificación?

- Procesos y metodologías de pruebas de penetración
- Análisis e inspección de aplicaciones web
- OSINT y técnicas de recopilación de información
- Evaluación de la vulnerabilidad de las aplicaciones web
- OWASP TOP 10 2013 / Guía de pruebas OWASP
- Explotación manual de XSS, SQLi, servicios web, HTML5, LFI/RFI
- Desarrollo de exploits para entornos web
- Conocimientos avanzados de reporting y remediación

## ¿Qué tengo que saber para tomar el examen?

- Funcionamiento de cookies y sesiones.
- Saber manejar bien Burpsuite/Owasp Zap, todo tiene que pasar por el proxy. No necesitas el burp profesional.
- Saber como explotar vulns como SQLi, XSS, subida de ficheros sin restricciones, SOAP, cookie hijacking y demás.
- SQLMap, una tool indispensable para el examen.
- El reporte es clave, debe parecerse lo más profesional posible, hay muchos templates para seguirlos, no te preocupes por eso. Dejaré los links al final que me sirvieron a mí para crear el mío.

## ¿Es difícil el examen?

Desde mi punto de vista, es más complicado que los laboratorios que nos dan en el curso ya que tienes varios dominios y aplicaciones y hay que ser muy ordenado. Pero no lo considero difícil de pasar, solo basta con tener los conocimientos que mencioné anteriormente, ser ordenado, ir a tu ritmo, y lo pasas de taquito.

## ¿Cómo es el examen?

El examen dura 14 días desde que arrancas, tenés 7 días para hacer tus pruebas y sacar las evidencias y luego de eso se cierra el laboratorio y tienes otros 7 días para hacer tu reporte. **Hay un requisito necesario pero insuficiente para aprobar (es decir, si lo sacás suma mucho, pero no significa que apruebes) que es ingresar al área de administración como usuario administrador.**

## ¿Cómo es el curso?

El curso que eLearn da está bien, toca todos los temas necesarios aunque algunos innecesarios pero está bien para aprender y aportar conocimiento. En mi caso solo leí el primer pdf, y me ví todos los videos, eso si. Tomar apuntes de las clases me sirvió mucho durante el exámen y me dio lo necesario para aprobar. 

## ¿Vale la pena?

Pues si, claramente, 100% recomendado ya que te va a abrir nuevas puertas y nuevos trabajos en pentesting web. El examen es real, desafiante y divertido. Muy recomendado obtenerlo sin dudas.

## Consejos para aprobar

- Ser ordenado, yo usé freemind (una herramienta que la recomiendan en el curso) y hacia mis notas en obsidian.
- Leer bien la carta de compromiso, ver que entra en el scope y que no entra.
- Usar herramientas como Sublist3r, virus total, amass, subfinder, ffuf o cualquiera que conozcas para enumerar subdominios, es clave un buen reconocimiento.
- Para el fuzzing puedes usar ffuf, gobuster o el que te quede cómodo, incluso yo usé zap que tiene un buen spider para encontrar archivos y carpetas dentro de las apps.
- Utilizar técnicas de estudio como pomodoro pueden ayudarte.
- Descansar, tomarte pausas, porque te vas a trabar más de una vez y las pausas ayudan mucho, tenés 7 días cracks.
- Probar en todos lados, user-agent es una buena pista para encontrar vulns.
- Practicar en PortSwigger te ayudará.
- Para el reporte saqué lo mejor de los reportes que he encontrado, links al final.
- Recordar que las versiones sin soporte o antiguas también se reportan, httpOnly, todo lo que consideres como mejora, reportalo, es un pentesting real no un ctf.
- En el reporte me basé en CVSS para el score, al final te dejo la calculadora para que puedas verlo.
- Hay una página que hace spam y cuando querés dirigirte a un subdominio te manda a otro, para lidiar con eso tenés que desactivar tus nameservers para tener internet y dejar solo la ip que ellos te dan, de ese modo solo tienes acceso a los subdominios que están en el scope.

## Links útiles para el exámen

- [Curso eWPT](https://my.ine.com/CyberSecurity/courses/38316560/web-application-penetration-testing)

- [Notas de the-mayor](https://themayor.notion.site/Pentesting-Notes-9c46a29fdead4d1880c70bfafa8d453a)

- [WAPT Report](https://pdfslide.net/documents/sample-wapt-report-v14.html)

- [TCM-Security-Sample-Pentest-Report](https://github.com/hmaverickadams/TCM-Security-Sample-Pentest-Report)

- [Pentest Report](https://github.com/anontuttuvenus/eWPT-Report-Template)

- [Web Pentest Report](https://underdefense.com/wp-content/uploads/2021/09/Anonymized-Web-application-penetration-testing-report.pdf)

- [Calculadora CVSS](https://www.first.org/cvss/calculator/3.0)

- [PortSwigger Academy](https://portswigger.net/web-security)

- [Hacker One - Pentest Reporting and best practices](https://www.youtube.com/watch?v=6QIrXgPGJhM&t=1042s)


Espero te haya gustado el artículo y cuando rindas lo apruebes, si yo lo pude pasar, vos también crack, solo dedícale ganas y esfuerzo y ve a por todo!

Nos vemos en otro artículo, saludos!

[R4z0r](https://juankaenel.github.io)👨‍💻




