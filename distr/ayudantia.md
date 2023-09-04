# Ayudantía 2

## RPC

RPC comunica procesos que están en distintos sistemas y llama a procedimientos de forma remota. Quien hace la llamada se queda bloqueado.

- Describir interfaces nos permite saber qué métodos podemos llamar.
- La generacion automática de código se encarga de empaquetar y desempaquetar mensajes.
- El mecanismo de lista de servicios sirve para que el cliente sepa qué servicio está disponible.

## Detección de terminacion

### Spoiler Dijkstra

- Existen aristas regulares y aristas inversas (ver slide 22 en Terminación).
- Todos los nodos parten en 0.
- La computación termina cuando no hay mensajes de ningún tipo (regulares ni receives).
- **Parent es el primero que envía un mensaje a alguien.**
- OutDeficit es la cantidad de mensajes enviados sin confirmar.
- InDeficit es la cantidad de mensajes recibidos sin confirmar.
- El algoritmo termina cuando el nodo de entorno tiene outDeficit = 0.

### Para resolver ejercicios:

- Primero, dibujar el grafo
- Luego, representar la secuencia como una tabla, cuyas columnas son: mensaje, parent, outDeficit, inDeficit global e inDeficit de cada uno de los canales que apuntan al nodo de referencia.
- Finalmente, ir rellenando la tabla. InDeficit global debe equivaler a la suma de InDeficit de todos los canales que apunten al nodo.

## Arquitecturas

La arquitectura Observador permite que, al ocurrir un determinado evento, el publicador notifique a los suscriptores si estos están suscritos en ese momento (acoplamiento temporal). Además, dada la forma en la que se describen los eventos, el publicador no tiene referencia a quién está suscrito.

- Acoplamiento temporal: Necesidad de ambas partes de estar activas en el momento de enviar/recibir mensajes.
- Acoplamiento referencial: Acceso al objeto. Conocer quién está al otro lado.

## Sección crítica

Es necesario cumplir con

- Exclusión mutua
- Ausencia de Deadlock
- Ausencia de postergación innecesaria
- Progreso/Entrada

El algoritmo de la panadería requiere de memoria compartida ya que, de no ser el caso, dos o más procesos podrían obtener el mismo número o un proceso puede sacar un número y el contador se actualiza antes de entrar a la sección crítica. Estos problemas hacen que no se garantice la ausencia de Deadlock ni progreso. El algoritmo que resuelve este problema es el algoritmo de Ricart-Agrawala.
