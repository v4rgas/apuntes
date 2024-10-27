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


# Relaciones de Myhill-Nerode
## Definición
Sea L un lenguaje sobre $\Sigma$ y $x, y \in \Sigma*$
$$x \equiv_L y \iff \forall z \in \Sigma*, xz \in L \iff yz \in L$$

O de manera equivalente

$$x \equiv_L y \iff \hat\delta(q_0, x) \in F \iff \hat\delta(q_0, y) \in F$$

## Clases de equivalencia
$$[x]_L = \{y \in \Sigma* | x \equiv_L y\}$$

## Relacion de Myhill-Nerode

Una relación de equivalencia R sobre $\Sigma*$ es una relación de Myhill-Nerode si y solo si

1. Es una congruencia por la derecha, es decir, $x \equiv_L y \implies \forall z \in \Sigma*, xz \equiv_L yz$
2. Refina L, es decir, $x \equiv_L y \implies x \in L \iff y \in L$
3. Es finita


# Teorema de Myhill-Nerode
Un lenguaje L es regular si y solo si el numero de clases de equivalencia es finito

# Autómatas en dos direcciones
## Automatas finitos deterministas en dos direcciones
1. Tiene una cinta infinita
2. Puede moverse a la izquierda o derecha
3. Tiene una cabeza de lectura


### Definición
$$A = (Q, \Sigma, \vdash, \dashv, \delta, q_0, q_f)$$

Donde

- $\vdash$ es el simbolo de inicio
- $\dashv$ es el simbolo de fin
- $\delta: Q \times \Sigma \rightarrow Q \times \Sigma \times \{L,R\}$
- $q_0$ es el estado inicial
- $q_f$ es el estado final
- La cinta contiene un simbolo de inicio y fin $\vdash, \dashv$

### Configuración
Viene dada por un par
$$(q,i) \in Q \times \{0,1,2,...\}$$

Donde i es la posicion de la cabeza de lectura y q es el estado actual

### Relacion de siguiente configuración

$$(q,i) \rightarrow^A (q',i')$$

### Ejecución

Una ejecución de A sobre w es una secuencia finita de configuraciones

$$(q_0,0) \rightarrow^A (p_1, i_1) \rightarrow^A (p_2, i_2) \rightarrow^A ... \rightarrow^A (p_f, i_f)$$

Es de aceptación si $p_f = q_f$, $i_f = n+1$, $w = w_1...w_n$

## 2DFA vs lenguajes regulares

Para todo 2DFA A, existe un DFA A' tal que $L(A) = L(A')$

# Evaluación de Regex
## Como evaluar una regex
1. Convertimos R en un $\epsilon$-NFA A_R$
2. Verificamos si $w \in L(A_R)$

## Como evaluar un automata no determinista
### Tamaño del input

- $|w|$: Largo de documento
- $|A|$: $|Q| + |\Delta|$

### Backtracking


fun eval-backtracking(A, w):
1. return backtracking(A, w, q_0, 1)

fun backtracking(A, w, p, i):
1. if $i \leq |w|$:
   1.  for $(p, a_i, q) \in \Delta$:
       1. if backtracking(A, w, q, i+1):
          1. return True
        2. else if $q \in F$:
            1. return True
2. return False

### 2DFA
fun eval-2DFA(A, w):
1. q = $q_0$
2. for i = 1 to $|w|$:
   1. q = $\delta(q, w_i)$
3. return $q \in F$

Complejidad $O(|w|+ |A|)$

### NFA Determinización
fun eval-NFA(A, w):
1. A' = determin(A)
2. return eval-DFA(A', w)

Complejidad $O(2^{|Q|} + |w|)$


### NFA on-the-fly

$\delta^{det}: 2^Q \times \Sigma \rightarrow 2^Q, \delta^{det}(S, a) = \{q \in Q | \exist p \in S, (p,a,q) \in \Delta \}$

1. Mantenemos un conjunto de estados S
2. Para cada letra a en w, actualizamos $\delta^{det}(S,a)$

fun eval-NFA-on-the-fly(A, w):
1. S = $I$
2. $for i = 1 to $|w|$:$
   1. $S_{old} = S$
   2. $S = \emptyset$
   3. $for p \in S_{old}:$
      1. $S = S \cup \{ q | (p,a_i,q) \in \Delta \}$
3. return $S \cap F \neq \emptyset$

Complejidad $O(|w| \cdot |A|)$

# Transductores
## Definición
Es una tupla

$$T = (Q, \Sigma, \Omega, \Delta, I, F)$$

Donde

- Q es un conjunto finito de estados
- $\Sigma$ es un alfabeto de entrada
- $\Omega$ es un alfabeto de salida
- $\Delta \subseteq Q \times \Sigma \times \Omega \times Q$
- I es el estado inicial
- F es el conjunto de estados finales


## Propiedades
Para una relación $R \subseteq \Sigma^* \times \Omega^*$

$$\pi_1(R) = \{w \in \Sigma^* | \exists v \in \Omega^*, (w,v) \in R\}$$
$$\pi_2(R) = \{v \in \Omega^* | \exists w \in \Sigma^*, (w,v) \in R\}$$

### Teorema
Si $T$ es un transductor, entonces

$\pi_1(T)$ y $\pi_2(T)$ son regulares sobre $\Sigma$ y $\Omega$ respectivamente


# Introducción al analisis lexico

## Transductores deterministas
### Funcion parcial
Un transductor define una funcion parcial si

2. $\forall u \in \Sigma^*, \llbracket T \rrbracket (u) \leq 1$


Un transductor es determinista si y solo si

1. T define una funcion parcial $\llbracket T \rrbracket : \Sigma^* \rightarrow \Omega^*$
2. $\forall (p,a_1,b_1,q_1), (p,a_2,b_2,q_2) \in \Delta, a_1 = a_2 \implies b_1 = b_2, q_1 = q_2$

3. $\forall (p, \epsilon, b, q) \in Delta, \forall (p, a, b', q') \in \Delta, (a, b', q') = (\epsilon, b, q)$

# Analisis lexico
## Definición
1. La sintaxis de un lenguaje es un conjunto de reglas que describes los programas validos en el lenguaje
2. La semántica de un lenguaje define el significado de los programas validos

## Verificación de sintaxis
1. Analisis lexico (Lexer)
2. Analisis sintactico (Parser)
3. Analisis semantico

## Lexer
Se divide el programa en tokens, donde un token es una secuencia de caracteres que representa un elemento del lenguaje

## Generador de analisis lexico
Un generador de analisis lexico es un software que crea codigo para un lexer a partir de una especificacion de tokens

### Lex
El formato de un programa en Lex es
1. Declaraciones
2. Reglas de traducción
3. Funciones auxiliares

Las reglas de traducción son de la forma
PATTERN ACTION

Donde PATTERN es una expresion regular y ACTION es un fragmento de codigo

Por ejemplo
```
digit [0-9]
%%
{digit}+ printf("Numero entero\n");
```

#### Evaluación de Lex
Sea $P_1, P_2, ..., P_n$ un conjunto de patrones y $C_1, C_2, ..., C_n$ un conjunto de acciones en 'lex.l'

1. Por cada patrón $P_i$ contruimos un NFA $A_i = (Q_i, \Sigma, \Delta_i, \{q_{0i}\}, \{q_{fi}\})$

2. Evaluamos con el transductor determinista $T = (Q, \Sigma, \{C_1, C_2, ..., C_n\}, \Delta, \{q_0\}, F)$
   - $Q = 2^{\cup_{i=1}^n Q_i}$
   - $q_0 = \{q_{0i} | i = 1, 2, ..., n\}$
   - $(S, a, \epsilon, S') \in \Delta \iff S' = \{q | \exists i, p \in S, (p,a,q) \in \Delta_i\}$
   - $(S, _, C_i, q_0), (S, EOF, C_i, q_0) \in \Delta \iff q_{fi} \in F$
   - $F = \{q_0\}$


# Algoritmo de Knuth-Morris-Pratt
## Problema
Dado un patron $w = w_1w_2...w$ y un documento $d = d_1d_2...d_n$, encontrar todas las ocurrencias de w en d

### Algoritmo ingenuo
1. Para cada i = 1 a n-(m-1)
   1. Para cada j = 1 a m
      1. Si $d_{i+j-1} \neq w_j$ entonces break
   2. Si $j > m$, return (i, i+m-1)

### Automata de un patron
Dado una palabra w, definimos el automata $NFA\ A_w = (Q, \Sigma, \Delta, I, F)$

Donde

- Q = {0, 1, ..., m}
- $\Delta = \{ (0,a,0) | a \in \Sigma \} \cup \{ (j, w_{j+1}, j+1) | j = 0, 1, ..., m-1 \}$
- I = {0}
- F = {m}

#### Determinización
Dado un NFA A, definimos el DFA $DFA\ A' = (Q', \Sigma, \delta, q_0, F')$

Donde

- $Q' = 2^Q$
- $\delta(S, a) = \{ q | \exists p \in S, (p,a,q) \in \Delta \}$
- F' = ${S \in 2^Q | S \cap F \neq \emptyset}$


Para todo $S \in Q'$

$$ i \in S \iff w_1...w_i \text{ es un sufijo de } w_1,...w_{max(S)}$$

### Automata finito con k-lookahead
Dado un automata A, definimos el automata $A_k = (Q, \Sigma, \Delta, I, F)$


# Gramaticas libres de contexto
## Definición
Una gramatica libre de contexto es una tupla

$$G = (V, \Sigma, P, S)$$

Donde

- V es un conjunto finito de variables
- $\Sigma$ es un alfabeto de terminales tal que $V \cap \Sigma = \emptyset$
- $P \subseteq V \times (V \cup \Sigma)^*$ es un conjunto finito de producciones
- $S \in V$ es el simbolo inicial

## Notación
- Para variables en una gramatica se usa $X, Y, Z, A, B, C$
- Para terminales se usa $a, b, c, d, e, f$
- Para palabras en $V \cup \Sigma$ se usa $\alpha, \beta, \gamma$
- Para una producción $(A, \alpha) \in P$ se escribe $A \rightarrow \alpha$
  
## Simplificación
Si tenemos un conjunto de reglas de la forma

1. $X \rightarrow \alpha_1$
2. $X \rightarrow \alpha_2$
3. $X \rightarrow \alpha_3$
4. ...

$$X \rightarrow \alpha_1 | \alpha_2 | \alpha_3 | ... | \alpha_n$$


## Producciones

Definamos la relación $\Rightarrow$ como $u \Rightarrow v$ si y solo si existe una producción $A \rightarrow \alpha$ tal que $u = \alpha_1 A \alpha_2$ y $v = \alpha_1 \alpha \alpha_2$

## Derivaciones

Definamos la relación $\Rightarrow^*$ como $u \Rightarrow^* v$ si y solo si existe una secuencia finita de palabras $u = u_0, u_1, u_2, ..., u_n = v$ tal que $u_i \Rightarrow u_{i+1}$

## Lenguaje generado por una gramatica
$$L(G) = \{w \in \Sigma^* | S \Rightarrow^* w\}$$

## Lenguajes libres de contexto
Un lenguaje es libre de contexto si y solo si existe una gramatica libre de contexto que lo genere