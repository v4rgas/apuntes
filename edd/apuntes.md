# Introducción
## Estructuras de datos y algoritmos
Estudio de algoritmos y su implementación eficiente mediante estructuras de datos

## Algoritmos y notación

## Memoria Ram
Cada variable se guarda en uno de estos segmentos de la RAM:
- stack: cosas de tamaño fijo
- heap: cosas de tamaño variable

### bit
Es una unidad indivisible de información que puede valer 0 o 1. También es una celda física indivisible de almacenamiento en un computador

La RAM puede ser imaginada como una matriz de bits

Cada variable tiene una posición, un tamaño y un valor

## Estructuras básicas
### Arreglo
Secuencia de celdas del mismo tamaño que tiene largo fijo y guardan valores del mismo tipo

Se almacenan de manera contigua en la memoria

### Listas ligadas
Es una secuencia de largo variable donde cada celda tiene el mismo tamaño y apunta hacia otra celda

Se almacenan de forma aleatoria en la memoria

## Ordenación y Selectionsort
Si se quiere encontrar un $a \in A$ la forma mas directa seria comparar cada uno lo que tendera complejidad O(n)

### Secuencias ordenadas
Secuencia ordenada es tal que $a_1\leq a_2 \leq ...\leq a_n$

Si L es el input y L' corresponde al output como lista ordenada

### Selection Sort
1. L' parte vacía
2. Encontrar el menor valor de L
3. Borrar lo encontrado
4. Escribirlo en L'
5. Si quedan valores en L, volver a línea 2
6. return L'

#### Finitud
Por cada iteración se saca un elemento, por lo que si L es finito, el algoritmo termina
#### Correctitud
P(n): Los primeros n valores borrados de L son los menos de L y ordenados en L'

##### Caso base

P(1) corresponde al estado de las listas liegos de borrar el menor elemento de L

##### Hipotesis inductiva
Supongamos que P(n) 

##### Tesis
Para P(n+1) sabemos que P(n) ya esta ordenado y por lo tanto el elemento restante ya es el mas grande de la lista original

P(n+1) también esta ordenado

#### Complejidad
Dado que hay que ordenar los items uno por uno, hay que iterar al menos n veces
- Revisar elemento: $\mathcal{O}(n)$
- La busqueda de menor elemento fuera: $\mathcal{O}(n)$

Total
$$\mathcal{O}(n^2)$$
Todo esto es sin contar el costo de insertar y quitar datos de L en L'

##### Complejidad de memoria
1
