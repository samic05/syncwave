---
title: "7 clases, 10 fachadas y 4 meses de onboarding: La trampa de los Nanoservicios"
date: 2026-10-17T21:42:52-05:00
draft: false
author: ["Santiago Galvis"]
description: "Cuando picar microservicios al extremo choca contra un legado SOA y monolítico. Así se ve la deuda técnica no planeada que nadie admite en los diagramas."
categories: ["Software Architecture"]
tags: ["Deuda Técnica", "Nanoservicios", "Microservicios", "SOA", "Legacy"]
series: ["Cicatrices de Arquitectura"]
ShowToc: true
TocOpen: false
mermaid: false
comments: true

cover:
  image: "cover-image.jpeg"
  alt: "Ilustración de múltiples nanoservicios haciendo cuello de botella hacia un monolito antiguo"
  caption: ""
  relative: true
---

Hace un tiempo estuve a cargo de una aplicación móvil del sector bancario. Un día común y corriente, en una planeación de sprint, nos pidieron agregar un campo en un endpoint, ya que querían mostrar un poco más de información relacionada con los productos de los clientes. Para nuestra sorpresa, nos dimos cuenta de que para hacer ese cambio tan simple requeríamos modificar clases, fachadas y cruzar varios despliegues, además de realizar una **regresión agresiva** porque varios flujos dependían de las mismas líneas de código.

Lo que en teoría debía tomarnos minutos, terminó con una estimación de **8 puntos**, lo cual es básicamente la mitad de la capacidad de un integrante del equipo. El PO puso el grito en el cielo, obviamente. A pesar de que yo tenía los argumentos técnicos para explicar por qué esto era así y debíamos manejarlo de esa manera debido al estado y las decisiones arquitectónicas que existían hasta ese momento, incluso para mí resultaba ilógico. ¿Cómo era posible que esta fuera nuestra realidad? Y más importante aún: **¿cómo llegamos hasta aquí?**

Tesis del post: La deuda técnica más peligrosa no nace por programar rápido y mal, sino por la sobre-ingeniería no planeada.

El espejismo de los "Nanoservicios" (Arriba moderno, abajo un museo)

[TODO - Contexto de la arquitectura "Frankenstein"]:

Contar cómo empezó la fiebre de picar servicios buscando agilidad o modernidad.

De Microservicios a Nanoservicios

[TODO - Pérdida de cohesión]:

Explicar cómo se pasó la línea de microservicios con un dominio claro a "nanoservicios" anémicos donde todo está hiper-fragmentado.

Qué sensación daba esto hacia afuera ("estamos en la última ola tecnológica") vs. lo que pasaba adentro del código.

El cuello de botella silencioso: SOA y el Monolito atrás

[TODO - Asimetría de escalabilidad]:

Contar la realidad de lo que había en el fondo: una arquitectura SOA y monolitos que no escalaban al mismo ritmo (throughput y latency) que la capa "nueva".

Por qué modernizar solo la fachada crea una falsa sensación de seguridad.

Cuando la bomba explota en silencio (Los síntomas reales)

[TODO - La muerte por mil cortes]:

Explicar que esta deuda técnica no explota un viernes a la medianoche con una alerta roja, sino que es silenciosa y cuando te das cuenta ya es tarde.

La carga cognitiva por las nubes (De 2 semanas a 4 meses):
[TODO: Contar cómo el onboarding de un dev nuevo pasó de tomar 2 semanas a 4 meses porque nadie logra tener el mapa mental completo del flujo].

El impuesto de las 7 clases y 10 fachadas:
[TODO: Explicar el dolor del boilerplate y el coupling distribuido. Qué pasa en los pipelines y en el debugging cuando un dato viaja por 10 capas sin aportar valor].

¿Cómo se doma al monstruo? La pelea infinita: Negocio vs. Tecnología

[TODO - La trinchera real]:

Ser 100% honestos: no hay varita mágica ni negocio va a parar 6 meses para que reescribamos todo de cero.

La negociación con Negocio: Cómo explicarles por qué una feature "pequeña" ahora tarda semanas y por qué hay que pagar intereses de esa deuda.

Decisiones técnicas para frenar la sangría: Qué hicieron (o qué hacen hoy) para empezar a consolidar, dejar de abrir servicios nuevos sin sentido y bajar la complejidad.

El detector de humo de SyncWave (Para no repetir la historia)

[TODO - Cierre con criterio de ingeniería]:

Dejar 3 reglas de bolsillo o "red flags" para que un Junior o un Senior detecte a tiempo que su equipo está creando deuda técnica no planeada:

[Red Flag 1 - Ej. sobre el número de repos/capas para un cambio simple]

[Red Flag 2 - Ej. sobre el tiempo de onboarding del equipo]

[Red Flag 3 - Ej. sobre escalar la capa frontal sin mirar el legacy de atrás]
