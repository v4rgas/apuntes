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
2. $\phi, \psi \in A \implies (\neg \phi \in A), (\phi x \psi) \in A, \forall x \in \{\vee, \wedge, \implies, \iff\}$

$$A = L(P)$$

### Semantica

Se preocupa del significado de los simbolos dados por la sintaxis

#### Valuaciones

Una valuacion de las variables de P es una funcion $\sigma : P \rightarrow \{0,1\}$

Una valuacion extendida es $\sigma : L(P) \rightarrow \{0,1\}$

Si $\exists \phi : \sigma(\phi) =1$ es satisfacible

Si $\not\exists \phi : \sigma(\phi) =1$ es contradiccion

Si $\forall \phi : \sigma(\phi) = 1$ es una tautologia
