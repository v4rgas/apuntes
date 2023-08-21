# Introduccion al procesamiento paralelos de datos
## Arquitecturas y procesadores modernos
- Paralelismo de instrucciones: Varias instrucciones se corren el mismo tiempo
- Paralelismo de datos: Subdivido los datos de entrada para correrlos de forma parelelo

Para ejectuar instruciones mas rapido se puede usar

## Instruction Level Paralelism
- Out of order
- Pipelining
- Branch-prediction
- Prefetching
Permiten ocular la latencia

## FPU
En computo cientifico importa la capacidad de trabajr con floats

En una FPU tradicional
$$T(n) = n \times l \times t$$
donde n son la cantidad de resultados, l el numero de etapas y t el clock cyle time

Si se utiliza Pipelining
$$T(n) = (s+l+n-1) \times t$$
donde s es la inialización

## Peak Performance
Si cada FPU produce un resultado por ciclo
$$Peak = clockspeed \times FPUs$$

## Real Performance
Se utilizan benchamrks como
- Linpack
- HPCG

Es efectada por latencia

## CPU
Los registros son programables mientras que el resto de caché depened del hardware

A medida que se baja en la lista se tiene menos velocidad de acceso pero más velocidad

1. Registros
2. L1
3. L2 (De aqui hacia abajo son compartidos y deben manejar coherencia)
4. L3
5. Memoria principal

## Caché
El working set corresponde al conjunto de paginas usadas por un programa

Al programar se debe tomar en cuanto el funcionamiento del caché

<!-- ### Scratch -->
- Valid: Con copia coherete en memoria
- Reserved: Valida con unica copia
- Dirty: Modificado
- Invalida: Modificado en otro caché (a traves de snooping o tag directories)

### Miss
- Compulsory: Obligatoria
- Capacity: Sin capacidad
- Conflict: Intento guardar un dato en dos lugares distintos
- Invalidation: Se invalidó el caché por un cambio en sus valores

### Coherencia
Los problemas de coherencia ocurren cuando se intenta acceder a un mismo dato que fue motificado concurrentemente

## RAM
### UMA
Todos los nodos utilizan una unica memoria RAM compartida, esto puede llevar a que se sature la conexión con la RAM

#### NUMA
Cada nodo tiene una conexión directa a un pedazo de la RAM, si un nodo quiere acceder a un dato fuera de su pedazo de RAM, debe enviar un mensaje y pedirlo


