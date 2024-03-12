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

y es una combinacion convexa si $\sum_{i=1}^n a_i = 1, 0 \leq a \leq 1$

### Conjunto Convexo
Un conjunto $S \subseteq R^n$ es convexo si
$$\forall x,y \in S, \forall \lambda \in [ 0,1 ] \implies \lambda x + (1 - \lambda)y \in S$$

El conjunto definido por una desigualdad lineal es convexo

### Funcion Convexa
$$f(\lambda x)$$
