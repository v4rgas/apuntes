# Sistemas Operativos

Permite utilizar los recursos del computador

Abstrae el hardware

## Funciones

Algunas son:

- Sistema de archivos
- Manejo de memoria
- Administración de procesos
- Control de IO
- Manejo de errores
- etc...

### Syscalls

El sistema operativo provee una inferfaz de llamas al sistema que permite invocar los servicios de este

Las interfaces de usuario que enmascaran syscalls

Hay syscalls estándar que todos los SO tienen

#### POSIX

- fork: clona el proceso desde la linea actual + 1
- exec:
- wait: espera a los procesos hijos

## Componentes

### Kernel

Programa que siempre se esta ejecutando, provee acceso a CPU, memoria y dispositivos

#### Monolítico

No existe separación entre el espacio del usuario y el espacio del Kernel

Todos los servicios se ejecutan en modo kernel

Todo es un único programa extensible

#### Microkernel

Mantiene el menor tamaño posible

Desventaja es el overhead

#### Kernel híbrido

Construido como

### Programa del sistema

Extienden las funciones del kernel

## Modos de protección

El hardware posee dos modos de protección:

- kernel mode: acceso completo al hardware
- user moed: set restringido de restricciones
  Si esta en usermode el hardware genera protection fault

Al iniciar el computador se ejecuta código de la tarjeta madre

- BIOS
- UEFI
  Este codigo tiene la mision de inicializar el hardware y arrancar un bootloader al kernel

El kernel es el unico software que funciona en modo privilegiado

### Anillos

El hardware moderno posee 4 o mas modos de proteccion llamados rings (anillos) de protección

Existe un set de instrucciones para cada anillo

Instrucciones privilegiadas (ring 0)

- Modificar vector de interrupciones
- Acceder IO
- Modificar timer del computador
- etc...

## Como funciona

Mientras nadie llame al SO, no hace nada

Si el usuario ejecuta algo:

1. El programa de usuario genera una interrupción generada por software (trap)
2. Se pasa al kernel space
3. Se ejecuta servicio
4. Se regresa el control al programa del usuario

# Procesos

Proceso = programa + recursos

La multiprogramación permite mantener multiples procesos en memoria

- El procesador los atiende en un cierto orden (scheduling)

- Cuando los atiende simultáneamente es multitasking

**1 nucleo corre 1 proceso**

## Componentes

- Codigo: estatico
- Datos: variables globales
- Stack: parametros, variables locales y entorno
- Heap: memoria asignada dinamicamente

El stack parte en el SBP, termina en SP y crece hacia abajo

## PCB

EL SO mantiene una tabla de procesos con un PCB para cada proceso

El PCB mantiene la información de cada proceso como:

- Estado
- Identificador
- Program counter
- etc...

### Estados

- New (fork)
- Ready (en memoria)
- Running
- Waiting (blocked, sleeping)
- Terminated (terminated, killed)

Cuando el proceso esta en ejecución pasa por el ciclo:

Ready -> Running -> Waiting

## Context Switch

Cuando se cambia de proceso al hacer multitasking se almacenan los estados del proceso actual (P1) en un registro (PCB1)

Luego se cargan los datos del otro proceso (PCB2) y se continua su ejecución (P2)

## Creación de procesos

Un proceso (parent) crea otro proceso (child) a través de un fork

Esta relación crea un árbol de procesos

El proceso raíz se crea durante la inicialización del kernel

### Fork

Fork crea un nuevo proceso como copia del padre y retorna el PID del hijo al padre y al hijo le retorna 0

### Exec

Carga un binario en la memoria reemplazando el codigo de quien los llamo

### Exit

Termina el proceso con un codigo de retorno dado y lo entrega al padre

## Señales

Un proceso puede enviar señales a otros procesos

- SIGTERM: le indica al proceso que debe terminar
- SIGKILL: quita el proceso de la tabla de procesos

existen muchas mas

### Huerfanos

En linux los hijos del proceso padre pueden seguir corriendo incluso si el padre terminó

Estos procesos son denominados "huerfanos" y pasan a ser parte del proceso init

init hace wait() periodicametne a los hijos

### Zombie

Es un proceso que ya ha terminado su ejecución pero todavia tiene una entrada en la tabla de procesos

Ocurre cuando un proceso termina y el padre no hace wait() (o sea que el padre no ha podido leer el codigo de exit)

# Scheduling

El SO debe elegir cual es el siguiente proceso (ready) que se va a ejectar

El responsable de esto es el scheduler

## Scheduler

Existen distintos niveles

- Long term: Admite procesos en la cola ready y determina grado de multiprogramación (Carga procesos en memoria)

- Short term(dispatcher): Selecciona uno de los procesos de la cola y ejecuta cambio de contexto (Ejecuta en CPU ahora)

- Medium term: Modifica temporalemnte grado de multiprogramacion y ejecuta swapping (RAM y disco)

### Thrashing

Si los grados de multiprogramacion son demasiados la mayoria de los recursos se iran en page faults. Esto es denominado thrashing

## Modelo de ejecución

Los procesos alternan entre dos estados

- CPU-BURST (Uso de CPU)
- IO-BURST (Espera de IO)

## Utilización

Si p es la probabilidad de que un proceso este en espera de IO

El uso del CPU para n procesos esta dado por

$$CPU = 1-p^n$$

## Tipos de scheduling

### Por tipo de interrupcion

- Preemptive (expropiativo): Utiliza interrupciones para decidir cuando sacar un proceso de ejecución

- Non-Preemptive (colaborativo): Se espera a que el proceso termine voluntariamente, se bloque en I/O o que termine

### Por objetivo

- Batch: Mantener CPU al maximo, minimizar turnaround(tiempo en el sistema) time y maximizar throughput
- Interactive: Minimizar tiempo de respuesta
- Real time: Alcanzar deadlines

## Algoritmos

### First Come, First Serve

- Non-Preemptive
- Simple
- Poco predecible, convoy effect

### Shortest Job First

- Version Preemtive: Elijo al que le queda menos tiempo
- Potencial inanición de procesos largos
- Optimo
- No se sabe cuanto demora cada CPU-BURST

### Interactive

#### Round Robin

- Cada proceso recibe una cantidad igual de tiempo
- Los procesos son atentidos muy rapido
- Los procesos largos se van a demorar mucho más de lo que se demorarian con otros algoritmos

#### Priority Scheduling

- Se atienden por prioridad
- Prioridades iguales, RR o FCFS
- Incrementar prioridad de procesos que llevan mas tiempo se denomina Aging

#### Multilevel Feeback Queue

- Se tienen muchas colas que dependen de la prioridad
  - A > B, A
  - A = B, A y B con RR
  - Los procesos entran en la cola con mayor prioridad
  - Si un proceso usa su q, prioridad se reduce
  - Despues de un tiempo S, todo se mueve a la cola con mayor prioridad
  - Los procesos cortos salen mas rapido y los más largos descienden a una cola de menor prioridad

### Real Time

El sistema determina si dado un deadline, un periodo y un tiempo de ejecucion es capaz de incoporar el proceso a la ejecución

#### Monotonic

- prioridad es $\frac{t}{t}$

# Threads

Es un proceso pero mas liviano

## Componentes

- Thread ID
- PC
- Registros
- Stack

Procesos pueden tener uno o mas threads

## Concurrencia

SI un CPU tiene multiples cores, podria existir concurrencia real pero sino, no necesariamente se ejecutan al mismo tiempo

A veces es mejor ahorrarse los saltos de contexto y utilizar solo un thread

Creacion y destruccion de threads es mas ligera que un proceso
Threads

## Que comparten

Los threads NO comparten

- Stack
- Registro
- Estado

Todo lo otro si

## Implementacion

### User Space

Manejados por una biblioteca de usuario

- No hay syscalls
- Tabla de threads vive dentro de cada proceso
- Kernel no ve threads
- Syscalls de un thread pueden bloquear el proceso porque todos son parte del mismo proceso

Non-blocking syscalls solucionarian este problema

### Kernel Space

- Syscalls no bloquean el proceso
- Syscalls más costosas
- Limitada por el SO
- fork y signals no son claras

Para evitar crear y destruir threads existen pools de threads que se reutilizan

### Hibrido

Existen threads the kernel y threads de usuario

- El kernel solo ve threads de kernel
- Varios user threads asignados a un kernel thread

Kernel threads son multiplexados entre user threads

# Sincronización

## Race Condition

Salida de una operacion depende del orden temporal de sus operaciones internas, se deben evitar

## Sección critica

### Caracteristicas de solucion

- Exclusion mutua (a lo más un thread)
- Progreso (a lo menos uno puede entrar)
- Espera acotada (ausencia de inanición, si un proceso quiere entrar podrá hacerlo en un tiempo finito)

### Posibles soluciones

- Deshabilitar las interrupciones (se hace en el sis::tema operativo)
- Solucion de peterson

### Sincronización por hardware

#### test_and_set() TSL

- Recibo false, seteo a true y devuelvo valor anterior
- Recibo true, seteo a true y devuelvo valor anterior

Todo esto ocurre de forma atomica, por lo que es posible implementar un lock

#### compare_and_swap() (XCHG, CAS)

- Recibe un valor, lo compara con un valor esperado
- Si son iguales lo reemplaza por un valor nuevo
- retorna valor previo

#### Spinlock

ocurre cuando lock=true, while(lock)

## Primitivas de sincronización

### Mutex Locks

lock.acquire() toma el lock
lock.release() lo suelta (error si no esta tomando)

## Semaforos

- P(),wait(),down() decrementa valor
- V(),signal(),up() incremeta valor

Pueden partir con distintos valores valores inciales

### P

```C
void P() {
        l.acquire()
        while(count<=0) {
                l.release();
                sleep();
                l.acquire();
            }
        count-=1;
        l.release();
    }

```

### V

```C
void V(S) {
        l.acquire();
        count+=1
        wakeup(/* first from sleep queue */)
        l.release()
    }
```

## Variables de condicion

Permiten bloquearse bajo condiciones aleatorias

- wait() espera a un signal
- signal() comienza el wait

## Problema de los escritores

N accesos lectura pero solo un escritor

## Filosofos comensales

5 filosofos pasan la vida comiendo y pensando

- Para comer deben tomar ambos palillos
- Solo puedne tomar un palillo a la vez

# Adminstración de Memoria

## Memoria

### Direcciones de memoria

Direcciones absolutas no sirven con multiprogramación y apuntan a un lugar e la memoria

### Espacios de direcciones

Los CPUs desde CDC6600 hasta el Intel8088 integraban un registro base y uno limit

- base: cuando se cargaba un programa base se cargaba con la primera direccion de memoria fisica asignada
- limit: se cargaba con la ultima direccion de memoria asignada
  MMU revisa dir > base, dir < limit

Cada proceso mantiene un espacio unico y secuencial de direcciones y el sistema operativo se encarga de traducirlo

#### Estrategias espacios libres

- Compactacion: Ocurre cuando se fusionan los huecos de la memoria. No se hace por es muy caro
  La solucion es asignar espacios que no necesiten compactar

- First-fit: Agarro primer lugar disponible
- Best-fit: Agarro el que deje menos espacio libre (mas pequeño donde quepa)
- Worst-fit: Agarro el que mas espacio libre deje

La fragmentacon ocurre cuando existen espacios libres no contiguos

## Segmentación

Se divide el espacio de direcciones en espacios más pequieños

La MMU se encarga de traducir direcciones de la tabla de segmentos

Segmentation Fault ocurre cuando se intenta acceder a memoria no localizada

### Tabla de segmentos

| Segment | Base             | Size   | Upwards |
| ------- | ---------------- | ------ | ------- |
| code    | inicio de code   | tamaño | 1       |
| heap    | inicio del heap  | tamaño | 1       |
| stack   | inicio del stack | tamaño | 0       |

Los primeros bits de una direccion virtual se usan para el segmento, el resto son el offset

## Paginación

Los segmentos son del mismo tamaño y conforman una pagina de memoria

- Espacio virtual: Pagina
- Espacio fisico: Frame

Paginas y frames son del mismo tamaño

La tabla de paginas asigna una pagina a un frame

Los primeros bits son el VPN (numero de pagina) y se traducen al PFN (numero de frame). El resto de nuemeros son el offset dentro de la pagina

TLB es un cache que sirve para guardar las traducciones

### Variantes de paginacion

Hacer paginas mas grandes haria que la tabla use menos espacios pero provocaría fragmentación interna (espacio sobre asignado)

#### Segmentos paginados

Se podria tener una tabla de paginas por cada segmento (code stack heap)

#### Paginación multinivel

En vez de segmentar, se puede paginar la tabla de paginas

EL VPN se divide en 2, uno apunta a donde esta la tabla de paginas y la otra a la parte de la pagina que estoy buscando

Este esquema se puede extender para mas niveles

#### Tablas de paginas invertidas

En vez de una tabla por proceso, se puede tener una tabla unica para todo el sistema

Se tiene una entrada por cada frame

Hay que buscar de a una las entradas

## Reemplazo de paginas

Generalmente el espacio de direcciones no cabe en la memoria fisica

Los procesos pueden tener memoria no cargada aún (en disco o swap)

Los procesos creen que tienen acceso a todo el rango, el SO se encarga de cargar las paginas necesarias

Si no caben mas en la memoria fisica, se pueden eliminar

### Page fault

Las paginas usan un bit para saber si estan cargadas o no

En el caso que no este cargada entonces se hace Page Fault lo que carga la pagina del disco a la memoria fisica

Si page fault, cargo pagina y corro instruccion de nuevo

### Tipos de reemplazos

- Min: se saca la que va a ser usado mas lejano en el futuro

- FIFO: se saca la que lleva mas tiempo en la memoria

- Random: podria eliminar paginas que se usarane n corto plazo

- LRU: se saca la que lleva mas tiempo sin usarse (poco eficiente porque requiere cambios en cada acceso a la memoria)

Para mejorar LRU se podria mantener una cola ordenada (caro) o un timestamp (barato pero buscar la pagina es caro)

- Algoritmo del reloj: Cada pagina tiene un reference bit. Si esta en 1, se cambia a 0 y se pasa a la siguiente pagina, si esta en 0 se elige esa pagina para borrarla

El reference bit se hace 1 si la pagina fue usada

- Aging: Cada pagina tiene un contador actualizado por hardware. Cada vez que la pagina es accedida se le pone un 1 y se le va haciendo shift en cada tick del reloj

Sirve como una historia de accesos entonces se cual borrar

### bits

- Dirty Bit: Me dice si la pagina ha sido modificada con respecto a la copia en disco.

Si no ha sido modificada con respecto al disco, es muy facil de sacar

- Reference Bit: Me dice si ha sido usada recientemente

## Working Set y thrashing

Workin set $w_{\Delta}(t)$: El conjunto de paginas utilizadas en los ultimos $\Delta$ accesos

El working set es usado por el medium-term scheduler

Si la suma de los working set es mayor que la cantidad de frames, habra thrashing

# Administacion de Almacenamiento

## Dispositivos IO

- Device driver: Codigo que se encarga de interacción con dispositivo
- Block layer: Capa intermedia, abstraccion de bloques de disco
- Sistema de archivos: Capa visible al programador

### Transferencia de datos

- Unbuffered: Lectura directa (se bloquea usuario, se envian datos y se reanuda)
- Buffer en user space: Le entrego una posicion al sistema operativo la cual es asignada para el proceso y avisa cuando este disponible
- Buffer en el kernel space: Mismo que lo anterior excepto que el sistema operativo lee en un buffer interno y una vez este listo, escribe en el espacio de memoria asignado
- Double-buffer: Mientras see lee en un buffer se puede escribir en otro

## Memoria secundaria

Más lento pero no volatil

- Discos magneticos: Discos divididos en tracks que son leidos por un cabezal. Cada track esta dividido en sectores del disco. Un cilindro es un conjunto de tracks

Latencia rotacional: Tiempo hasta que sector este abajo del brazo
Seek time: TIempo para mover el brazo a cilindro buscado

- Discos de estado solido: Sin seektime ni latenica rotaciona

## Sistemas de disco

- Formateo Bajo nivel: Crea estructura de sectores, es ejecutado en fabrica

- Formateo logico: Estructuras para sistema archivos de una particion

- Particionamiento: Agrupación de sectores que son tratados como discos separados

Tabla de partciiones accesible en MBR

Sistemas operativos modernos usan GPT

## Scheduling de accesos

El sistema operativo solicita datos del disco especificando. Estos van en cola al disco

La latencia se mite como el desplazamiento que tiene que hacer el brazo

- FCFS: Primero que llega, primero que se responde
- Shortest seek time first: Encuentra el más cercano

No es optimo por posible inanición de sectores alejados

- Scan: Se mueve de derecha a izquierda la aguja
- C-Scan: Busca hacia un lado y luego vuelve al inicio para repetir
- F-Scan: Ignora solicitudes mientras lee

- Look y C-Look: Como SCAN pero no se mueve hasta el extremo si no es necesario

## Archivos

Un archivo es una agrupación de bytes

### Caracterización

- Nombre (low level, inode number)
- Nombre (simbolico)
- Tipo
- Ubicación
- Tamaño
- Proteccion
- Fecha
- Usuario propietario
- Metadata

## API sistemas de archivos

### Archivos

Creacion: Open
Lectura/Escritura: read, write
Lectura/Escritura(no secuencial): lseek
Escritura inmediata: fsync
Cambio de nombre: mv
Estado: stat
Eliminacion: rm

### Directorios

Creacion: mkdir
Lectura: opendir, readdir, closedir
Eliminación: rmdir

## Links

Permite que un archivo permita a dos directorios

- Symbolic link: link a nombre de archivo (ln -s)
- Hard link: Crea otro archivo con el mismo inode (ln)

## Implementación

- MBR: Sector 0 Indica partition table
  - Boot Control Block: Uno por cada particion, permite cargar el sistema operativo

- Volume control block
  - Datos de formato del sistema de archivo

### VFS
Es una interfaz comun para disitntos sistemas de archivo

Cualquier sistema de archivo que implemente VFS puede ser incorporado al sistema

- Tabla de archivo abiertos: mantiene file descriptors
- v-node: representa el punto de acceso al archivo y apunto a funciones del sistema de archivos que ejecutan llamadas

- FILE* = file-contro block

- Mount table: Tabla de archivos montados

### Asignación de bloques
Cada archivo requiere utilizar bloques del disco y los bloques deben estar asignada de manera eficiente

- Asignación contigua: Cada archivo tiene un start y un largo. Requiere defragmentacion
 
- Asignación enlazada: Cada bloque indica cual es el bloque siguiente. Se guarda el inicio y el final

- Asignación enlazada FAT: La tabla FAT guarda los enlaces entre bloques del archivo

- Asignación indexada: Archivos contienen un index block(gasta un bloque), el cual contiene los bloques de cada archivo

- Asignación multinivel: Cada entrada de un bloque indice apunta a otro bloque indice

### Espacio libres
- BITMAP: bit que representa cada espacio libre, rapido pero no escala bien
- Lista enlazada: punteros a bloques libres

# Redes
El objetivo es que un proceso se pueda comunicar con otro

Cosas a tener encuenta
- Donde se encuentra el proceso
- Por donde se va a hacer la comunicación
- Como se envia la información
- A quien se le transmite el mensaje
- Como encontrar el destinatario
- Existe un proceso que reciba el mensaje

## Redes de computadores
La gran red: el internet

El internet es una interconexion de redes

- Hosts: emiten y reciben mensajes
- Enlaces: transmiten unidades de información
- Dispositivos de intercambio: Routers y switches
- Puntos de acceso: model, access point, cell tower
- Proveedores de acceso: ISP

Se utilizas paquetes para transimir informacion y protocolos para intecambiarlos
## Protocolos criticos
- TCP: Transport Control Protocol
- IP: Internet Protocol

Para que todos sigan el mismo protocolo se obedecen estandares

## Hardware de red
### Comunicacion end-to-end
Existen
- Servidores: Hosts que proveen servicios y suelen vivir en data centers

- Clientes: Dispotivos livianos que solicitan servicios

## Hardware
### DSL (Digital Suscriber Line)
DSL Modem utiliza linea telefonica tradicional, se convierte señal digital en análoga

Se usa ancho de banda que no coidice con frecuencias tradicionales asi que se puede usar el telefono e internet al mismo tiempo

DSLAM recibe la señal analoga y separa la correspondiente a internet y a telefono

Existen diferentes frecuencias de transmicion, mientras mas alta más rapida

El estandar es ADSL y es asimetrico

### Cable Coaxial
Utiliza infraestructura de TV por cable

Mezcla enlaces de fibra y coxial

Es asimetrico

Medio broacast compartido, un unico cable para cada casa por lo que el ancho de banda se ve disminuido por cada usuario

### Fibra Optica (FTTH)
Cables con altisima transimision

Un modem recibe señal y lo separa para cada destinatario

### Dial-UP
Utiliza linea telefonica

### Satelite
Es muy versatil pero suceptible a interferencia y latencia

### Ethernet
Estandar de conexion por pares trenzados de cobre 

Implementan LAN


### WI-FI
Estandar de conexion inalambrico de corto alcance

Implementan WLAN


### WIDE AREA: 3G, 4G, LTE
Estandar de conexion inalambrica de corto alcance

Implementan WLAN

## Medio fisico
Permitir transmitir bits a traves de un medio fisico

Bits se transmiten como ondas

- Medios guiados: Solidos, cables
- No guiados: broacast

### UTP
Pares tranzados de cobre que pueden recorrer varios kilometros sin atenuarse

Se usan en telefonia, ethernet y LAN

### Coaxial
Par concentrico (No trenzado)

Alta resistencial ruido externo
Ancho de banda depende de la distancia

Es un medio compartido

### Fibra optica
Cables de fibra de vidrio que usan pulsos de luz para transmitir

Baja atenuacion

- Monomodo: un haz de luz
- Multimodo: multiples haces a distintos angulos

### Medios inhalambricos
La señal se debilita al propagarse y se causan interferencias con otras señales

Propagación multicamino

#### 802.11, Wireless Lan
Modo infraestructura: Medio cableado con varios access points

Modo Ad-hoc: Red WIFI entre pares de miembros

Trabaja en un espectro de 2.4GHz a 2.485GHz dividido en 11 canales

Administrador AP elige un canal, host escucha y detecta beacons enviados por AP

Beacons contienen SSID y MAC de AP

## Funcionamiento de Red
### Packet Switching
- Router: Almacena paquetes, examina y determina punto de reenvio con tabla de reenvio (store and forward)

Puede haber congestion en un router si es que existe una diferencia entre las velocidades de los enlaces

Si existen varios routers entonces es más facil de manejar debido a que existen caminos alternativos

### Circuit Swiching (telefonica)
Antes de establecer un intercambio de datos se busca un camino entre routers

Este camino y su banda ancha se reservan y se hace el intercambio

Garantiza banda ancha, pero si se llena no se pueden generar rutas nuevas

#### Multiplexion en CS
- FDM: Una banda de frecuencia (ancho de banda) para cada transmision

- TDM: Un turno para todo el ancho de banda

## Funcionamiento de red internet

Routers en cada domicilio que estan conectados a un router que provee internet

Redes domiciliarias se conectan a ISP que se conectan a IXP que hablan con ISP de alto nivel

## Conceptos de software

Los sistemas de software para redes siguen protocolos

Cada capa se comunica entre si usando protocolos

### Modelos de capas

Cada capa usa un protocolo especifico que se encarga de resolver los problemas de cada capa

Cada capa entrega servicios a una capa superior y solicita servicios a una capa inferior

#### Modelo OSI

- Aplicacion
- Presentacion
- Sesion
- Transporte
- Red
- Enlace
- Fisica

TCP/IP es una implementacion de este protocolo

#### TCP/IP

- Aplicacion (Comunicacion entre procesos, HTTP, SMTP, FTP,...) + autenticación y encriptacion (capas de presentacion y sesion combinadas)
- Transporte (TCP (connection oriented), UDP (conection less), trasmite mensajes)
- Red (Transmir paquetes, hacer routing. IP, ICMP, IGMP)
- Enlace (Enlace entre tarjetas de red con switches y hubs. Ethernet, WIFI, MAC. Envio de frames)
- Fisica (Transmiten bits, Tables coaxiales, fibra optica y medio inalambrico)

Cada capa añade o quita un header al mensaje

## Capa de Aplicacion

### Arquitectura cliente-servidor

Cada cliente se comunica con un servidor, el cual posee direccion fija y conocida

Un servidor puede recibir multiples solicitudes

### Peer-to-Peer

Evita centralizacion haciendo comunicacion directas entre pares

### Sockets

Elemento de software que interactua en la red

Es una puerta de entrada y salida del sistema

Utiliza servicios de transporte

Solo existen hosts de origen y destino

Definir un socket requiere
- Tipo de servicio de transporte (TCP/UDP)
- Direccion en extremo opuesto (IP + Puerto)

### Conexion UDP
- Se crea socket de cliente y servidor
- Para enviar mensajes se debe especificar numero de socket,tamaño de buffer, bytes para enviar, flags y struct con datos destinatario y tamaño de ip 
- Para recibirlo se debe especificar numero de socket, buffer con datos, largo de bytes, flags, struct con datos de origen y addrlen

### Conexion TCP

Para establecer la conexion se requiere un handshake para que ambos sockets sepan que van a recibir o enviar mensajes en un cierto puerto

No se necesita especificar el receptor cada vez, porque se crea una conexion unica con cada cliente

- Cliente solicita conexion a puerto e ip
- Servidor acepta conexión y crea socket para cliente en especifico

### HTTP

Cliente solicita y recibe archivos

Servidor recibe solicitudes y envia archivos

Las conexiones HTTP son stateless excepto por las cookies que son guardadas en el cliente

EL contenido estatico puede ser almacenado en un server intermedio llamado web proxy

Un CDN (conjunto de proxys) guardas muchos datos estaticos que sirven para responder rapido

### FTP
Requiere inicio de sesion y se utiliza para la transferencia de archivos entre sistema local y remoto

### SMTP, POP, IMAP

#### Envio correo

SMTP envia en puerto 25, se encarga de enviar un correo a una casilla de un servidor

#### Lectura correo

POP3 no guarda estado y lee correo en puerto 120 descargando todos los correos cada vez que se quieren leer

IMAP guarda un estado, permite creacion de directorios en el servidor

### DNS
Protocolo que traduce un dominio a una IP

Existe una base de datos distribuida y jerarquica de dominios

- Consulta iterativa: Le pregunto a cada nodo lo mismo
- Consulta recursiva: Cada nodo delega responsabilidad

## Capa de transporte
Objetivo es que mensaje llegue de un host a otro

- Emisor: Recibe un mensaje de capa de aplicacion, lo divide en segmentos y solicida a capa de red que envie los segmentos el receptor

- Receptor: Recibe segmentos, esamble y entrega a capa de alplicacion

TCP: Entrega confiable y orderdenada, establece conexion

UDP: No confiable y desordenada, mejor esfuerzo. Tiene un checksum que si no calza se ignora el mensaje. Es conection-less. Solo 8 bytes de overhead

UDP delega todo a la capa de aplicacion

### Segmentos
El mensaje se divide en segmentos y cada segmento tiene un header con el puerto de inicio y destino

El puerto permite identificar el proceso y multiplexar mensajes que van a un destinatario

### Mutiplexion
Un socket se crea con un puerto y se pueden crear muchos sockets

### Demutiplexion sin conexion
Segmento UDP tiene un IP de destino y un puerto, el receptor observa el puerto de destino y si contesta contesta al puerto del cliente

### Demutiplexion con conexion
Un segmento TCP tiene un IP origen, puerto origen, ip destino y puerto destino

Cada socket habla con un proceso especifico

### UDP
- Segmentos pueden perderse
- Pueden llegar en distinto orden
- NO establece coneixon previa
- No se mantienen estado de conexion

Delega todo el trabajo a la capa de aplicacion y es rapido

Existen solo 8 bytes de overhead
- Puerto de inicio
- Puerto de destino
- Longitud mensaje
- Checksum (para saber si pasa a la capa superior o se ignora)

### Reliable Data Transfer
Consiste en proveer una transferencia confiable sobre un servicio desconfiable

#### V1
El sender envia, el reciever recibe

- Paquetes pueden tener error
#### V2
Se pueden enviar checksums en el header

- Protocolo ARQ (Automatic Repeat reQuest)
- paquetes feedback, ACK (acknoledgement) y NAK (not ack)
- Dos estados, esperando paquete y esperando ACK

Sender envia un paquete y se queda esperando un ACK

Reciever revisa paquete, si esta corrupto se envia ACK, sino NAK

Si el sender recibe un NAK se vuelve a enviar paquete

- Puede suceder que ACK llegue corrupto

#### V2.1
- Se añade un bit que va junto al ACK para saber a cual paquete corresponde

El reciever espera un ACK para un bit de paquete en especifico

#### V2.2
- Se eliminan NAK adjunta sequence number al ACK

Se pueden perder paquetes y no se sabe por que

#### V3
Se pone un timer en el sender y espera a que llegue le ACK con el sequence number adecuado

### Mejoras
Se enviar varios paquetes simultaneos en modo pipelined

Se ponen numeros de secuencia incrementales

Se utilizan buffers en receptor y emisor

Go-Back-N o Selective Repeat

### Go-Back-N
Existe una ventana de paquetes que el sender va enviado

A medida que se reciben los ACK para los primeros paquetes, esta ventana se corre a la derecha y se comienzan a enviar los siguientes

Despues de un timeout se vuelven a enviar los de la ventane en orden

### Selective Repeat
El receptor tambien tiene una ventana de paquetes que puede recibir fuera de orden y se van corriedno a la derecha

### TCP
Posee 20 bytes

- Puerto origen
- Puerto destino
- Numero de secuencia
- Acknoledgement number
- Largo de header
- Flags
- Recieve window
- Internet checksum
- Urgent data pointer

Los numeros de secuencia van subiendo con la cantidad de bytes enviados

El ACK es el secuence number del siguiente bytes que se espera

El ACK se envia junto a al respuesta, PIGGYBACKED ACK

#### Handshake
1. Se envia segmento SYN al servidor (numeros de secuencia inicial de cliente)
2. Servidor recibe SYN y responde con SYN ACK (server asigna buffer y establece numero de secuencia inicial del servidor)
3. Cliente recibe SYN ACK y responde con ACK (puede contener datos)

Termino
1. Cliente envia FIN
2. Server responde ACK y envia FIN
3. Cliente responde FIN


## Capa de Red
Se encarga de que un mensaje encuentre un servidor

Transmite paquetes de un emisor a un receptor

- Direccionamiento (decision local de un router): Determinar ubicacion de nodo destino

- Enrutamiento (distribuido entre routers): Coordinar routers para que el paquete llegue

Cada router tiene un tabla de foward que apunta a una salida dependiendo del header

El header son 20 bytes (si es que no hay options) y contienen

- ip destino
- ip origen
- tipo de servicio 
- upper layer protocol (tcp/udp)
- identificador de paquete ip
- flags
- offset fragmentacion
- checksum
- ttl (cantidad de saltos antes de morir)


### Modos de conexion
#### Virtual Circuit Network (telefonos)
Antes de establecer conexion, construye un camino virtual y se establece un ancho de banda en cada router para poder garantizarlo

- Es conection oritented
- Un router esta saturado, se bloquean nuevos caminos.
- Es poco flexible

#### Datagram networks
Cada paquete se encapsula y se pone un header

Cada router decide donde envia cada paquete

Los routers no saben de caminos ni estados, es conectionless

No garantiza ancho de banda

#### Fowarding table
Un router debe tener una respuesta de Link Interface para cada direccion posible por lo que se asigna un link a cada rango de direcciones

### Fragmentación
Cada enlace tiene un MTU (maximum transfer unit)

Al encontrar el enlace con un menor MTU (que el tamaño del datagrama) el router fragmenta el datagrama y lo envia en fragmentos con el mismo identificador. El otro router lo ensambla

header:

- flag que dice si es o no el fragmento
- offset para saber en que parte del buffer va
- numero identificador

### Direccion IP
- Direcciones de 32 bit en grupos de 8 bits (1 byte): IPv4

- Subredes: a.b.c.d/x
La x corresponde a cuantos de los primeros bits corresponden a la subred

El restante corresponde al numero de host

Una mascara es una direccion los primeros x bits en 1 y el resto 0

Para saber a que red pertenezco hago un and con la mascara

Existe una direccion de broacast que reenvia un mensaje a todos miembros de la subred

Cada subconjunto al que apunta un router es una subred

- Nombre de la subred: bits de host en 0
- Broacast: bits del host en 1
- Tamano de subred: 2^{bits para host} - 2

### Direccionamiento red
Cada router posee una IP para cada salida

Cada grupo de host conectados a un router es una subred

Switches no tienen direccion IP

### Subredes y direcciones especiales
#### Direcciones
- 0.0.0.0: host actual
- 127.0.0.1: loopback

#### Subredes
- Red local: 0.0.0.0/8
- Red loopback: 127.0.0.0/8
- Redes privadas: 10.0.0.0/8, 172.16.0.0/12, 192.168.0.1/16



### DHCP
Se encarga de asignar una ip a un dispositivo nuevo conectado

1. Cliente envia un discorver message a broacast (DHCP Discover)
2. Server envia DHCP offer message a broacast con la nueva ip y el id (DHCP offer message)
3. Cliente envia un request con la ip a broadcast (DHCP request)
4. Servidor envia ACK a cliente (DHCP ACK)

### NAT
Permite multiplexar direcciones IP en redes privadas

Se crea una tabla donde la IP del router en un cierto puerto apunta a una IP dentro de LAN

Cambiar la IP del header y el puerto de la capa de tranporte (viola el principio de aislamientos de capas)

UPnp: Protocolo que permite que los servidores se registren dentro de un router NAT para vincurlase a un cierto puerto

### ICMP
Usado para el diagnostico y control en la capa de red

traceroutes manda pings con ttl cada vez mayores para conocer el camino

### IPV6
Header de 40 bytes
- Direccion IPV6 de origen
- Direccion IPV6 de destino
- Hop Limit

### Algoritmos de Routing
- Centralizados (informacion completa de la red)
- Decentralizados (calculo iterativo y distribuido)

#### Link-State Routing
Todos los nodos le comunican a sus vecinos todo lo que conocen de la red

Cada uno calcula todas las rutas mas cortas

#### Distance vector
Se utiliza ecuacion de bellman-ford

Cada nodo mantiene una mejor ruta a sus vecinos
Cada nodo comparte su tabla

#### Routing de grana escala
Routers pertenecientes al mismo dominio adminsitrativa forman un sistema autonomo

Centro de un sistema se utilizan algoritmos Link-State

Entre distintos sistamas se utilizan Distance Vectors

Routers de salida son gateway routers

El protocolo mas comun Inter-AS es BGP
Anuncia a otros AS sobre la existencia

## Capa de Enlace
Objetivo es trasmitir un frame a traves de un enlace
### Frames, errores y control de flujo
- Transformar paquetes a frames y transmitirlos a traves de un enlace
- Determinar quien puede usar MAC
- Transferencia confiable en medios con errores
- Deteccion y correccion de errores
- Control de flujo (saturacion de frames)

Cada capa de enlace es independiente

Se implementa en una Network Interface Card

### Deteccion de errores
Se agregan bits de deteccion de errores unicos para los bits del mensaje

Si cuando llega el frame, coinciden es correcto

- Simple Parity check (XOR de todos los bits 1 si par sino 0)
- Double Parity Check (tengo dos bits de paridad, calculo columnas y filas)
- Checksum (Secuencias de bits unicos)
    - CRC: Se define un patron de G bits de largo r+1, se le añaden r ceros a la secuencia de datos y se divde. Si la division es la misma a la llegada entonces esta correcto

### MAC
Formas de controlar acceso al medio:
- Particion del canal: tiempo, frecuencia, codigo
- Acceso aleatorio
- Turnos

#### Time-Division Multiple Access
Cada emisor tiene tiempos fijos, le asigno un slot de tiempo a cada emisor y van circulando

- Los slots no utilizados se pierden

#### Frequency-Division Mutiple Access
Se divide por frecuencia, cada estacion tiene una banda fija asignada

#### Code-Division Mutiple Acess
Permite que varios usuarios usen la misma frecuencia

Cada cliente tiene un codigo ortogonal, que se utiliza para hacer un XOR con lo recibido y se recupera lo enviado

#### Acceso Aleatorio
Los protocolos optimistas se enfrentan a una colision esperando un tiempo y volviendo a intentar

- Aloha Particionado: Todos se poden de acuerdo en tamaños de frames y cada vez que alguien transmite se hace en unidades de una cantidad de tiempo 

- Aloha: Equivalente al particionado pero sin sincronizar al momento de comenzar a transmitir

- CSMA: Antes de transmitir reviso si hay alguien hablando, retrasos en la propagacion causan colisiones
    - CSMA/CD: Detectan las colisiones y se aborta cuando se detecta colision (jam signal, se usa en LAN)
    - CSMA/CA: Si se detecta colision se espera un tiempo aleatorio y si se logra se envia un ACK

- Turnos: Un maestro que hace polling a cada slave

- Token: Nodos se pasan un token y lo necesita para transmitir

### Hardware capa de enlace
Un switch extende una subred provista por un router

Para encontrar un destinatario dentro de una LAN se utiliza la dirección MAC

La direccion MAC es un identificador unico modificable por software

Un switch tiene una MAC (e IP) por cada puerta

Los switches no tienen direccion MAC, pero cada interfaz de red SI

#### Tabla ARP (IP, MAC, TLL)
Es un protocolo de capa de enlace que usa valores de capa de red

Routers y hosts usan procolo ARP, los switches solo guardan una dirección MAC en cada puerto

TTL es el tiempo que sera recordada esa entrada

Switches guardan (puerta, TTL)

Para añadir las direcciones MAC a la tabla ARP, se envia una ARP Query con la IP del host que me interesa y MAC Broacast

El que tenga la IP responde con su MAC

Si se quiere enviar una ARP Query a un dispotivo fuera de la red, se debe usar MAC del router

### Frame Ethernet
Se añade un header de 46 a 1500 bytes:
- Destination MAC
- Source MAC
- Type (IP, Novell, AppleTalk, ARP)
- CRC (descarta frames erroneos)

### Switches
Un switch recibe y transmite en a un enlace basado en MAC

Filtra para saber si enviar o botar

Si no conoce MAC, envia broacast

La tabla del switch aprende automaticamente. Por cada frame recibido guarda

- Source MAC
- enlace de recepcion
- tiempo de recepcion

Despues de un cierto tiempo elimina entrada

#### Protocolo STP
Se encarga de evitar problemas con ciclos de switches asignando un arbol a toda la arquitectura

Cada switch ve su arbol

### VLAN

Permite crear LAN virtuales y se hace modificando paquete Ethernet

Se expande el campo de Type para añadir el numero de VLAN y avisar que es de ese tipo
