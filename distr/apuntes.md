# Sistemas distribuidos

Coleccion de entidades independientes que cooperan para resolver un problema

- Para comunicar, se envian y reciben mensajes en vez de compartir memoria

- No existe certeza de si de recibio el mensaje

- No comparten memoria ni reloj

- Hay autonomía y heterogeneidad (debilmente acoplados)

- Se espera que el usuario vea la coleccion de computadores como un solo computador

Una ejecucion distribuidas es la ejecución de procesos a través del sistema sistribuido para conseguir un objectivo en común

## Red de comunicaciones
Se podrian usar operaciones read/write (como en variables compartidas) pero los programas tendrian que implementar busy waiting

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

#### Algoritmo sacar numero (memoria compartida)

Cada proceso que quiere entrar a su sección critico saca un numero y espera su turno para correr la sección critica

Esto requiere memoria compartida porque todos los procesos necesitan saber cual es el siguiente numero y cual es el numero actual

#### Algoritmo panaderia (memoria compartida)

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
2. Si estoy no quiero entrar, acepto
3. Si el turno es menor al mio, entonces apruebo
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

#### Algoritmo Ricart-Agrawala con Tokens
Cada proceso tiene 2 arreglos
- granted: El numero que tenia cada turno la ultima vez que se metio a la sección critica
 - Este array va includio en el mensaje que tiene al token, y la unica copia significativa es la que va con el token
- request: El numero que acompañaba al ultimo mensaje que rqst de cada nodo

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

# Arquitecturas


# RPC (Remote Procedure Core)
Es un mecanismo de invocación remota que sirve para llamar procedimientos de forma remota.

La llamada es bloqueante y se espera a recibir respuesta



## Mensajes asincronos
Son apropiados si la informacion fluye por los canales en una dirección pero no para procesos de tipo cliente y servidor

Cada cliente necesitaria un canal de respuesta diferente, por lo que abrian muchos canales

Para interacciones cliente-servidor es mejor usar RPC

## Mecanismo RPC
Un programa esta compuesto por modulos que contienen procesos y procedimientos

Un proceso en un modulo se comunica con otro proceso en otro modulo llamando procedimientos exportados

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
Es como deben ser organizados componentes del sismtea y como deben interactuar
- Componentes: Unidad modular con inferfaces
- Conectores: Mecanismos que median comunicacion

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

### Arquitectura de dos capas físicas
Existe un cliente que contiene solo los programas que se utilizan a nivel de inferaz de usuario y un servidor que se encarga de todo el resto

### Arquitectura de tres capas fisicas
- Cliente
- Middleware
- Servidor de datos


## Separación entre procesamiento y coordinación
Existen
- Acomplamiento referencial: Un proceso solo puede comunicarse cuando conoce el identificador de otro
- Acomplamiento temporal: Ambos deben estar corriendo al mismo tiempo

### Arquitectura publicacion-suscripcion
El servidor tiene una referencia del cliente y cada vez que ocurre algo el servidor envia la información al cliente
- Basado en tópicos (atributo, valor)
- Basado en contenidos (atributo, rango)

#### Patron observador
- Un Sujeto publica informacion
- Los objetos que se suscriben implementan interfaz Observador
- Cada Observador tiene un metodo actualizar
- Sujeto permite registrar observadores y llama a su metodo actualizar cuando los datos se actualizan
- La clase que representa los Datos hereda de sujeto

# Detección de terminación
## Casos simples
Cuando un programa esta ocrrindoe en un unico procesador, se que sabe que ha terminado si:
- Cada proceso esta bloqueado o terminado
- No hay operaciones I/O

## Casos distribuidos
En un programa distribuido:
- El estado global no es visible para ningun procesador
- Incluso si todos los procesadores estan desocupados, puede haber mensajes en transito entre procesadores

### Detección en anillos
Si existe un solo token y la conexión es unidireccional

En cualquier instante:
- Un proceso esta desocupado si terminó o esta suspendido ejecutando receive
- Despues de recibir un mensaje, un proceso se vuelve activo

Si un token esta desocupado y tiene el token, inicia el algoritmo para detectar detención

Si token parte y termina en el mismo nodo que esta desocupado, se sabe que el algoritmo distribuido que se estaba ejecutando terminó

#### Multiples conexiones
Si existe de una conexión por nodo, el algoritmo requiere que se pase el token por cada uno de los caminos posibles (ciclo de euler) para detectar detención

Se requiere contar el numero de veces consecutivas que ha pasado por un nodo inactivo

Dado que rojo es activo y azul es inactivo

Al recibir el token:
- Si el valor del token igual a la cantidad de nodos, se anuncia terminación y se detiene
- Si el proceso esta rojo, se pinta azul, pone el token en 0 y lo envia
- Si esta azul, se incrementa el valor del token y se pasa

## Algoritmo basado en arbol de cobertura
### Version Simple
- Cada hoja le reporta a su padro si ha terminado
- Cada nodo reporta a su padre cuando han temrinado y todos sus hijos han terminado
- El proceso raiz determina cuando ocurre la detención

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
- InDeficit = 1 y isTerminated() y outDeficit = 0

# Instaneas globales
Es un registro consistente de los estados de todos los nodos y canales en un sistema distribuido

Para que sea consistente, cada mensaje debe estar en uno de dos estados
- enviado y en transito a un canal
 recibido

 No se requiere que toda la informacion se recolecte a un instante en particular

 ## Estado de los nodos y canales
 - Estado de nodo: Valores de sus variables internas
 - Estado canal: Secuencia de mensajes que se enviaron por canal pero no estan recibidos
    - Mensajes enviados por nodo A, recibidos por nodo B y los que aun estan en el canal
    - Mensaje marker separa los mensajes enviados antes y despues de tomar instantanea

## Algoritmo
Supongamos que un nodo inicia el algoritmo
- Envio send c(marker, myID) a todos los canales c en mi output

- Se debe guardar el ultimo mensaje enviado a ese destino
- Se debe agregar un proceso para recibir marker
- Se debe agregar un proceso que regista el estado una vez ha recibido markers por todos sus canales de input

Cuando un nodo recibe el primer marker (orden de comenzar a registrar estados)
1. Se registra el estado en sus canales de output
2. Se registran los canales recibidos por los canales de entrada
    - todos los mensajes recibidos por el canal donde llego el marker hasta antes del marker
    - la diferencia entre el ultimo mensaje recibido hasta antes de que el nodo registrara su estado y el ultimo recibido antes de recibir el marker
3. envia marker por cada canal

1. Recibo mensaje por un canal
2. Guardo el ultimo mensaje recibido por ese canal (messageAtMarker[canal])
3. Si es el primer marker
    1. Guardo el array de todos los ultimos mensajes que envie a cada canal (stateatRecord)
    2. Guardo el array de todos los ultimos mensajes que recibi en cada canal (messageatRecord)
    3. Por cada canal de output, envio marker y mi id
4. Espero a recibir makers a traves de todos los canales de input

Los mensajes del canal seran desde messageAtRecord[channel]+1 hasta messageAtMarker[channel]

# Problema filosofos comenzales
## Solucion descentralizada
Cada filoso es atendido por un mozo (proceso waiter) a traves de los canales hungry, eat, done

Cada filosofo
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
 
<!-- para la prueba -->
<!-- # seccion critica distribuida -->
<!-- # rpc -->
<!-- # algoritmos de terminación -->



