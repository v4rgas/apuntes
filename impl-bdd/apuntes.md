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

