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

P(1) corresponde al estado de las listas luego de borrar el menor elemento de L

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

### Insertion Sort

1. Definir secuencia B vacio
2. Tomo primer x de A
3. Saco x de A

#### Finitud

Cada iteración borra un elemento, por lo que cuando me quede sin elementos el algoritmo se detendrá

#### Correctitud

P(n) implica que luego de la enesima iteracion B (el array al que inserto) se encuentra ordenada

##### Caso Base

P(1) esta trivialmente ordenado si tenemos un elemento

##### HI

Luego de la enesima iteración, B esta ordenada

#### Tesis

##### Complejidad

Insertar requiere dos pasos

1. Busqueda posicion del dato
2. Insercion: SI es busqueda binaria es $\mathcal{O}(log(n))$

## Merge Sort

### Merge

Si tenemos dos secuencias ordenadas

Merge(A,B):

1. Iniciamos C vacía
2. sean a y b los primeros elementos de A y B, extraemos el menor

# Estructuras de datos

## Arboles binarios de busqueda (ABB)

- Cada nodo tiene una llave y un valor
- Solo puede tener hasta dos hijos
- El subarbol con llaves menores debe estar a la izquierda
- El subarbol con llaves mayores debe estar a la derecha

## Arbol binario

- Cada nodo tiene <= 1 padre
- Cada nodo tiene a lo mas 2 hijos
- Una hoja es un nodo sin hijos
- Una raiz es un nodo sin padre

La altura de un arbol balanceado es O(log(n)) donde n es la cantidad de nodos

## Arboles AVL

Un ABB es AVL-Balanceado si:

1. Las alturas de los hijos diferen a los mas por 1
2. Cada hijo es AVL-Balanceado

Solucion de desbalanceo por insercion:

- Izquierda de la izquierda: Rotacion izquierda
- Derecha de la derecha: Rotacion derecha
- Derecha de la izquierda: Rotacion izquierda derecha
- Izquierda de la derecha: Rotacion derecha izquierda

La insercion y balanceo tienen complejidad O(log(n)) donde n es la cantidad de nodos

## Arboles de busqueda 2-3

Almacena una llave y un valor

1. Un nodo puede ser un 2 nodo (una llave) o 3 nodo (2 llaves distintas y ordenadas)
2. Un nodo puede tener 2 (si es un 2 nodo) o 3 hijos (si es 3 nodo)

Para insertar se hace de las misma manera que en un arbol binario excepto que cuando se convierte en un 4 nodo se hace split poniendo el del medio en el anterior

Insercion es O(log(n)) pero pueden haber muchos splits consecutivos

## Arboles rojo-negro

Es un arbol ABB en el cual

1. Cada nodo es rojo o negro
2. La raíz es negra
3. Si un nodo es rojo, sus hijos son negros
4. La cantidad de nodos negros camino a cada hoja debe ser la misma
5. Las hojas vacías son negras

Todos los arboles rojo-negros tienen un arbol 2-4 equivalente (algunos tienen un 2-3)

## Diccionarios

- Asociar un valor a una llave
- Actualizar el valor de una llave
- Obtener el valor de una llave
- A veces eliminar llave-valor

El objetivo principal es facilitar la búsqueda
El secundario es inserción/modificación eficiente de pares llave/valor

Diccionario = array + funcion hash

## Funciones de hash

Una función que permite mapear un espacio de llaves a uno mas pequeño

$$h:K \rightarrow \{ 0,...,m-1 \}$$

llamaremos valor de hash de k a K(k)

## Colisiones

### Insercion con cadenamiento

Si al usar la función hash existe una colisión, pongo el valor al principio de la lista ligada

### Direccionamiento abierto

Busco sistematicamente la siguiente celda vacia e inserto ahi. Puede ser de formar:

- Lineal
- Cuadratica
- Sumando hash

## Factor de carga

$$\lambda = \frac{n}{m}$$
n: cantidad de valores almacenados

m: cantidad de espacios totales

## Tablas de Hash

Dado una cantidad de elementos y una cantidad de llaves, una tabla de hash es una estructura que asocia llaves indexadas usando la función de hash

# Ordenacion en tiempo lineal
# Backtracking
## Constraint satisfaction problem (CSP)
Corresponde a una tripleta (X, D, C) tal que
- X: Conjunto de variables
- D: Conjunto de dominios respectivos
- C: Conjunto de restricciones

Para resolver estos problemas:
1. Fuerza bruta
- Generar todas las asignaciones de variables
- Verificar cada asignacion

2. Backtracking

## Backtracking
1. Asignamos $x_k$ cuando ya se han asignado $x_1...x_{k-1}$
2. Se verifica si la asignacion parcial resuelve el problema
3. Si no, se retracta $x_k$
