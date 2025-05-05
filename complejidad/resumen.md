# Palabras, problemas y algoritmos

Un problema es una relacion entre un conjunto de instancias y un conjunto de soluciones.

Formalmente, para un conjunto de instancias I y un conjunto de soluciones S y un problema P, tenemos que:
$$P \subseteq I \times S$$
Una solucion al problema P es una funcion $f: I \rightarrow S$ tal que, $\forall X \in I$:
$$(X,f(X)) \in P$$

## Palabras, alfabetos y letras

- Un alfabeto $\Sigma$ es un conjunto finito
- $x \in \Sigma$ es una letra o simbolo
- Una palabra es una secuencia finita de simbolos, $w = a_1a_2...a_n$ donde $a_i \in \Sigma$

## Maquinas de Turing

Una maquina de Turing determinista es una estructura
$$M = (Q,\Sigma,\Gamma,\delta,q_0,q_{f})$$

- $Q$ es un conjunto finito de estados
- $\Sigma$ es un conjunto finito de simbolos de entrada
- $\Gamma$ es un conjunto finito de simbolos de cinta
- $\delta$ es una funcion parcial de transicion
- $q_0$ es el estado inicial
- $q_f$ es el estado de aceptacion

### Configuracion

Una configuracion de M es un triple:
$$C = (q,i,w) \in Q \times \mathbb{N} \times \Gamma^*$$

- $q$ es el estado actual
- $i$ es la posicion de la cabeza de lectura/escritura
- $w$ es la palabra en la cinta

#### Definiciones

- Una configuracion es de aceptacion si $q = q_f$
- Una configuracion es de detencion si $\delta(q,a_i) = \emptyset$
- Una configuracion es de rechazo si $q \neq q_f$ y $\delta(q,a_i) = \emptyset$

### Siguiente configuracion

Se define la relacion $\rightarrow^M$ tal que:
$$C \rightarrow^M C' \iff C' = (q',i',w')$$
donde:

- $q' = \delta(q,a_i)$
- $i' = i + 1$ si la cabeza se mueve a la derecha
- $i' = i - 1$ si la cabeza se mueve a la izquierda
- $w'$ es la palabra resultante de aplicar la transicion a la cinta

#### Lenguaje aceptado

El lenguaje aceptado por una maquina de Turing M es el conjunto de palabras que llevan a una configuracion de aceptacion. Formalmente:
$$L(M) = \{w \in \Sigma^* | M \text{ acepta } w\}$$

# Maquinas de Turing extendidas

## Maquinas de Turing con multiples cintas

Una maquina de turing con k cintas es una estructura:
$$M = (Q,\Sigma,\Gamma,\delta,q_0,q_f)$$

Donde:

- Q es un conjunto finito de estados
- Σ es un conjunto finito de simbolos de entrada
- Γ es un conjunto finito de simbolos de cinta
- q_0 es el estado inicial
- q_f es el estado aceptacion

Y la funcion de transicion

$$\delta: Q \times \Gamma^k \rightarrow Q \times \Gamma^k \times \{\Gamma \times \{L,R\}\}^k$$

Una configuracion de M esta dada por un triple:
$$C = (q, (i_1,w_1), ... (i_k, w_k)) \in Q \times \mathbb{N}^k \times (\Gamma^*)^k$$

### Teorema

Para toda maquina de turing con k cintas exista una de 1 cinta con el mismo lenguaje

## Maquina de Turing infinita en dos direcciones

Es una maquina de turing con una cinta infinita en ambas direcciones. La cinta es infinita a la izquierda y a la derecha. La cabeza de lectura/escritura puede moverse a la izquierda o a la derecha.

## Maquina de Turing no determinista

Una maquina de turing no dteterminista es una estructura:
$$M = (Q,\Sigma,\Gamma,\Delta,q_0,q_f)$$

Donde:

- Q es un conjunto finito de estados
- Σ es un conjunto finito de simbolos de entrada
- Γ es un conjunto finito de simbolos de cinta
- q_0 es el estado inicial
- q_f es el estado aceptacion
- Δ es una funcion de transicion no determinista
$$\Delta \subseteq (Q \times \Gamma) \times (Q \times \Gamma \times \{L,R\})$$

Una configuracion de M esta dada por un triple:
$$C = (q,i,w) \in Q \times \mathbb{N} \times \Gamma^*$$
Donde:

- Es de aceptacion si $q = q_f$
- Es de detencion si $((q,a_i),(p,b,d)) \notin \Delta$ y $\forall (p,b,d) \in (Q \times \Gamma \times \{L,R\})$

# Medición de recursos

## Tesis de Church-Turing

Una maquina de Turing pede simular eficientemente cualquier modelo realista de computacion. Eficientemente significa diferencia a lo mas polinomial.

## Tiempo en una maquina de Turing

Sea $w \in \Sigma^*$, se define el tiempo de M sobre w como:
$$time_M(w) = |\rho|$$

Donde:

- $\rho$ es la secuencia de configuraciones de M sobre w

Diremos que M corre en tiempo $T(n)$ si:
$$\forall w \in \Sigma^* \text{ tal que } |w| = n, time_M(w) \leq T(n)$$

## Tiempo en una maquina de Turing no determinista

Diremos que M corre en tiempo $T(n)$ si:

$$max |\rho| \leq T(n)$$

Donde el max es sobre todas las computaciones de M sobre w.

## Espacio en una maquina de Turing

Decimos que M es una maquina de Turing con cintas de trabajo si:

$$\delta(p,w) = (q, (b_1,d_1), (b_2,d_2), ... (b_k,d_k)) \implies a_1 = b_1$$

Sea M una maquina de Turing con k cintas de trabajo, se define el espacio de M sobre w como:
$$space_M(w) = \Sigma_{j_2}^{k+1}(|v_j|-1)$$

O sea, la cantidad de celdas utilizadas por las cintas de trabajo.

Diremos que M corre en espacio $S(n)$ si:
$$\forall w \in \Sigma^* \text{ tal que } |w| = n, space_M(w) \leq S(n)$$

## Espacio en una maquina de Turing no determinista

Diremos que M corre en espacio $S(n)$ si:

$$max space_M(\rho) \leq S(n)$$

## Eficiencia de simulacion

Para toda MT M no determinsta que corre en tiempo $T(n)$, existe una MT determinista tal que:
$$L(M) = L(M_{det}), M_{det} \text{ corre en tiempo } c^{T(n)}, c \geq 2$$

Para toda MT M con k cintas que corre en espacio $S(n)$, existe una MT de 1 cinta tal que:
$$L(M) = L(M_{1c}), M_{1c} \text{ corre en espacio } S(n)$$

Para toda MT M con k cintas que corre en tiempo $T(n)$, existe una MT de 1 cinta tal que:
$$L(M) = L(M_{1c}), M_{1c} \text{ corre en tiempo } 6 T(n)^2$$

# Clases de complejidad

Una clase de complejidad C sobre un alfabeto $\Sigma$ es un conjunto de lenguajes sobre $\Sigma$.

La clase co-C se define como:
$$co-C = \{\Sigma^* \backslash L | L \in C\}$$

## Clases basadas en tiempo y espacio

- DTIME($T(n)$): Lenguajes aceptados por una MT determinista de k cintas que corre en tiempo $T(n)$
- NTIME($T(n)$): Lenguajes aceptados por una MT no determinista de k cintas que corre en tiempo $T(n)$
- DSPACE($S(n)$): Lenguajes aceptados por una MT determinista de k cintas de trabajo que corre en espacio $S(n)$
- NSPACE($S(n)$): Lenguajes aceptados por una MT no determinista de k cintas de trabajo que corre en espacio $S(n)$

## Propiedades

1. $DTIME(T(n)) \subseteq NTIME(T(n))$
2. $DSPACE(S(n)) \subseteq NSPACE(S(n))$
3. $DTIME(T(n)) = co-DTIME(T(n))$
4. $DSPACE(S(n)) = co-DSPACE(S(n))$

# Propiedades clases de complejidad

## Teorema de compresion de cinta

Sea M una MT det con k cintas de trabajo que corre en espacio $S(n)$, existe una MT det con k cintas de trabajo que corre en espacio:
$$\lceil S(n)/c \rceil$$

O sea

$$DSPACE(S(n)) = DSPACE(cS(n))$$

## Teorema aceleración lineal

Sea M una MT det con k cintas de trabajo que corre en tiempo $T(n)$, existe una MT det con k cintas de trabajo que corre en tiempo:
$$4\lceil T(n)/c \rceil +n+\lceil n/c \rceil$$

con el mismo lenguaje aceptado.

# P, NP, PSPACE, EXPTIME y otras clases

## Tiempo

- P = DTIME($O(n^k)$)
- EXPTIME = DTIME($2^{O(n^k)}$)
- NP = NTIME($O(n^k)$)
- NEXPTIME = NTIME($2^{O(n^k)}$)

## Espacio

- L = DSPACE($O(\log n)$)
- NL = NSPACE($O(\log n)$)
- PSPACE = DSPACE($O(n^k)$)
- NPSPACE = NSPACE($O(n^k)$)
- EXPSPACE = DSPACE($2^{O(n^k)}$)
- NEXPSPACE = NSPACE($2^{O(n^k)}$)

## Tiempo vs Espacio

- DTIME($T(n)$) $\subseteq$ DSPACE($T(n)$)
- NTIME($T(n)$) $\subseteq$ NSPACE($T(n)$)

### Corolario

- P $\subseteq$ PSPACE
- NP $\subseteq$ NPSPACE
- EXP $\subseteq$ EXPSPACE
- NEXP $\subseteq$ NEXPSPACE

## Espacio vs tiempo

- DSPACE($S(n)$) $\subseteq$ DTIME($c^{S(n)}$)
- NSPACE($S(n)$) $\subseteq$ NTIME($c^{S(n)}$)

### Corolario

- L $\subseteq$ P
- NL $\subseteq$ NP
- PSPACE $\subseteq$ EXPTIME
- NPSPACE $\subseteq$ NEXPTIME

## Tiempo No determinista vs espacio

- NTIME($T(n)$) $\subseteq$ DSPACE($T(n)$)

## Corolario

- NP $\subseteq$ PSPACE
- co-NP $\subseteq$ PSPACE
- NEXPTIME $\subseteq$ EXPSPACE
- co-NEXPTIME $\subseteq$ EXPSPACE

# Espacio no determinista vs tiempo

- NSPACE($S(n)$) $\subseteq$ NTIME($c^{S(n)})$

# Teorema de Savitch

## Funciones construibles

Una funcion $f: \mathbb{N} \rightarrow \mathbb{N}$ es espacio construible si exista una MT tal que:
$$\forall n \in \mathbb{N}, \text{ la MT corre en espacio } O(f(n))$$

En otras palabras, es posible computar f(n) en espacio O(f(n)).

## Teorema de Savitch

Sea $S(n)$ una funcion construible con $S(n) \geq log(n)$, entonces:
$$NSPACE(S(n)) \subseteq DSPACE(S(n)^2)$$

## Corolario

$$PSPACE = NPSPACE = co-NPSPACE$$
$$EXPSPACE = NEXPSPACE = co-NEXPSPACE$$
$$NL=DSPACE(log(n)^2)$$

# Diagonalizacion