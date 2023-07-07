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
Permite que un proceso se comunique con otro

- Depende donde se encunetra el otro proceso
- Que medio se utiliza para el envio
- Como se envia la informacion por el medio
- A quien se le transmite

## Red de paquetes
La unidad de minima de informacion en una red es un paquete

Las reglas para intercambiar paquetes son un protocolo
- TCP: Transport Control Protocol
- IP: Internet Protocol

Para asegurarse que se se siga el mismo protocolo se deben acordar estandares (IETF, RFC)

## Hardware de red
### Comunicación end-to-end
Algunos hosts son clientes otros servidores

- Clientes: Dispositivos livianos que solicitan servicios
- Servidores: Dispositivos poderosos que proveen servicios (generalmente en data centers)

Los hosts pueden acceder a traves de distintos medios
- DSL
- Cable
- FTTH
- Ethernet
- Wifi
- etc...

### DSL (Digital Subscriber Line)
Utiliza una linea telefonica tradicional conviertiendo una señal digital a una análoga

Usa una frecuencia distinta a la linea telefonica tradicional

Distintos estandares permiten distintas velocidades 

### Cable
Utiliza infraestructura de TV por cable y un modem

El medio es compartido asi que la velocidad baja

### FTTH
Cables de fibra optica de altisima velocidad, convierte señal optica-electrica

### Dial-Up, Satelite
Enlace a traves de linea telefonica

### Ethernet
Estandar de conexion por pares trenzados de cobre e implementan LAN (Local Area Network)

### WIFI
Implementa Wireless LAN, WLAN

### Wide Area (3G, 4G, LTE)

## Medios fisicos
Permite que un proceso se comunique con otro

Los procesos trnasmiten bits como ondas
- Medios guiados: Solidos, pares trenzados de cobre (UTP), cable coaxial (concentrico no centrado), fibra optica (usa luz por lo que no hay interferencia electromagnetica)
- Medios no guiados: Inahalmbricos, enlace satelital

Las señales no guiadas se debilitan al propagarse y sufren de propagacion multicamino

### WIFI (IEE 802.11, WLAN)
- Infrastructure Mode: Varios access point conectados a una misma red
- Ad-Hoc: Red wifi entre pares de miembros

Las redes wifi funcionan entre 2.4 y 2.485 GHz dividos en 11 canales

## Funciones de redes
La transmicion de un enlace se mide en bits/sec
### Packet switching
Los dispositivos intercambian paquetes

Un router almacena paquetes, los examina y determina el punto de reenvio. Para esto utiliza tablas de reenvio (fowarding tables)

Esto se denomina Store-And-Forward packet switching

Un router puede sufrir congestion, pero si hay N enlaces iguales en la ruta el tiempo de transimicon de origen a destino es $N \times \frac{L}{R}$

### Circuit Switching
Enfoque usado por comuniacion telefonica, se enfoca en garantizar un cierto ancho de banda

Se encarga de buscar un camino que garantice estabilidad

#### Multiplexion
Al usar un medio compartido se deben mantener serparadas las comunicaciones

- FDM: Banda de frecuencia para cada transmisión
- TDM: Division por tiempo entre emisores

