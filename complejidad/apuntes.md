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

- $w^0 = \epsilon$

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

#### Problema computacional (simplificado)

Un problema de decision es un $L \subseteq \Sigma*$

##### Ejemplo

- $L = \{w \in \{0,1\}^* | w = 0^n1^n, n \geq 0\}$, modela el problema de si dos palabras tienen la misma longitud

Un problema no es mas que un conjunto de palabras que defino, a esto se le denomina un lenguaje

Una solucion entonces corresponde a una funcion computable:

$$f: \Sigma* \rightarrow \{True,False\}$$

$$f(w) = True \iff w \in L$$

#### Computabilidad

Definiciones formales

- Funciones parcialmente recursivas
- $\lambda$-calculo
- Maquinas de Turing
- Sistema de Post

Utilizaremos maquinas de Turing porque modela bien la idea de recursos, tiempo y espacio

## Maquinas de Turing

### Definicion

- Cinta infinita dividida en celdas
- Cabezal de lectura/escritura
- Conjunto finito de estados
- Funcion de transicion

### Formalizacion

Una maquina de Turing determinista es una 6-tupla

$$M = (Q,\Sigma,\Gamma,\delta,q_0,q_f)$$

- $Q$ es el conjunto finito de estados
- $\Sigma$ es el alfabeto de entrada
- $\Gamma$ es el alfabeto de la cinta
- $\delta: Q \times \Gamma \rightharpoonup Q \times \Gamma \times \{L,R\}$ es la función de transición
- $q_0 \in Q$ es el estado inicial
- $q_f \in Q$ es el estado final

Notar que

$$\delta(q_f,a) = \emptyset$$

#### Configuracion

Una configuracion de una maquina de Turing es una terna

$$C = (q,i,w) \in Q \times \mathbb{N} \times \Gamma*$$

- $q$ es el estado actual
- $i$ es la posicion del cabezal
- $w$ es la palabra en la cinta

Puede ser de aceptacion, de rechazo o de no detencion

#### Siguiente configuracion

Se define la relacion $\rightarrow^M$ tal que

$$(p, i, u \cdot a \cdot v) \rightarrow^M (q, j, u \cdot b \cdot v)$$

#### Ejecucion

Una ejecucion de una maquina de Turing es una secuencia de configuraciones (no necesariamente finita)

$$p: C_0 \rightarrow^M C_1 \rightarrow^M ... \rightarrow^M C_n$$

Decimos que p es una ejecion de M sobre la palabra w si

- $C_0 = (q_0,0,w)$
- Si p es finito, entonces Cn es de detencion

#### Lenguaje aceptado

El lenguaje aceptado por una maquina de Turing M es

$$L(M) = \{w \in \Sigma* | M \text{ acepta w}\}$$

## Maquinas de Turing extendidas

1. Cinta infinita en ambas direcciones
2. Multiples cintas y cabezales
3. No-determinismo
4. Cinta en 2D, 3D, etc

Son todas equivalentes a una maquina de Turing

### Multiples cintas

Dado el simbolo que cada cabezal lee, se elige el siguiente estado

$$M = (Q,\Sigma,\Gamma,\delta,q_0,q_f)$$

donde 

$$\delta: Q \times \Gamma^k \rightharpoonup Q \times (\Gamma \times \{L,R\})^k$$

#### Configuracion

Una configuracion esta dada por

- Estado actual
- Posicion de cada cabezal
- Contenido de cada cinta

La representamos como un triple

$$(q, (i_1,...,i_k), (w_1,...,w_k))$$

- Es aceptacion si $q = q_f$
- Es de detencion si $\delta(q,w_1[i_1],...,w_k[i_k])$ no esta definido

#### Siguiente configuracion

Se define la relacion $\rightarrow^M$ tal que

<!-- $$(p, (i_1,...,i_k), (u_1,...,u_k)) \rightarrow^M (q, (i_1 + d_1,...,i_k + d_k), (v_1,...,v_k))$$ -->

`COMPLETAR`


#### Teorema

Para cada maquina de Turing M con k cintas, existe una maquina de Turing M' con una cinta tal que

$$L(M) = L(M')$$


### Infinita en dos direcciones

Dada una maquina de Turing M, existe una maquina de Turing M' con cinta infinita en ambas direcciones tal que

$$L(M) = L(M')$$


Es equivalente a una maquina de Turing con dos cintas

### No-determinismo

En una maquina no determinista, la funcion de transicion es una relacion de la forma

$$\Delta \subseteq (Q \times \Gamma) \times (Q \times \Gamma \times \{L,R\})$$


#### Ejecucion 

M acepta w si existe una ejecucion de M sobre w que termina en un estado final


#### Interpretaciones

Puede ser visto como:

1. Paralelización infinito
2. Guessing and verifying

NO es aleatoriedad, y cada input siempre tiene la misma salida

# Espacio

## Cinta de trabajo

Decimos que una maquina de turing tiene cinta de trabajo si

$$\delta(p,a) = (q,(b,d) \implies a = b$$

## Defincion 

Sea una TM con k cintas de trabajo

$$space_M(w) := \Sigma_{i=2}^{k+1} (|v_j|-1)$$

Intuitivamente, es la cantidad de celdas que se usan en la cinta de trabajo

$$space_M(w) := max\{space_M(\rho)\}  \leq S(|w|)$$


## Eficiencia no determinismo

Dada una maquina de Turing no determinista M que corre en tiempo $T(n)$,
sabemos que existe una maquina determinista que corre en tiempo $c^T(n)$ para algun $c > 1$ 
con c cintas

# Propiedades de las clases de complejidad

## Propiedades simples

## Compresion de cintas
Sea $M$ una maquina de Turing que corre en tiempo $T(n)$ y espacio $S(n)$. Existe una maquina de turing determinista con k cintas de trabajo que ahora corre en espacio

$$S(n)/c$$

o sea

$$DSPACE(c \times S(n)) \subseteq DSPACE(S(n))$$

## Aceleración Lineal
Sea $M$ una máquina de Turing determinista con $k$ cintas ($k > 1$) que corre en tiempo $T(n)$.  
Para todo $c \in \mathbb{N} \setminus \{0\}$, existe una máquina de Turing determinista $M_c$ con $k$ cintas que corre en tiempo:

$$4 \cdot \lceil \frac{T(n)}{c} \rceil + n + \lceil \frac{n}{c} \rceil$$

tal que:

$$L(M) = L(M_c)$$

# Clases P, NP, PSpace y otras

## Tiempo no determinista
Se define el tiempo no determinista como

$NP := \bigcup_{k \in \mathbb{N}} NDTIME(n^k)$

$NEXP := \bigcup_{k \in \mathbb{N}} NDTIME(2^{n^k})$

## Espacio

$$L := DSPACE(log(n))$$

$$PSPACE := \bigcup_{k \in \mathbb{N}} DSPACE(n^k)$$

$$EXPSPACE := \bigcup_{k \in \mathbb{N}} DSPACE(2^{n^k})$$

## Espacio no determinista

$$NL := NSPACE(log(n))$$
$$NPSPACE := \bigcup_{k \in \mathbb{N}} NSPACE(n^k)$$
$$NEXPSPACE := \bigcup_{k \in \mathbb{N}} NSPACE(2^{n^k})$$

## Relaciones entre clases

1. $DTIME(n) \subseteq NTIME(n)$
2. $SPACE(n) \subseteq NSPACE(n)$

Si $T(n) \in O(T'(n))$ y $S(n) \in O(S'(n))$ entonces

3. $DTIME(T(n)) \subseteq DTIME(T'(n))$
4. $DSPACE(S(n)) \subseteq DSPACE(S'(n))$

## Relacion entre tiempo y espacio

1. $DTIME(T(n)) \subseteq DSPACE(T(n))$
2. $NTIME(T(n)) \subseteq NSPACE(T(n))$


Si lo puedo resolver en tiempo T(n), y en cada paso a lo mas me muevo a una celda, entonces el espacio que uso es T(n) (en el peor de los casos)


1. $DSPACE(S(n)) \subseteq DTIME(c^{S(n)})$
2. $NSPACE(S(n)) \subseteq NTIME(c^{S(n)})$

Notemos que la mayor cantida de configuracion esta dada por

$$|Q| \cdot n \cdot |\Gamma|^{S(n)} \cdot S(n)$$

Como no puede repetir configuracions (si no se queda pegada) entonces el tiempo es menor o igual a la cantidad de configuraciones

## Relacion tiempo no determinsta y espacio

1. $NTIME(T(n)) \subseteq DSPACE(T(n))$