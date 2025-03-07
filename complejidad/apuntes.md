# Titulo

## Formalizar idea de problema computacional

Para un conjunto de instancias I y un conjunto de soluciones S y un problem P

$$P \subseteq I \times S $$

Una solucion al problema P es una funcion $f: I \rightarrow S$ tal que, $\forall X \in I$

$$(X,f(X)) \in P$$

## Palabras
- Un alfabeto $\Sigma$ es un conjunto finito
- $x \in \Sigma$ es una letra o simbolo
- Una palabra es una secuencia finita de simbolos

### Definiciones
- $|w| =$ numero de letras en w

- $|\epsilon| = 0$ es la palabra vacia

- $\Sigma*$ todas las palabras sobre $\Sigma$

### Concatenacion

- $a \cdot b = a_1...b_1...$

- $w^n = w \cdot w \cdot w...w_n$

-  $w^0 = \epsilon$


### Simplificaciones

#### Instancias de un problema

Toda estructura finita es posbile de representar como una palabra de un alfabeto finito de al menos dos letras 

(es como reducirlo a binario)

Existen ciertaws representaciones que simplifican la solucion de un problema

##### Ejemplo
Un grafo G = (V,E) se puede representar como una palabra de la forma

$G = v_1v_2v_3...v_n$

$E = (v_1,v_2)(v_2,v_3)...(v_{n-1},v_n)$

$w = G \cdot E$

#### Soluciones de un problema

Nos restringimos al conjutno de problemas de decision

- $S = \{True,False\}$ es el conjunto de soluciones
- cada instancia tiene una y solo una solucion

##### Ejemplo
Verificar si dos nodos de un grafo estan conectados por una arista

Instancia: Un grafo G y dos nodos u,v
Problema: True si existe una arista que conecta u y v, False en otro caso




