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

Ocurre cuando un proceso termina y el padre no hace wait()

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


