# Sistemas distribuidos

Coleccion de entidades independientes que cooperan para resolver un problema

- Para comunicar, se envian y reciben mensajes en vez de compartir memoria

- No existe certeza de si de recibio el mensaje

- No comparten memoria ni reloj

- Hay autonomía y heterogeneidad (debilmente acoplados)

- Se espera que el usuario vea la coleccion de computadores como un solo computador

Una ejecucion distribuidas es la ejecución de procesos a través del sistema sistribuido para conseguir un objectivo en común

<!-- ## Algoritmos distribuidos -->

## Memoria distribuida

Se podrian usar operaciones read/write pero los programas tendrian que implementar busy waiting

### Operaciones de red

- Existen canales compartidos que conectan procesos
- Cada proceso tiene variables locales (no tienen accesos concurrentes, no se requiere mutex)
- Procesos se comunican para interactuar

#### Canales

Los canales son FIFO, confiables y libre de errores. El acceso a un canal es atómico

1. Se crea un canal
2. Se envia algo por el canal
3. Alguien hace recieve y guarda lo que se envió

**channel ch(tp1 id1,...,tpn idn)**

- ch es el nombre del canal
- tp es el tipo de datos
- id nombre del campo transmitido

channel input(char) -> un canal que recibe caracteres

**send ch(expr1,...)**

- expr1 son expresiones cuyos tipos corresponden al canal

Al hacer send el mensaje se añade a una cola (no es bloqueante)

**receive ch(var,...)**

- var son las variables donde se guarda loq ue recibe

recieve saca el primer mensaje de la cola y se lo asigna a al variable (es bloqueante)
