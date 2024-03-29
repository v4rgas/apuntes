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
