---
title: "7 clases, 10 fachadas y 4 meses de onboarding: La trampa de los Nanoservicios"
date: 2026-10-17T21:42:52-05:00
draft: false
author: ["Santiago Galvis"]
description: "Cuando picar microservicios al extremo choca contra un legado SOA y monolítico. Así se ve la deuda técnica no planeada que nadie admite en los diagramas."
categories: ["Software Architecture"]
tags: ["Deuda Técnica", "Nanoservicios", "Microservicios", "SOA", "Legacy"]
series: ["Cicatrices de Arquitectura"]
ShowToc: false
TocOpen: true
mermaid: true
comments: true

cover:
  image: "cover-image.jpeg"
  alt: "Ilustración de múltiples nanoservicios haciendo cuello de botella hacia un monolito antiguo"
  caption: ""
  relative: true
---
# El espejismo de los Nanoservicios: Por qué agregar un campo nos costó 8 puntos

Hace un tiempo estuve a cargo de una aplicación móvil del sector bancario. Un día común y corriente, en una planeación de sprint, nos pidieron agregar un campo en un endpoint, ya que querían mostrar un poco más de información relacionada con los productos de los clientes. Para nuestra sorpresa, nos dimos cuenta de que para hacer ese cambio tan simple requeríamos modificar clases, fachadas y cruzar varios despliegues, además de realizar una **regresión agresiva** porque varios flujos dependían de las mismas líneas de código.

Lo que en teoría debía tomarnos minutos, terminó con una estimación de **8 puntos**, lo cual es básicamente la mitad de la capacidad de un integrante del equipo. El PO puso el grito en el cielo, obviamente. Aunque tenía todos los argumentos técnicos para justificarlo frente al equipo —el *coupling* distribuido, el estado del *legacy* y las decisiones de arquitectura heredadas—, incluso para mí resultaba ilógico. ¿Cómo era posible que esta fuera nuestra realidad? Y más importante aún: **¿cómo llegamos hasta aquí?**

**La tesis es simple:** La deuda técnica más peligrosa no nace por programar rápido y mal, sino por la sobre-ingeniería no planeada.

### El espejismo de los "Nanoservicios" (Arriba moderno, abajo un museo)

Lo que empezó como una idea para tener dominios claros, cruzó rápidamente la línea hacia "nanoservicios" anémicos hiper-fragmentados. Hacia afuera, todo el mundo vendía el proyecto como si fuera una chimba simplemente porque "estamos en la nube y usamos microservicios". Éramos la última ola tecnológica.

Pero adentro del código, la realidad era oscura. Se suponía que esta capa debía funcionar como un BFF (*Backend For Frontend*): su trabajo era simplificar los llamados de los servicios de integración hacia el *front*, computar campos y hacerle la vida fácil al cliente. En la práctica, terminó siendo un pasamanos de datos glorificado.

### El cuello de botella silencioso: SOA y el Monolito atrás

Pero el verdadero cuello de botella estaba en el fondo: el monolito SOA. Para mí, eso era básicamente una caja negra. No existía el menor interés por documentar o entender qué sistema consumía qué o cómo convivían por detrás. La expectativa genérica siempre era "exponer un servicio que orqueste todo" en un centralizador de APIs para simplificar la integración. 

El problema de modernizar solo la fachada es la asimetría de escalabilidad: la capa externa en la nube escala infinito, pero la infraestructura *legacy* interna es finita y no reacciona a la misma velocidad. Al no tener claro el comportamiento de estos sistemas de fondo, los llamaban de forma directa y síncrona. Nadie pensó en jugar con colas, asincronismo o los trucos que los arquitectos tenemos bajo la manga para mitigar el golpe, proteger el *legacy* y dar una percepción de rendimiento óptimo sin sacrificar la mantenibilidad.

### Cuando la bomba explota en silencio (Los síntomas reales)

Esta deuda técnica no explota un viernes a la medianoche con una alerta roja, sino que es silenciosa y cuando te das cuenta ya es tarde.

**La carga cognitiva por las nubes**
Como la arquitectura era tan compleja e innecesariamente dividida, nadie en el equipo lograba tener en la cabeza el mapa mental completo de cómo funcionaba un flujo de inicio a fin. El *onboarding* de un dev nuevo, que en condiciones normales tomaría un par de semanas, se convertía en meses de frustración.

**El impuesto de las 7 clases y 10 fachadas**
Aquí es donde sangrábamos a diario. El verdadero dolor no era la latencia de red, era tratar de hacer un simple *debug*. Un solo *request* pasaba por varios repositorios distintos y todos los microservicios tenían capas inútiles: fachadas, *delegates*, *mappers*... un montón de *boilerplate* y *coupling* distribuido por el que un dato viajaba 10 capas sin aportar ni un gramo de valor de negocio.

### ¿Cómo se doma al monstruo? La pelea infinita: Negocio vs. Tecnología

Siendo 100% honestos: no hay varita mágica, y Negocio no va a parar la fábrica 6 meses para que reescribas todo de cero. Cuando te sientas con el PO a explicarle por qué una *feature* trivial ahora cuesta 8 puntos, la conversación suele volverse un callejón sin salida.

En nuestro caso, el equipo ya había levantado la mano advirtiendo los síntomas. ¿La respuesta de Negocio? Nos pidieron un mapeo exacto de qué tocaba hacer para arreglar esa deuda. El problema de fondo es que veían la deuda técnica como un bache en la calle: lo tapas una vez y te olvidas. Lo que cuesta evangelizar del otro lado de la mesa es que **los productos tecnológicos en producción están vivos**. Si no reciben mantenimiento continuo, se vuelven obsoletos.

¿Qué pasaba en la realidad? Como la prioridad siempre era sacar nuevas funcionalidades para cumplir las metas del trimestre, nuestras propuestas de *refactoring* se perdían en el fondo del *backlog*. Entonces, para poder cumplir con las fechas y simplemente sobrevivir al *sprint*, terminábamos buscando atajos... es decir, creando más deuda técnica sobre la deuda que ya teníamos. Un círculo vicioso perfecto.

![image](content-one.jpeg)

### El detector de humo (Para no repetir la historia)

Para que un Junior o un Senior detecte a tiempo que su equipo está creando deuda técnica no planeada, les dejo mis 3 "red flags" de bolsillo:

1. **La regla de los repositorios inútiles:** Si agregar un campo en la respuesta de un *endpoint* requiere tocar 1 o 2 repositorios, modificar 2 o 3 clases y realizar pruebas de flujos que no están relacionados con el cambio, tienes un *coupling* distribuido severo y perdiste cohesión.
2. **El onboarding infinito:** Si a un dev nuevo le toma más de 2 semanas entender un flujo transaccional y los servicios no son semanticos, algo va mal, el codigo es como un libro, deberia ser sencillo de entender y seguir su contexto con claridad.
3. **Escalar el front ignorando el legacy:** Si tu capa frontal es 100% nube y elástica, pero por cada *request* le pega de forma síncrona a un monolito SOA o base de datos finita que no escala al mismo ritmo, estás construyendo un castillo de naipes. Toca jugar con la Arquitectura tal vez alguinas colas.

*¿Cuál ha sido ese cambio "simple" que destapó la peor deuda técnica oculta en sus equipos? ¿Cómo lograron negociar ese refactoring con Negocio sin detener la fábrica? Los leo en los comentarios.*  
