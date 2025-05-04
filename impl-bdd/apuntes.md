# Modelo Relacional

Una relacion esta compuesta por

1. Un esquema: name(att: type, att: type, ...)
2. Una instancia: 


## Propiedades de una relacion

Un atibuto puede ser cualquier tipo de primitivo, como int, float, string, etc.

donde (v1, v2, ..., vn) es una tupla de valores de la relacion.

El orden de las filas no importa.

# Base de datos relacional

1. Un conjunto finito de relaciones, cada uno con un nombre unico.
2. Un esquema relacional S es una BD D es el conjunto de esquemas de las relaciones R1, R2, ..., Rn.

$$schema(D) = {R1, R2, ..., Rn}$$

## Restricciones de integridad

Una condicion $\phi$ que restringe los datos que pueden ser almacenados en una BD.

## Extraccion de datos basado en consultas

Una consulta es una funcion $f: dom(S) \rightarrow D$

- S es un esquema de la BD
- D es un dominio cualquiera

Un lenguaje de consultas es un conjunto de expresiones sintacticas que representan consultas.
- SQL
- Algebra relacional

# SQL

Es el standard para BD relacionales.

# Algebra relacional

- Lenguaje de consultas procedural
- Basado en operadores relacionales (algebra)

## Utilidad
- Facil de componer
- Facil de optimizar

## Operadores

- Seleccion $\sigma_{\phi}(R)$: Selecciona las filas de R que cumplen con la condicion $\phi$
- Proyección: $\pi_{A1, A2, ..., An}(R)$ Selecciona las columnas A1, A2, ..., An de R
- Joins:
    - Producto cruz: $R \times S$
    - $\bowtie_{\phi}(R, S)$: $\phi_{\omega}(R \times S)$

## Algebra relacional extendida
- Rename: $\rho_{A1/B1, A2/B2, ..., An/Bn}(R)$
- Eliminar duplicados: $\delta(R)$
- Group by: $\gamma_{G,A}(R)$, donde G es el conjunto de atributos por los que se agrupa y A es el conjunto de atributos que se agregan.

- Sort: $\tau_{A1, A2, ..., An}(R)$

# Almacenamiento de datos
## Niveles de almacenamiento

### No volatil
- Cinta magnetica
- HDD
- SSD

### Volatil
- RAM
- Cache
- CPU

## Unidades minimas de memoria
- Sector: La unidad mínima de almacenamiento en un disco.
- Bloque: Conjunto de sectores que el sistema operativo gestiona como una sola unidad.
- Página: Unidad mínima de memoria gestionada en la memoria virtual.


### Demora en la busqueda de datos

$$TiempoTotal = SeekTime + RotationalDelay + TransferTime$$


## Localidad de los datos

Si las tuplas t y t' son parte de la misma relaciones entonces almacenar t:

- en el mismo bloque que t'
- en el mismo track que t'
- en el mismo cilindro que t'
- en el cilindro adyacente a t'


## Pre fetch

Es la estrategia de traer datos antes de que sean necesarios.

Para una que requiere un bloque de datos, se traen los bloques adyacentes.

## Planificacion del disco

Dado una secuencia de peticiones, se busca minimizar el tiempo TiempoTotal

### Algoritmo del ascensor

- Disco es como un ascensor, donde los pisos son los cilindros.
- Brazo del disco mantiene su dirección si quedan peticiones en la misma dirección.


## Multipes discos

Distribuir tuplas en múltiples discos para así tener acceso concurrente a los datos.

## Comparacion de optimizaciones

- OLAP: On-line Analytical Processing, consultas que no requieren respuesta instantanea, solo un proceso usando el disco
- OLTP: On-line Transaction Processing, consultas que requieren respuesta instantanea, como transacciones de tarjetas de crédito.

OLAP: Localidad de los datos, prefetch

OLTP: Planificación del disco, múltiples discos


## Nuevas tecnologias

- SSD: Solid State Drive, no tiene partes moviles, por lo que es más rápido. Es más caro que un HDD.

# Buffer manager

Es un mediador entre el disco y la memoria principal

Cuenta con una cantidad restringida de memoria ram.

## Frames

Cada frame tiene dos variables:

1. #pin = cantidad de procesos que estan usando la pagina
2. Dirty = Si es necesario guardar la pagina en disco

## Funciones

- pin(pageno): Solicita la pagina pageno
    - Si la pagina no esta en memoria, se trae del disco
        - Se selecciona un frame vacio
        - Se trae la pagina pageno a memoria y se guarda en el frame
        - Se actualiza FRAME.#pin = 1, FRAME.dirty = 0
    - Si la pagina esta en memoria, se actualiza FRAME.#pin += 1
    - Buffer manager retorna la direccion de memoria de la pagina

- unpin(pageno, dirty)
    - Se solicita liberacion de la pagina pageno
    - Se decrementa FRAME.#pin
    - Se actualiza FRAME.dirty = dirty

### Requisitos
1. Cada proceso debe mantener las funcione pin y unpin correctamente anidadas.
2. Cada proceso debe liberar todas las paginas que ha solicitado lo antes posible.


## Politicas de reemplazo

La efectividad de un buffer manager depende de la politica de reemplazo.

- FIFO: Reemplaza la pagina que lleva más tiempo en memoria.
- LRU: Reemplaza la pagina que menos se ha usado.
- CLOCK: Reemplazan las paginas de manera circular si no han sido usadas.
- Random: Reemplaza una pagina al azar.

### Tecnicas adicionales

- Prefetch: Trae las paginas que se van a usar en el futuro.
- Page fixing: Fija una pagina en memoria para que no sea reemplazada.
- Buffer particionado: Divide la memoria en partes para diferentes procesos.

### Ventajas de usar un buffer manager propio
- Mayor conocimiento del patron de acceso a los datos.
- Politicas de reemplazo personalizadas.
- Pin y unpin personalizados.
- Acceso fino por parte del administrador de transacciones.

# Indices
## Interfaz de acceso
1. Create or destroy
2. Insert
3. Delete
4. Get
5. Scan

## Que es un indice
Un indice es una estructura de datos que permite lookup rapido y modificaciones a data entries por medio de un search key.

Metodo de acceso que optimiza el acceso a los datos para una consulta.

1. Un indice optimiza un subconjunto de consutlas
2. Tambien puede hacer otras consultas mas costosas
3. Es posible sacrificar tiempo por espacio



### Que nos gustaria optimizar (lookups)
1. Busqueda por valor (value query)
2. Busqueda por rango (range query)
3. Busqueda por match (query by match)

El parametro mas importante de optimizar es el tiempo de acceso

## Componentes de un indice
1. Search key = parametro de busqueda, subconjunto de atributos en una relacion
2. Index entry = puntero o valor de la tupla
3. Data entry = tupla

## Clustered vs non clustered
- Clustered: Indice para el cual el orden de las data entries es el mismo que el orden de los records en el disco.
- Non clustered: No clustered.

Son usualmente conocidos como indice primario y secundario respectivamente.

## Densos y dispersos

- Denso: Un index entry por cada record de la relacion.
- Disperso: No todos los records tienen un index entry.

## Tipos de indices
1. Basado en arboles: ISAM, B+Tree
2. Basado en hashing: Extendable hashing

# Indices basados en arboles
