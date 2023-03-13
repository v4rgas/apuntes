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


