# Logica

Sistema formal para determinar validez de argumentos

## Logica proposicional

Una proposicion es una afirmacion que puede ser verdadera o falsa

- **verdadero** (1)
- **falso** (0)

### Ventajas de la logica proposicional

- Es sencilla
- Cualquier construccion correcta admite un valor de verdad

### Sintaxis

Sea un P un conjunto de variables proposicionales

$L(P)$ es el menor conjunto que satisface

$$ P \subseteq L(P)$$
$$ \phi \in L(P) \implies \neg \phi \in L(P)$$
$$ \phi, \psi \in L(P) \wedge x \in ( \vee, \wedge, \rightarrow,\leftrightarrow) \implies (\phi x \psi) \in L(P)$$

$\phi \in L(P)$ es una formula proposicional

### Teorema de lectura unica

Garantiza que se puede parsear toda formula en $L(P)$ obteniendo un arbol sintactico

#### Arbol sintactico

- Cumple que todas las hojas son las variables mencionadas en $\phi$

- Todo nodo interior es una formula y es padre de sus subformulas

- $\phi$ es la raiz

Dado un P en una formula $\phi \in L(P)$

- Atomica:
  $$\phi = p \iff p \in P$$

Sino, es compuesta

### Induccion en $L(P)$

Para cada $A \subseteq L(P)$ tal que

1. $p \in A \forall p \in P$
2. $\phi, \psi \in A \implies (\neg \phi \in A), (\phi x \psi) \in A, \forall x \in \{\vee, \wedge, \rightarrow, \leftrightarrow \}$

$$A = L(P)$$

### Semantica

Se preocupa del significado de los simbolos dados por la sintaxis

#### Valuaciones

Una valuación de las variables de P es una funcion $\sigma : P \rightarrow \{0,1\}$

Una valuacion extendida es $\sigma : L(P) \rightarrow \{0,1\}$

Si $\exists \phi : \sigma(\phi) =1$ es satisfacible

Si $\not\exists \phi : \sigma(\phi) =1$ es contradicción

Si $\forall \phi : \sigma(\phi) = 1$ es una tautología

#### Visualizaciones

Los mundos posibles se pueden expresar de a través de tablas de verdad

Un modelo son todas las valuaciones que satisfacen una cierta formula

### Clases de equivalencia $\phi \equiv \psi$

$\phi, \psi \in L(P)$ son equivalentes si $\forall \sigma, \sigma(\phi) = \sigma(\psi)$

Para demostrar esto se pueden comparar sus tablas de verdad o utilizar reglas de equivalencia lógica

### Conectivos N-Arios

$$C(p_1,...p_n) = \bigvee_{i:b_1=1}(\bigwedge_{j:\sigma_i(p_j)=1}p_j\wedge\bigwedge_{j:\sigma_i(p_j)=0}\neg p_j)$$

### Formas normales

#### DNF

$$\phi = \bigvee_{i=1}^m(\bigwedge_{j=1}^{n} l_{ji})$$

#### CNF

$$\phi = \bigwedge_{i=1}^m(\bigvee_{j=1}^{n} l_{ji})$$

### Consecuencia logica

$$\Sigma \subseteq L(P), \phi \in L(P)$$
$$\Sigma \models \phi$$
si para $\Sigma$ es verdadero para toda valuacion en la que $\phi$ es verdadero

$\Sigma$ se llama conjunto de premisas o base de conocimiento (KB)

$\phi$ es la consecuencia

#### Definiciones

- Inconsistencia $\forall \phi, \Sigma \models \phi$
- Tatuologia si $\{T\} \models \phi$
- $\Sigma\cup\{\phi\} \models \psi \iff \Sigma \models \{\phi \rightarrow \psi\}$
- $\Sigma \models \phi \wedge \Sigma \cup \phi \models \psi \implies \Sigma \models \psi$

#### Monotonia

$$\Sigma \models \phi \implies \Sigma \cup \psi \models \phi$$

### Revision de conocimiento $\Sigma \circ \phi$

## Satisfacibilidad

### Teorema inconsistencia

$$\Sigma \models \varphi \iff models(\Sigma \cup \{\neg \varphi\}) = \empty$$

### Problema SAT

Es el problema de saber si algo es satifacible o no

Evaluar una formula tiene un complejidad igual a $O(m^2)$ con m el largo de la formula (si es un string)

#### Fuerza bruta

Se podria probar cada evaluación para ver si existe alguna que satisfaga

$$O(m^2 \cdot 2^n)$$

#### DPLL

Disenado para formulas CNF

- Si $\varphi$ es un literal decimos que es una clausula unitaria
- Si p es una variable que aparece solo como p o solo como $\neg p$ entonces es un literal puro
- Si una clausula ya incluye un 1, se elimina de el conjunto
- Si tiene un 0, se dejan solo variables libres. Si no quedan, se deja en el conjunto

### Sistemas deductivos
Podemos demotrar que un conjunto es inconsistente si como consecuencia logica se tiene la clausula vacia

#### Resolucion
$$\neg p \vee C_1, p \vee C_2 \implies C_1 \vee C_2$$

Un conjunto es incosistente si se puede obtener por resolucion la clausula vacia

Si la resolucion tiene la regla de tautologia, factorizacion y resoluciion es completo y correcto

- Correctitud: Todo lo que deducimos es consecuencia logica

- Completitud: Podemos deducir cualquier consecuencia logica


# Maquinas de Turing

## Palabras
Sea un alfabeto A un conjunto finito de simbolos

Una palabra es:
- $w=\epsilon$ palabra vacia
- $w=a,  a \in A$
- $w = a \cdot w'$ para alguna palabra w' (concatenar)

El conjutno de todas las palabras con simbolos de A es A*

### Lenguaje
$$L \subseteq A*$$

## Problemas de decision
Sea A un alfabeto y $L \subseteq A*$ un lenguaje, un problema de decision asociado a L corresponde a decidir si 
$$\exists w \in L:w \in L$$

## Algoritmo
### Tesis Church-Turing
Todo proceso efectivo es equivalente a una máquina de Turing

## Maquina de Turing
- Memoria para lectura y escritura
- Cabeza lectora de estados internos
- Conjunto de instrucciones

Una cabeza lectura lee una cinta infinita e inicia con un "estado mental"

Si se detiene en un estado no final, se rechaza

### Definicion
$$M = (Q, A, q_0, F, \delta)$$
- Q es un conjunto finito de estados
- A es un alfabeto
- $q_0$ es un estado inicial
- $F \subseteq Q$ es un conjunto finito de estados finales
- $\delta$ es la funcion parcial de transición

### Lenguaje aceptado
M es un lenguaje aceptado si
$$L(M) = \{w \in A* | M\ acepta\ w\}$$

### Configuración
Una configuración de M es una palabra
$$u \cdot q \cdot v \in A*_{-} \cdot Q \cdot A$$
q es el estado actual, $u \cdot v$ es el contenido de la cinta y |u|+1 la posicion de la cabeza

#### Estados
- Detencion si $\delta(q,a)$ no esta definido
- Aceptación si detención y $q \in F$
- Rechazo si detención y $q \not\in F$

### Ejecuciones
Para todo input existe una ejecución unica

### Decibilidad
Un lenguaje es decidible o recursivo si existe una MT M tal que
- L = L(M)
- M se detiene en todo input

Si es que no se detiene para todo input, es recursivametne numerable pero no decidible


Un programa es decidible si existe un algoritmo

## Maquinas universales
Si cod(M) es la codificacion de una maquina de turing

Una maquina universal U:
- Su entrada es de la forma cod(M)00w para $w \in A*$ y M maquina con alfabeto A
- U acepta cod(M)00w si y solo si M acepta w
- Rechaza si entrada no tiene forma correcta

## Problema Halting
No eixste una maquina de turing tak que
- H = L(M)
- M se detiene para todo input

## Reduccion
Una reduccion trasforma un problema A en un problema B tal que si hay un algoritmo para B hay uno para A

Una reduccion trasforma un lenguaje en otro tal forma que si un algoritmo para B hay un algoritmo para A

### Reducciones de mapeo
Si existe una funcion computable tal que
$$w \in L_1 \iff f(w) \in L_2$$
$$L_1 \leq_m L_2$$

## Funciones computable
Una funcion es computable si existe una maquina que a calcula

## Lenguajes complementarios
$$\bar{L} = A* \backslash L$$


# rellenar aca 

## Lenguajes
- RE: aceptados por alguna maquina
- coRE: complemento de RE
- R: decidible, aceptados por alguna maquina y siempre se detiene

## Clases de complejidad

## Concepto de paso
Dada una MT un paso va a ser una transformacion dentro de la maquina

### Tiempo de ejecucion
El tiempo es la cardinalidad del conjunto de todos los pasos

El tiempo en el peor caso es el maximo de los tiempos para cada palabra

### Tiempo de aceptacion
- M se detiene para toda entrada
- L = L(M)
- $t_M \in O(g)$

entonces L es acpetado en tiempo g

## P
Un programa puede ser solucioando eficientemente si es que su complejidad es un $O(n^k)$

P es el conjutno de todos los algoritmos con esta complejidado

## P y EXP
$$P \subseteq EXP$$

## Reduccion polinomial
Si existe una funcion computable en tiempo polinomial  tal que
$$w \in L_1 \iff f(w) \in L_2$$
$$L_1 \leq_p L_2$$

### Teorema
1. $L_2 \in P \implies L_1\in P$
2. $L_2 \notin P \implies L_1 \not\in P$

## Dificultad Relativa
### Hardness
Dada una clase de complejidad C, L es C-hard si
$$\forall L' \in C, L'\leq_p L$$

## Dificultad Exacta
### Completitud
L es C-Completo si
- $L \in C$
- $L$ es $C$-hard

#### Teorema
Si $P \subsetneq C$ y $L$ es C-hard entonces $L \notin P$


