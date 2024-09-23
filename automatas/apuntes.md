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

Por todo automata con transicion de función parcial, existe un automata A' con funcion total de transicion tal que 

$$L(A) = L(A')$$


## Complemento, intersección y union
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

# Automatas no deterministas
## Estructura
$$A = (Q, \Sigma, \Delta,I,F)$$

donde existe una relacion de transición

$$\Delta \subseteq Q \times \Sigma \times Q$$

y un conjunto de estados iniciales
$$I \subseteq Q$$

## Lenguaje aceptado
A acepta si existe una ejecución de aceptación

A rechaza si todas las ejecuciones no son de aceptación

$$L(A) = \{ w \in \Sigma^* | A\ acepta\ w\}$$

## Teorema
Para todo autómata finito no determinista A, existe un automata determinista A' tal que

$$L(A) = L(A')$$

# Regex
## Definición
R es Regex sobre $\Sigma$ si R es igual a:
1. $a \in \Sigma$
2. $\epsilon$
3. $\emptyset$
4. (R1 + R2) (R1 o R2)
5. $R1 \cdot R2$ (R1 seguido de R2)
6. $R*$ (0 o más)

## Teorema Kleene
Para todo lenguaje regular L, existe una regex R tal que $L = L(R)$
# Automatas con transiciones sin lectura
## Automata finito no-determinista con $\epsilon$-transiciones
1. Tiene transiciones no deterministas
2. Tiene transiciones leyendo $\epsilon$ (vacio)

Es equivalente a un NFA



# Lenguajes no regulares
## Lema de bombeo
Sea $L \in \Sigma*$, Si L es regular entonces

$$\exists N > 0, \forall x \cdot y \cdot z \in W, |y| \geq N$$

$$\exists u \cdot v \cdot w = y, v \neq \varepsilon, \forall i \geq 0, x \cdot u \cdot v^i \cdot w \cdot z \in L$$

# Minimización de automatas
1. Eliminar estados inaccesibles
2. Colapsar estados equivalentes

La parte dificil es colapsar estados equivalentes
## Función de transicion extendida
$$\hat\delta : Q \times \Sigma* \rightarrow Q$$

inductivamente como:
$$\hat\delta(q,\epsilon) \equiv q\$$
$$\hat\delta(q, w\cdot a) \equiv \delta(\hat\delta(q,w),a)$$

## Cuando colapsar estados
p y q son indistiguibles ($p \approx_A q$) si

$$p \approx_A q \iff (\hat\delta(p,w) \in F \iff \hat\delta(q,w) \in F), \forall w \in \Sigma $$

## Algoritmo para encontrar las clases de equivalencia
1. Construya tabla con los pares {p,q}
2. Marque los pares (p,q) si p $\in F$ y q $\notin F$ o viceversa
3. Repita hasta que no se marquen mas pares
   1. Marque (p,q) si (p,q) no esta marcado y (delta(p,a), delta(q,a)) esta marcado
4. Las clases de equivalencia son los pares no marcados $p \neq q \iff \{p,q\}$ está marcado








