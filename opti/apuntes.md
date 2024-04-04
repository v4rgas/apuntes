# Introducción

La optimización consiste en tomar decisiones en escenarios con recursos escasos con objetivos claros de manera inteligente

Se utiliza en disciplinas como:
- Modelacion estocastica
- Simulacion
- Comportamientos del mercado

# Propiedades Basicas
## Forma algebraica
$$min f(x),g_i(x)\leq b_i, i = 1,...,m, x \in C$$

Donde $g(x) \leq b$ representa una restriccion

$x \in R^n$ una variable 

$f(x): R^n \rightarrow R$ una funcion objetivo

## Definiciones Fundamentales
### Combinacion Convexa
$$y = a_1 x_1 + a_2 x_2 + ... + a_n x_n$$

y es una combinacion convexa si $\sum2_{i=1}^n a_i = 1, 0 \leq a \leq 1$

### Conjunto Convexo
Un conjunto $S \subseteq R^n$ es convexo si
$$\forall x,y \in S, \forall \lambda \in [ 0,1 ] \implies \lambda x + (1 - \lambda)y \in S$$

El conjunto definido por una desigualdad lineal es convexo

### Funcion Convexa
$$f(\lambda x + (1-\lambda)y) \leq \lambda f(x) + (1-\lambda)f(y), \forall x,y, \forall \lambda \in [0,1]$$

#### Estrictamente Convexa
$$f(\lambda x + (1-\lambda)y) < \lambda f(x) + (1-\lambda)f(y), \forall x \neq y, \forall \lambda \in (0,1)$$

### Conjuntos de Nivel
Dado un $f: H \rightarrow R$
$$C_k = \{x \in H | f(x) = k \}$$

## Tipos de Problemas
### La mochila
- Un excursionista tiene una mochila con carga total de b kilos
- Debe elegir entre n productos 
- Cada producto j pesa $a_j$ kilos y tiene $c_j$ energía

Se necesita maximizar la energía sin exceder el peso

#### Solucion
$$max \sum_j c_j x_j$$

$$\sum_j a_j x_j \leq b$$

$$x_j \in \{ 0,1 \}$$

### Vendedor viajero
- Tenemos n ciudades
- El vendedor debe recorrer cada ciudad
- Conozco el costo de viajar entre cada par de ciudades
    - $c_{ij}$
Se busca secuencia para pasar por ciudades todas las ciudades minimizando costo

### Solucion
$$min \sum_i \sum_j c_{ij} x_{ij}$$
$$\sum_j x_{ij} = 1$$
$$\sum_i x_{ij} = 1$$
$$\sum_{i \in C}(\sum_{j\notin C} x_{ij} + \sum_{k \notin C} x_{ki} ) \geq 2 $$

# Problemas no lineales
## Fermat-Weber
Minimizar suma de distancias a m puntos en un plano desde un punto c
## Menor circulo circunscrito
Supongamos que tenemos m puntos en un plano, se debe determinar las coordnadas del punto que minimice la mayor a distancia a los otros puntos

## Machine learning
Construye un hiperplano de tal manea que todos los puntos de un tipo esten arriba y los del otro por abajo

Maximizar la distancia de los puntos hacia el hiperplano

## Definiciones
1. Un poliedro es un conjutno definido por restricciones lineales
2. Un punto en un poliedro es un punto extremo si no puede ser escrito como combinacion convexa de dos puntos diferentes de P

### Maximo global
### Minimo local

## Teoremas
Sea P un problema convexo. Sea x un minimo local de f en D, entonces x es minimo global de f en D

# Existencia de soluciones
- Funcion obejtivo lineal
- Restricciones lineales
- f continua
- D es cerrado
- f lo puedo acotar por abajo (si minimizo)

Entonces f posee un minimo global dentro de D

# Equivalencias
## Definicion fundamental
Solución optima no es lo mismo que valor óptimo

## Equivalencia 1
max f(x) = min -f(x)

## Equivalencia 2
Encontrar el $\mu$ mas pequeño tal que existaun x que satisfaga f(x) $\leq \mu$ 

## Equivalencia 3
1x
