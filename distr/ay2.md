# Relojes logicos
## Lamport
Se ubican en el middleware ya que aseguran que todos los mensajes se contabilicen al reloj de maneara ordenada, sin importar el proceso

## Multicast totalmente ordenado
La cola se ordena por orden creciente de timestamps para que todos los vean en el mismo orden

El mensaje solo entra a la aplicacion si es que ha sido ack'd por cada uno de los otros procesos

## Reloj Vectorial
Cada valor representa el ultimo timestamp que llego de cada proceso, se actualiza siempre al mayor que se recibe

# Consensos
## Bizantinos

# Coordinacion
## Algoritmo de Lamport
Un nodo inicia el algoritmo y le pregunta a sus vecinos sobre la topologia de la red

Cada vecino le pregunta a sus vecinos y luego se devuelve la informacion al nodo original
