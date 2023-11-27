# Sistemas distribuidos
Colección de entidades independientes que cooperan para resolver un problema

- Para comunicar, se envían y reciben mensajes en vez de compartir memoria
- No existe certeza de si de recibió el mensaje
- No comparten memoria ni reloj
- Hay autonomía y heterogeneidad (débilmente acoplados)
- Se espera que el usuario vea la colección de computadores como un solo computador

Una ejecución distribuida es la ejecución de procesos a través del sistema distribuido para conseguir un objetivo en común

## Red de comunicaciones
Se podrian usar operaciones read/write (como en variables compartidas) pero los programas tendrian que implementar busy waiting

### Operaciones de red
- Existen canales compartidos que conectan procesos
- Cada proceso tiene variables locales (no tienen accesos concurrentes, no se requiere mutex)
- Procesos se comunican para interactuar

### Canales

Los canales permiten comunicar procesos dentro de un sistema distribuido

Los canales son colas FIFO, confiables y libres de error. El acceso a un canal es atómico

1. Se crea un canal
2. Se envía algo por el canal
3. Alguien hace `receive` y guarda lo que se envió

**channel ch(tp1 id1,...,tpn idn)**

- ch es el nombre del canal
- tp es el tipo de datos
- id nombre del campo transmitido

`channel input(char)` -> un canal que recibe caracteres

**send ch(expr1,...)**

- `expr1` son expresiones cuyos tipos corresponden al canal

Al hacer `send` el mensaje se añade a una cola (no es bloqueante)

**receive ch(var,...)**

- `var` son las variables donde se guarda lo que recibe

`receive` saca el primer mensaje de la cola y se lo asigna a la variable (es bloqueante)

### Problema de la sección critica

El problema ocurre cuando varios procesos tienen acceso a recursos compartidos

Existen protocolos de entrada y de salida que rodean a la sección crítica y que evitan el acceso simultáneo al recurso

Una solución debe cumplir con

- Exclusión mutua
- No hay *deadlock*
- No hay postergación innecesaria
- Un proceso que intenta entrar finalmente lo logra (progreso)

#### Algoritmo sacar número (memoria compartida)

Cada proceso que quiere entrar a su sección crítica saca un número y espera su turno para correr la sección crítica

Variables compartidas:

- `number <- 1`. Dispensador de números
- `next <- 1`. Proceso que está actualmente en la sección crítica
- `turn[n] <- {0 ... 0}`. Número que sacó cada uno de los n procesos

`FA(number, 1)` suma 1 a la variable compartida `number`

Esto requiere memoria compartida porque todos los procesos necesitan saber cual es el siguiente numero y cual es el numero actual

#### Algoritmo panadería (memoria compartida)

1. Asigno mi turno en 1
2. Consulto el turno de todos los demas
3. Fijo mi turno como el máximo de todos los turnos + 1
4. Mientras mi número sea mayor a cualquier otro espero
5. Si tengo el mismo número que otro entonces el de menor id pasa primero

#### Algoritmo Ricart-Agrawala

Cada nodo tiene dos procesos

Principal:

1. Escojo un número que va creciendo
2. Le pido permiso a todos los demás para entrar a la sección crítica a través de un mensaje con mi número e ID
3. Espero a que todos me den permiso para entrar
4. Al salir de la sección crítica aviso a todos los procesos de la cola del proceso `receive` salí de la cola

Receive:

1. Recibo solicitues de un cierto nodo y turno
2. Si no quiero entrar a la sección crítica, acepto
3. Si el turno es menor al mío, entonces apruebo
4. Si no, lo inserto en la cola `deferred`

Los principales problemas

- La seleccion de número. Debo elegir siempre un número mayor al que he visto. Para solucionarlo, se introduce una variable local `highest` para cada nodo que tiene como valor el `rqst#` más grande que ha recibido

- Dos procesos podrían elegir el mismo número. Para solucionar esto, desempato con el número de proceso

- Estoy esperando a todos los procesos pero pueden haber procesos que no quieren entrar, si no quiero entrar debería siempre aceptar a todos

#### Eficiencia
Un algoritmo con muchos nodos puede ser ineficiente

Para entrar a su sección crítica un nodo debe
1. Enviar n-1 mensajes
2. Recibir n-1 mensajes
Incluso si nadie quiere entrar

#### Algoritmos basados en tokens
Solo se puede entrar a la sección critica si es que se tiene el token

Un nodo con token puede entrar a la sección crítica una cantidad arbitraria de veces

#### Token circulando en un anillo
El token va circulando en un anillo, se utiliza y se pasa al siguiente en el anillo

- Helper
1. Recibo el token
2. Si User lo quiere se lo paso y espero a que termine
3. Envio el token al siguiente

- User
1. Se pide el token para entrar al Helper
2. Espero hasta que reciba el token
3. Corro sección crítica
4. Salgo y entrego el token al siguiente

#### Algoritmo Ricart-Agrawala con Tokens
Cada proceso tiene 2 arreglos
- `granted`: El número que tenia cada turno la última vez que se metió a la sección crítica
 - Este array va incluido en el mensaje que tiene al token, y la única copia significativa es la que va con el token
- `request`: El número que acompañaba al ultimo mensaje `rqst` de cada nodo

- Main
1. Si no tengo el Token
    1. Defino mi turno como el último + 1
    2. Le aviso a todos los nodos que quiero entrar a la sección crítica
    3. Espero a recibir el token
2. Cuando reciba el token entro a la sección crítica
3. Pongo mi numero de turno en granted
4. Envio Token con granted

- Receiver
1. Espero que me pidan permiso para entrar
2. Actualizo arreglo request
3. Si tengo el token y no estoy en la sección critica, envio el token

- SendToken
1. Si existe un p tal que `request[p]` > `granted[p]` elijo uno y envio token

# Arquitecturas


# RPC (Remote Procedure Call)
Es un mecanismo de invocación remota que sirve para llamar procedimientos de forma remota.

La llamada es bloqueante y se espera a recibir respuesta



## Mensajes asincronos
Son apropiados si la informacion fluye por los canales en una dirección pero no para procesos de tipo cliente y servidor

Cada cliente necesitaria un canal de respuesta diferente, por lo que abrian muchos canales

Para interacciones cliente-servidor es mejor usar RPC

## Mecanismo RPC
Un programa está compuesto por módulos que contienen procesos y procedimientos

Un proceso en un módulo se comunica con otro proceso en otro módulo llamando procedimientos exportados

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

### Flujo
1. Cliente llamba un procedimiento
2. Stub (encargado de comunicar con servidor) construye un mensaje
3. Mensaje se envía a través de la red
4. Sistema operativo del servidor entrega el mensaje al Stub del servidor
5. Stub desempaqueta el mensaje
6. Stub del sevidor hace una llamada local al procedimiento

### Marshaling (Paso de parametros)
Corresponde al empaque de los mensajes hecho por el stub para ser enviados al servidor
El servidor solo ve una serie de bytes

### Punteros
Los punteros solo tienen sentido dentro de un espacio de memoria determinado

Una de las soluciones es copiar la estructura completa al stack para luego copiarla de vuelta y sobreescribir el valor original

Esto es llamado copia-por-valor/restauracion

# Arquitecturas
## Arquitectura de software
Es cómo deben ser organizados componentes del sistema y cómo deben interactuar
- Componentes: Unidad modular con interfaces
- Conectores: Mecanismos que median comunicación

## Arquitectura de capas
### Estratificada pura
Cada capa solo llama a la capa **inmediatamente** inferior
### Estratificada mezclada
Cada capa puede llamar a cualquier capa de abajo
### Estratificada con upcalls
Una capa de abajo puede llamar a una capa inmediatamente superior

### Estratificación lógica de aplicaciones
Se diferencia entre tres niveles logicos estratificados para permitir accesos a bases de datos
- Nivel de interfaz (interración con el usuario)
- Nivel de procesamiento (funcionalidad central de aplicacion)
- Nivel de datos (sistema de archivo persistente)

### Arquitectura de dos capas físicas
Existe un cliente que contiene solo los programas que se utilizan a nivel de inferaz de usuario y un servidor que se encarga de todo el resto

### Arquitectura de tres capas fisicas
- Cliente
- Middleware
- Servidor de datos

El Middleware podría actuar como servidor o como cliente dependiendo de con quién esté interactuando

## Separación entre procesamiento y coordinación
Existen
- Acoplamiento referencial: Un proceso solo puede comunicarse cuando conoce el identificador de otro
- Acoplamiento temporal: Ambos deben estar corriendo al mismo tiempo

### Arquitectura publicación-suscripción
El servidor tiene una referencia del cliente y cada vez que ocurre algo el servidor envía la información al cliente
- Basado en tópicos (atributo, valor)
- Basado en contenidos (atributo, rango)

#### Patrón observador
- Un Sujeto publica información
- Los objetos que se suscriben implementan interfaz Observador
- Cada Observador tiene un metodo actualizar
- Sujeto permite registrar observadores y llama a su metodo actualizar cuando los datos se actualizan
- La clase que representa los Datos hereda de sujeto

# Detección de terminación
## Casos simples
Cuando un programa esta corriendo en un único procesador, se sabe que ha terminado si:
- Cada proceso esta bloqueado o terminado
- No hay operaciones I/O

## Casos distribuidos
En un programa distribuido:
- El estado global no es visible para ningún procesador
- Incluso si todos los procesadores estan desocupados, puede haber mensajes en tránsito entre procesadores

### Detección en anillos
Si existe un solo token y la conexión es unidireccional

En cualquier instante:
- Un proceso esta desocupado si terminó o esta suspendido ejecutando `receive`
- Despues de recibir un mensaje, un proceso se vuelve activo

Si un token esta desocupado y tiene el token, inicia el algoritmo para detectar detención

Si token llega de vuelta al mismo nodo en el que partió, y éste está desocupado, se sabe que el algoritmo distribuido que se estaba ejecutando terminó

#### Múltiples conexiones
Si existe de una conexión por nodo, el algoritmo requiere que se pase el token por cada uno de los caminos posibles (ciclo de Euler) para detectar detención

Se requiere contar el numero de veces consecutivas que ha pasado por un nodo inactivo

Dado que rojo es activo y azul es inactivo

Al recibir el token:
- Si el valor del token es igual a la cantidad de nodos, se anuncia terminación y se detiene
- Si el proceso esta rojo, se pinta azul, pone el token en 0 y lo envia
- Si esta azul, se incrementa el valor del token y se pasa

## Algoritmo basado en arbol de cobertura
### Version Simple
- Cada hoja le reporta a su padre si ha terminado
- Cada nodo reporta a su padre cuando han temrinado y todos sus hijos han terminado
- El proceso raíz determina cuándo ocurre la detención

### Version segura, basada en colorear tokens
1. Todos los tokens inician azules
2. Si un nodo manda un mensaje a otro token se vuelve rojo
3. Si un nodo se desocupe le manda un token de su color propio al padre y se cambia a color azul
4. Si se recibe un token rojo y uno azul de los hijos, se envia un token rojo al padre al terminar
5. Si el token padre recibe un token rojo al final, se reinicia el algoritmo de deteccion

### Algoritmo de Dijkstra
Se requiere que exista un "nodo de entorno" que no reciba ningun mensaje durante la computación regular

- Por cada mensaje recibido por un nodo se manda una confirmacion inversa
- La diferencia entre el numero de mensajes recibidos por `node[i]` y señales de confirmacion enviadas a traves de arista `e` es `inDeficit[e]`
- La diferencia entre el numero de mensajes enviados por todas sus aristas  el numero de señales recibidas `outDeficit`
- Cuando un nodo termina no envia mas mensajes pero sigue enviando señales mientras `inDeficit[e]!=0`
- Si outDeficit_env = 0, se anuncia terminación

Un nodo envia señales de confirmación si
- InDeficit > 1
- InDeficit = 1 y isTerminated() (nodo ya terminó) y outDeficit = 0

# Instantáneas globales
Es un registro consistente de los estados de todos los nodos y canales en un sistema distribuido

Para que sea consistente, cada mensaje debe estar en uno de dos estados
- enviado y en transito a un canal
- recibido

No se requiere que toda la informacion se recolecte a un instante en particular

## Estado de los nodos y canales
- Estado de nodo: Valores de sus variables internas
- Estado canal: Secuencia de mensajes que se enviaron por canal pero no estan recibidos
    - Mensajes enviados por nodo A, recibidos por nodo B y los que aún están en el canal
    - Mensaje marker separa los mensajes enviados antes y después de tomar instantánea

## Algoritmo
Supongamos que un nodo inicia el algoritmo
- Envío `send c(marker, myID)` a todos los canales `c` en mi output

- Se debe guardar el último mensaje enviado a ese destino
- Se debe agregar un proceso para recibir marker
- Se debe agregar un proceso que regista el estado una vez ha recibido markers por todos sus canales de input

Cuando un nodo recibe el primer marker (orden de comenzar a registrar estados)
1. Se registra el estado en sus canales de output
2. Se registran los canales recibidos por los canales de entrada
    - todos los mensajes recibidos por el canal donde llego el marker hasta antes del marker
    - la diferencia entre el último mensaje recibido hasta antes de que el nodo registrara su estado y el último recibido antes de recibir el marker
3. envia marker por cada canal

1. Recibo mensaje por un canal
2. Guardo el ultimo mensaje recibido por ese canal (`messageAtMarker[canal]`)
3. Si es el primer marker
    1. Guardo el array de todos los últimos mensajes que envié a cada canal (stateatRecord)
    2. Guardo el array de todos los últimos mensajes que recibí en cada canal (messageatRecord)
    3. Por cada canal de output, envío marker y mi id
4. Espero a recibir markers a través de todos los canales de input

Los mensajes del canal serán desde `messageAtRecord[channel]+1` hasta `messageAtMarker[channel]`

# Problema filósofos comensales
## Solución descentralizada
Cada filósofo es atendido por un mozo (proceso waiter) a traves de los canales hungry, eat, done

Cada filósofo
1. send hungry()
2. receive eat()
3. send done()

Cada mozo tienes canales para pedir los cubiertos
- needL, needR, passL, passR

Cada mozo
1. Si recibe hungry
    1. envia needL y needR
    2. recibe los cubiertos y se los da al filosofo
    3. envio los cubiertos de vuelta

# Relojes lógicos
Es imposible sincronizar todos los relojes fisicos por lo que no se pueden usar timestamps convencionales para ordernar

Un reloj logico es un contador que incrementa a medida que ocurren eventos (send, broacast, receive)

Cada reloj logico parte en 0 y cada mensajer contiene un timestamp de este reloj

## Algoritmo de Lamport
Sean:
- `A`, `B` y `C` procesos
- `lc` variable local de cada proceso que representa su reloj lógico. Todos inicializados en 0
- `send(ts)` envío de un mensaje con su timestamp (valor actual del reloj lógico)
- `receive(ts)` recepción de un mensaje con el timestamp del proceso que lo envió

El algoritmo de Lamport se define como:

1. Al enviar un mensaje, el proceso emisor realiza `send(lc)` con su valor de `lc` actual
2. Inmediatamente después, el mismo proceso incrementa `lc` en 1
3. Al recibir un mensaje, el proceso receptor actualiza el valor de `lc` como el máximo entre: su valor actual de `lc` y el timestamp+1
4. Luego, incrementa en 1 el valor de `lc`

Se incrementa el `lc` después de recibir para que no hayan dos eventos a la misma hora lógica. Por lo mismo, los timestamps deben crecer monotónicamente.

Si dos procesos tienen algún evento en la misma hora lógica, se distinguen por número de proceso.

## Multicast totalmente ordenado

Para resolver problemas de dependencia de eventos (*e.g.* lectura sucia en bases de datos) los mensajes (con su respectivo timestamp) se guardan en una cola. Los ack siguen la misma regla.

La idea del multicasting es que todos los procesos reciban los mensajes en el mismo orden, resolviendo problemas de dependencia.

- Cada mensaje es recibido por todos los procesos, incluido su emisor (*i.e.* broadcast)
- Cada mensaje tiene un timestamp asociado
- Cada proceso tiene una cola de prioridades en donde la prioridad es el timestamp (menor timestamp, más prioritario)
- Cuando un proceso recibe un mensaje regular, envía un ACK con su timestamp. Notar que este timestamp debe ser mayor al del mensaje original
- Dado que estamos en *broadcast*, eventualmente todos los procesos tienen la misma cola
- Si el mensaje ha sido reconocido (*acknowledged*) por todos los procesos, se elimina de todas las colas en conjunto con sus ACKs

## Semaforos distribuidos
En sistemas de memoria comaprtida, representamos un semáforo s como un entero

- `P(s)` espera hasta que `s` sea positivo y lo decrementa
- `V(s)` incrementa `s`

El numero de operaciones P terminadas es igual a el numero de operaciones V terminadas + s

### Implementación
En cada proceso
- Una cola local de mensajes mq
- Un reloj logico lc

Para ejecutar al operacion P o V un proceso hace broacast incluyendo
- Identidad
- Etiqueta P o V
- timestamp

Broadcast es una operación atomica, los mensajes enviados por dos procesos pueden ser recibidos en ordenes distintos

En el multicast totalmente ordenado, los mensaje se en el mismo orden por cada receptor

### Mensajes totalmente recibidos y prefijos estables
Si un mensaje `m` tiene un timestamp ts y se reciben mensajes de todos los otros procesos con timestamps > ts, entonces `m` esta totalmente recibido

Ningun mensaje va a tener un timestamp menor que ts

La parte de la cola que tiene mensajes totalmente recibidos se denomina "prefijo estable"

### Mensajes ACK de recepcion
Cada vez que se recibe un P o V se envia un ACK con un timestamp normal y su proposito es apurar la determinacion de cuando un mensaje se vuelve totalmente recibido

### Actualizacion de prefijos estables
Cada proceso guarda una variable que representa el valor del semaforo

Por cada mensaje en el prefijo esatble
- Si es V, incremento s y elimino mensaje
- Si es P, reviso en orden de timestamp. Si s>0 decremento s, y elimino el mensaje

Todos los procesos toman la misma decisiona cerca del orden en que finalizan las operaciones

### user y helper
En cada nodo ocurre que

1. user solicita a helper la ejecuciones de P o V
2. en caso de que sea P, user espera hasta autorización
3. helper entonce se encarga de: 
    - maneja la cola de mensajes
    - el valor del semaoro
    - broacast de solicitudes
    - recepciones de mensaje P o V mandado por otros helper, incluyendose

## Relojes vectoriales
Los timestamps pasan a ser vectores con los valores de los vectores logicos de todos los otros procesos

Cada vez que envio un mensaje, envio el vector entero de timestamps

Actualizo cada entrada del vector al mayor valor que haya visto para esa entrada

### Reglas de actualización
- Antes de ejecutar un evento actualizo mi reloj `V[i] += 1`
- Al enviar un mensaje adjunto mi reloj vectorial
- Al recibir un mensaje, ajusto mi propio vector `V[k] = max(V[k], ts(m)[k]) para todo k` 
- Al recibir un mensaje le sumo 1 a mi reloj

### Causalidad
Definamos la relacion
$$ts(a) < ts(b) \iff \forall k, ts(a) \leq ts(b) \wedge \exists k', ts(a)[k'] < ts(b)[k']$$

Entonces se puede notar que ts(m1) < ts(m2) podria ser que m2 es causalmente dependiente de m1

# Consenso
Como ponerse de acuerdo respecto a algo

## Sistema confiable
- Inocuo ante fallas (fallas no producen daños)
- Tolerante a fallas (continua si hay fallas)

La replicación es uno de los metodos en lo que se puede lograr un sistema confiable

## Como lograr consenso
### Si no hay fallas
El algoritmo para lograr consenso se puede realizar una votación simple

### Si hay fallas
Consideremos dos tipos de fallas de los procesos
- Fallas simples: Proceso deja de funcionar
- Fallas bizantinas: Un nodo puede enviar mensajes arbitrarios diseñados para que el consenso falle

Resolver este tipo de problemas requiere poder asumir que si un computador deja de enviar mensajes despues de un cierto tiempo, este dejo de funcionar

#### Problema de los generales bizantinos
- Un grupo de ejercitos bizantinos rodea una ciudad
- Solo pueden capturar la ciudad si todos atacan juntos
- Los generales tienen mensajeros completamente confiables
- Los generales pueden ser traidores que intentan que no se llegue a consenso

Hay que diseñar un algoritmo para que todos lo generales lleguen a consenso

### Algoritmo simple de una rueda
1. Cada nodo envia mensajes y recibe respuestas sobre la cual decision se va a tomar
2. Si la votación es empate entonces se retiran

Si un nodo se cae, el resto de nodos no puedan llegar a consenso

### Algoritmo doble rueda
1. Se hace una rueda simple 
2. Por cada nodo envio lo que recibi acerca de los otros nodos
3. Digo que el voto de un nodo va a ser igual a el voto de mayoria entre los valores reportados para ese nodo y los valores recibidos

Este algoritmo resuelve el problema con fallas simples y el de fallos bizantinos para 4 nodos si 1 falla

Por cada traidor adicional hay que enviar una vuelta adicional de mensajes (que dijo a sobre que dijo b que dijo c)

El numero de generales debe ser $n \geq 3t+1$ con t la cantidad de traidores
    

# Algoritmos de eleccion
## Bully
Si un proceso se da cuenta que el coordinador no responde entonces:
1. k envia mensaje election a todos los procesos con identificadores > k
2. Si nadie responde, k gana y se vuelve coordinador
3. Si uno responde, entonces ese se hace cargo

## Basado en anillo logico
Cada proceso tiene un sucecesor

1. Un nodo inicia con un mensaje Election con una cola a la cual se añade
2. Busca el primero de sus sucesores que conteste y lo envia
3. Cada nodo al recibir el mensaje se añade a la cola
4. Cuando da la vuelta entera se elige el con mayor indice

## Para entornos inalambricos
1. Un nodo cualquier inicia el algoritmo y envia election a todos sus vecinos
2. Cuando un nodo recibe election por primera vez, se asigna como padre el que envio y se envia todos sus vecinos
3. Si se recibe de un nodo diferente se envia un ack


# Determinacion de topologia
## Lamport
1. El primer nodo envia una sonda a sus vecinos
2. Cada nodo repite lo mismo
3. despues cada nodo envia la respuesta con la informacion de la topologia al nodo desde que recibio la sonda

# Arquitecturas descentralizadas p2p

Todos los nodos que forman un sistema p2p son iguales

En un sistema peer to peer existen muchas posiblidades de conexiones

Existe simetria en la mayor parte de interacciones

## Overlay Network
Consiste en superponer una red sobre los nodos para forzar un patron de conexion

Existen dos tipos
### No estructuado
Cada nodo tiene una lista ad hoc de vecinos

- El overlay resulta aleatorio
- Se cambia de vecinos continuamente
- Para buscar un dato no hay ruta predeterminada


#### Flooding
Para buscar un dato
1. Nodo u envia una solicitud de busqueda a todos sus vecinos
2. Si un nodo ya lo recibio la solicitud, se ignora
3. Si no busca localmente el dato
4. Si no tiene el dato entonces solicita a todos sus vecinos
5. Si lo tiene, responde al nodo el cual recibio la solicitud

Es un algoritmo muy costoso en terminos de comunicacion

Se limita la cantida dde saltos de los mensajes con un TTL

#### Random walks
1. Un nodo esolicita un dato a un vecino de manera aleatoria
2. Si no tiene l dato, reenvia la solicitud a otro vecino elegido aleatoriamente

Puede demorarse mucho en buscar el dato pero se suelen iniciar varias random walks

## Estructurado
## Hashing
Es util asignar un hash a cada proceso y dato

Luego con una tabla de hash distribuida se puede buscar donde esta un cierto proceso o dato

Con la tabla de hash se donde ir, con la topologia se como llegar

#### Superpeers
Una alternativa es designar nodos especiales que mantienen indices de los  datos o actua como broker

Todo nodo regular (weak) esta conectado a un super peer

Para elegir un super peer debe cumplir que
1. Conexion rapida
2. Distribucion pareja
3. Numero maximo de nodos atendidos 

##### Gravitacion
Una alternativa es poner a los nodos en un espacio 2d

1. Asignamos N superpeers con N tokens
2. Cada token representa una fuerza de repulsion que hace que otro token se aleje
3. Si todos los toquen ejercen la misma fuerza, se van a alejar unos de otros, por lo que estaran distribuidos de manera uniforme
4. El token se pasa cuando las fuerzas sobre el exceden cierto limite
5. Despues de tener el nodo un tiempo, el nodo se vuelve super peer

### Chord
Otra alternativa es que cada nodo tenga un identificar de bits y utilizar los primeros bits  para marcar a los superpeers

Un dato con clave k de m bits se almacena en un nodo con el menor identificador tal que id >= k

Existen atajos entre nodos para poder ir rapidamente a los recursos necesitados

Estos atajos son asignados mediante algun algoritmo

Permite la union y la secesion de nodos

# Consistencia y replicación
## Razones
- Confiabilidad
- Desempeño

## MIMD
Los multiprocesadores utilizan una memoria compartida

Los multicomputadores enviar y reciben mensajes

### Multiprocesador
Mientras mas lejano el procesador más ciclos se requieren para acceder a la memoria

#### UMA
Acceso uniforma a la memoria, cada procesador tiene su propio cache y además hay uno compartido

Al usar un unico BUS, el tamaño maximo actualmente es de al rededor de 32 CPU

#### NUMA
Cada procesador tiene su memoria y existe una red de interconexion que sirve para comunicarse

Si no tienen cache, la coherencia de memoria esta garantizada

## Modelo de consistencia data-centric
Es un contrato entre los procesos y la informacion

Si lo procesos siguen ciertas reglas, entonce el sistema funciona correctamente

## Soluciones
Para resolver estos problemas es necesario tener relojes logicos para definir cual fue el ultimo write

La replicacion de datos no puede ser resuelta de forma eficiente por lo general eficia implica relajar consistencia

Existen multiples modelos de consistencia

### Consistencia Secuencial
El resultado de cualquier ejecucion es el mismo que si las operaciones si ejecutasen en algun orden secuencial

### Consistencia Causal
Si hay dos operaciones tal que una es potencialmente causa de la otra, nadie puede ver la segunda antes de la primera

Las operaciones que no estan causalmente relacionadas se denominan concurrentes

### Consistencia de Entrada
Se asocia un lock a los datos y se utilizan los locks bajo consistencia secuencial

# Lectores y escritores
- Lectores solo leen BD
- Escritores la actualizan
- La BD parte en un estado consistente
- Un escritor debe tener acceso exclusivo a la BD
- Los lectores pueden leer todos al mismo tiempo

# Referencias Globales
## Arquitecutra basada en objetos
Cada objeto es un componente

Los componetnes estan conectados a traves de RPC

Ofrece una forma de encapsular una entidad unica

La interfaz del objeto oculta detalles de implementación

Un proxy se encarga del marshaling y el skeleton del unmarshaling

## RPC Asincrono
A veces no es necesario esperar a que el servidor devuelva los resultados por lo que se puede hacer una llamada asincrona y solo esperar confirmacion de recepción

## RPC Sincrono Referido
Se llama al servidor y despues se puede decidir cuando esperar la respuesta

### RPC Multicast
UN cliente puede envair a multiples servidores

## Arquitectura Pub/Sub
Existe acoplamiento temporal pero no referencial

Es un desafio implementar de manera eficiente y escalable el matching entre subs y notififaciones sin perder bajo acoplamiento

## Shared Space
Se crean 3 operaciones
- in
- read
- out

Se publica y cualquier puede leer

# Middleware
Es un capa de software que se encuentra sobre el sistema operativo

## Rol
Middleware es a un sistema distribuido lo mismo que un sistema oeprativo es a un computador

Gestiona recursos de manera eficienet a traves de una red

Oculta a las aplicaciones (como tan razonablemente sea posible) los detalles

## Organizacion
Se suelen utilizar 
- Wrappers, Adapters
- Interceptores (permiten ejecucion de codigo entre medio de llamadas)

Para lograr `openness`

Los componentes deben obedecer reglas estandares que describen la sintaxis y semantico de los servicios que proporcionan

### Brokers
Los wrappers a veces no escalan bien ya que se necesitan muchos por cada una de las apliaciones

Los brokers son un mecanismo que simplifica la implementaciones de mulitples wrappers ya que conoce a todas las aplicaciones y ofrece interfacs para cada una

Las aplicaciones le piden al broker que contacte la otra aplicacion

Esto tiene la desventaja de tener un solo punto de falla

# Procesos
Los procesos y threads son una forma de hacer mas cosas al mismo tiempo

## Virtualizacion
Permite extender o reemplazar una interfaz existente para imitar el comportamiento de otro sistema

La virtualizacion permtite que cada aplicacion corra en su propia maquina virtual (con sus propias librerias y sistema operativo)

La escencia de la virtualizacion es imitar library functions, system calls, privileged instructions, general instruction

### Niveles
1. Sistema que corre sobre el sistema operativo el cual traduce las system calls
2. Sistema implementado como una capa que esconde el hardware original (virtual machine)
3. Un monitor de maquina virtual hosted que corre encima de un sistema operativo anfitrion

# Servidores
### Stateless
No almacenan informacion del cliente

- Soft state: Se mantiene un estado por un tiempo limitado
- Session state: Seria de operacion por un unico usuario durante un tiempo limitado
### Stateful
Mantienen estado de los clientes

### Servidores de objetos
- Ofrecen un medio para invocar objetos
- Los servicios son implementados por estos objetos
- Es fácil cambiar servicios agrando o sacando objetos

Los objetos tienen datos y codigos

#### Objetos transitorios
Existen solo mientras el servidor existe o menos


Una politica es que el objeto usara recursos solo mientras sea necesario, teniendo que ser creado cuando se necesita

La otra es tenerlos todos creados lo que tiene un costo mayor

Los objetos de la msima clase pueden compatir codigo

## Threads
Opciones
- Tener un solo thread
- Un thread por cada objeto (lo que protege objetos de accesos concurrentes)
- Thread por cada invocación

### Politicas de despacho
- Incondicionalmente justa: Toda accion atomica incondicional va a ser ejecutada
- Debilmente justa: Incondicionalmente justa y todas las acciones atomicas condicionales que permanecen verdadera van a ser ejecutadas 
- Fuertemente justa: Incondicionalmente justa y toda accion condicional elegible es ejecutada finalmente

### Locks
Se requieren instrucciones atomicas para poder hacer locks correctos, estas instrucciones pueden ser:
```python
bool TS(bool lock):
    bool init = lock
    lock = true
    return init
```

```python
bool FA(int var, int incr):
    int tmp = var
    var += incr
    return tmp
```

## Politica de activacion
Son las decisiones sobre como invocar un objeto

Un adaptador es un mecanismo para agrupar objetos segun la politica de activacion (sofware que implementa politica)


## Clusters
Coleccion de computaores conectados a traves de una red

Cada computador corre uno o mas servidores

### Separacion 3 capas
#### Switches
Unico punto de acceso al cluster, tiene una unica direccion de red

Pueden haber multiples switches

Es un trasport layer switch el que acepta solicitued de coenxion y las pasa a un servidor

Cuando el servidor responde se hace pasar por el switch

#### Content Awaer
La ventaja es que el switch podria comenza ra hacer caching para mejorar los tiempos de respuesta

## Clusters de area ancha
Proveedores de cloud manejan data centers dispeustos en diferentes ubicaciones en todo el mundo

Ofrecen a sus usuarios la capacidad de constrir sistemas distribuidos de area ancah formados por un conjunto grande ade maquinas virtuales

# Fallas
Un sistema distribuido es mas resistente a fallas debido a que deben caerse todos los nodos para dejar de funcionar

La resistencia a fallas esta relacionada a los sistemas "dependables"

## Dependability
- Availability
- Reliability
- Safety
- Maintainability

## Tipos de fallas
- Crash failure
- Omission failure
    - Receive omission
    - Send omission
- Timing failure
- Response failure
    - Value failure
    - State transition failure
- Arbitrary failure

## Sistemas
### Async
No se puede asumir sobre la velocida de ejecucion o del envio de mensajes

No se puede saber si hubo un error o solo es demora
### Sync
Se garantiza que los mensajes se recibiran en un tiempo especifico

### Parcialmente sync
Casi siempre se ejecuta como un sistemas sincronico pero no esta garantizado

Normalmente se utilizan timeouts para saber si un proceso se cayo

Las soluciones ante fallas deben tomar en cuenta que puede ser que no se haya caido realmente

## Estrategia
Lo que hacen los sistemas tolerantes a fallas es que ocultan la ocurrencia de fallas a los otros procesos

Una forma es usar redundancia

### Information redundancy
Bits extra que permiten recuperar bits perdidos o revisar si es que esta completo el mensaje

### Time redundancy
Se hace una accion y si es necesario se hace de nuevo

Si la accion no esta terminada se cancela
