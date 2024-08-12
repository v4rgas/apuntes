# Palabras y autómatas
- Letras: a, b, c, d, e...
- Palabras: w, u, v, x, z...
- Alfabetos: $\Sigma, \Gamma, ...$
- Lenguajes: L, M, N...
- Numeros: i, j, k...

## Alfabetos, letras y palabras
- Un alfabeto $\Sigma$ es un conjunto finito

- Un elemento de un alfabeto es una letra o simbolo

- Una palabra es una secuencia finita de letras

- El largo de palabra w corresponde al numero de letro

- $\epsilon$ es una palabra sin simbolos 

- $\Sigma*$ es el conjunto de todas las palabras sobre $\Sigma$

## Concatenacion 
Dado dos palbras $u, v \in \Sigma^*, u \cdot v = u_1...u_n v_1...v_n$

Es asociativa

## Lenguajes
L es un lenguaje si $L \subseteq \Sigma^*$

## Automatas Finitos
Modelo de computacion mas sencillo, con cantidad finita de memoria

- Procesa cada palabra en una sola pasada
- Automata decide si acepta o rechaza el input

### Finito determinista
- Q conjunto finito de estados
- $\Sigma$ es un alfaeto de input
- $\delta: Q \times \Sigma \rightarrow Q$ es la funcion de transición
- $q_0 \in Q$ estado inicial
- $F \subseteq Q$ es un conjunto de estados finales

### Ejecucion
Dado un automata A

Una ejecucion p de A sobre w

$$p: p_0 \rightarrow^{a_1} p_1 \rightarrow^{a_2} p_2...$$

$\mathcal{L}$

Estado Letra, Estado Letra, Estado Letra

### Lenguaje aceptado por un automatada
Un lenguaje se dice regular si y solo si existe un automata finito determinista A tal que

$$L = L(A)$$

Un lenguaje aceptado por A se define como 

$$L(A) = \{ w \in \Sigma^* | A \ acepta \ w\}$$

# Construcciones de automatas
## Automatas con funcion parcial de transición
A = ($Q, \Sigma, \delta, q_0, F$)

Donde

$$\delta: Q \times E \rightarrow Q$$

A acepta w si existe una ejecucion de A sobre w


## Comnplemento, intersección y union
Dados dos lenguajes L, L' $\subseteq \Sigma^*$
$$L^C = \{w \in \Sigma^* | w \notin L \}$$
$$L \cap L' = \{w \in \Sigma^* | w \in L \wedge w \in L' \}$$
$$L \cup L' = \{w \in \Sigma^* | w \in L \vee w \in L' \}$$

### Complemento
$$A = (Q, \Sigma, \delta, q_0, Q \backslash F)$$

### Intersección
$$Q^\times = Q \times Q' = \{(q, q') | q \in Q, q' \in Q' \}$$
$$\delta^\times((q, q'),a) = (\delta(q,a), \delta'(q',a))$$
$$q^\times_0 = (q_0, q_0')$$
$$F^\times = F \times F'$$

Equivalente al producto de A y A'
$$A \times A'$$


