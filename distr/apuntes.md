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

### Canales

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

### Problema de la sección critica

El problema ocurre cuando varios procesos tienen acceso a recursos compartidos

Existen protocolos de entrada y de salida que rodean a la sección critica y que evitan el acceso simultaneo al recurso

Una solucion debe cumplir con

- Exclusion mutua
- No hay deadlock
- No hay postergación innecesaria
- Un proceso que intenta entrar finalmente lo logra (progreso)

#### Algoritmo sacar numero

Cada proceso que quiere entrar a su sección critico saca un numero y espera su turno para correr la sección critica

Esto requiere memoria compartida porque todos los procesos necesitan saber cual es el siguiente numero y cual es el numero actual

#### Algoritmo panaderia

1. Asigno mi turno en 1
2. Consulto el turno de todos los demas
3. Fijo mi turno como el maximo de todos los turnos + 1
4. Mientras mi numero sea mayor a cualquier otro espero
5. Si tengo el mismo numero que otro entonces el de menor id pasa primero

#### Algoritmo Ricart-Agrawla

Cada nodo tiene dos procesos

Principal:

1. Escojo un numero que va creciendo
2. Le pido permiso a todos los demas para entrar a la sección critica a través de un mensaje con mi numero e ID
3. Espero a que todos me den permiso para entrar
4. Al salir de la sección critica aviso a todos los procesos de la cola del proceso recieve salí de la cola

Recieve:

1. Recibo solicitues de un cierto nodo y turno
2. Si el turno es menor al mio, entonces apruebo
3. Sino lo inserto en un cola

Los principales problemas

- la seleccion de numero. Debo elegir siempre un numero mayor al que he visto

- Si dos procesos tienen el mismo numero, desempato con el numero de proceso

- Estoy esperando a todos los procesos pero pueden haber procesos que no quieren entrar, si no quiero entrar deberia siempre aceptar a todos
