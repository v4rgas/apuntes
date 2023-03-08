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


## Componentes

### Kernel
Programa que siempre se esta ejecutando, provee acceso a CPU, memoria y dispositivos
#### Monolitico
No existe separación entre el espacio del usuario y el espacio del Kernel

Todos los servicios se ejecutan en modo kernel

Todo es un único programa extensible

#### Microkernel
Mantiene el menor tamaño posible

Desventaja es el overhead

#### Kernel híbrido
COnstruido como

### Programa del sistema
Extienden las funciones del kernel

## Estructura

