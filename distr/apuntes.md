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

#### Eficiencia
Un algoritmo con muchos nodos puede ser ineficiente

Para entrar a su sección critica un nodo debe
1. Enviar n-1 mensajes
2. Recibir n-1 mensajes
Incluso si nadie quiere entrar


#### Algoritmos basados en tokens
Solo se puede entrar a la sección critica si es que se tiene el token

Un nodo con token puede entrar a la sección critica una cantidad arbitraria de veces

#### Token circulando en un anillo
El token va circulando en un anillo, se utiliza y se pasa al siguiente en el anillo
- Helper
1. Recibo el token
2. Si User lo quiere se lo paso y espero a que termine
3. Envio el token al siguiente

- User
1. Se pide el token para entrar al helper
2. Espero hasta que reciba el token
3. Corro sección critica
4. Salgo y entrego el token al siguiente

#### Algoritmo Ricart_agrawala con Tokens
Cada proceso tiene 2 arreglos
- Granted: El numero que tenia cada turno la ultima vez que se metio a la sección critica
 - Este array va inludio en el mensaje que tiene al token, y la significativa es la ultima enviada
- Request: Numeros que acompañaban al ultimo mensaje que rqst de cada nodo

- Main
1. Si no tengo el Token
    1. Defino mi turno como el ultimo + 1
    2. Le aviso a todos los nodos que quiero entrar a la sección critica
    3. Espero a recibir el token
    4. Cuando reciba el token entro a la sección critica
    5. Pongo mi numero de turno en granted
    6. Envio Token con granted

- Receiver
1. Espero que me pidan permiso para entrar
2. Actualizo arreglo request
3. Si tengo el token y no estoy en la sección critica, envio el token

- SendToken
1. Si existe un p tal que request[p] > granted[p] elijo uno y envio token


# RPC (Remote Procedure Core)
## Arquitectura de capas
### Estratificada pura
Cada capa solo llama a la de abajo
### Estratificada mezclada
Capa puede llamar a cualquier capa de abajo
### Estratificada con upcalls
Una capa de abajo puede llamar a una capa inmediatamente superior

### Estratificación lógica de aplicaciones
Se diferencia entre tres niveles logicos estratificados para permitir accesos a bases de datos
- Nivel de interfaz (interración con el usuario)
- Nivel de procesamiento (funcionalidad central de aplicacion)
- Nivel de datos (sistema de archivo persistente)

## Mensajes asincronos
Son apropiados si la informacion fluye por los canales en una dirección pero no para procesos de tipo cliente y servidor

Cada cliente necesitaria un canal de respuesta diferente, por lo que abrian muchos canales

Para interacciones cliente-servidor es mejor usar RPC

## Mecanismo RPC
Se hace una llamada remota a un servidor para que ejecute un procedimiento de la misma manera en que se podría hacer una llamada local para ejecutar un procedimiento

Despues de la llamada remota se puede esperar a que el sevidor devuelva el resultado del procedimiento

## Modelo para RPC
### Programa compuesto por módulos
Un módulo contiene procesos y procedimientos
- Lo procesos pueden compatrir variables y llamar a los procedimientos
- Un proceso en un modulo puede comunicarse con un proceso en otro modulo solo llamando a los procedimientos exportados por el segundo módulo

### Implementación de una llamada intermódulos
1. Un nuevo proceso (en un servidor) atiende la llamada
2. Los parámetros son pasados como mensajes entre el proceso que llama y el nuevo proceso en el servidor
3. Se suspende el proceso que hizo la llamada mientras el servidor ejecuta su procedimiento
4. Cuando un servidor termina la ejecución, envía los parametros del restulado y valor de retorno
5. Se reciben los resultados y el proceso que llamó continua

## Proposito RPC
Todas las llamadas a procedimientos deberian ser en principio igual que hacer una llamda local
- El paso de mensajes no tiene que ser visible para el programador
- No se sabe si se esta ejecutnado en otro computador

## Desafios RPC
- La memoria no es compartida entre cliente y servidor
- Hay que pasar parámetros y resultados entre computadores
- Cualquiera de los computadores puede fallar, no hay forma de saber si la operación se concretó

## Operación transparente en RPC
1. Cliente llamba un procedimiento
2. Stub (encargado de comunicar con servidor) construye un mensaje
3. Mensaje se envía a través de la red
4. Sistema operativo del servidor entrega el mensaje al Stub del servidor
5. Stub desempaqueta el mensaje
6. Stub del sevidor hace una llamada local al procedimiento

