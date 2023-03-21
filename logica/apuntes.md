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
$$ \phi, \psi \in L(P) \wedge x \in ( \vee, \wedge, \implies,\iff) \implies (\phi x \psi) \in L(P)$$

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

