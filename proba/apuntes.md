# Fundamentos de los Modelos de Probabilidad

## Eventos y Probabilidad

Existen eventos determinísticos y no determinístico

Los eventos NO determinísticos son el objeto de la estadística

### Espacio de Resultados Posibles

Para todo fenómeno se define un conjunto de resultados llamado "Espacio de Resultados Posibles"

Un evento de interés se compone de uno o más resultados en este espacio

Ejemplos:

- Dado:
  $$S = \{1,2,3,4,5,6\}$$
- Tiempo:

$$S = \{x \ | \ 10 < x < 100\}$$

### Probabilidad Clásica

La probabilidad clásica corresponde a la proporción de eventos de que ocurrirá un suceso suponiendo equiprobabilidad

### Probabilidad Frecuentista

Se aproxima la probabilidad de un suceso por el limite de la frecuencia relativa de un suceso

$$P(A) = \underset{n \rightarrow \infty}{lim} \frac{n_A}{n}$$

### Probabilidad Subjetiva

Expresa el grado en que una persona cree que ocurrirá un suceso

## Elementos de Teoría de Conjuntos

Considerando

- Espacio muestral: Conjunto de resultados posibles
- Punto muestral: Un resultado particular
- Evento: Subconjunto de resultados
- Evento Imposible: Probabilidad 0
- Evento Certeza: Probabilidad 1
- Evento Complemento: $\bar{E} = S - E$
- Union de Eventos: Para $E_1$ y $E_2$, la union corresponde a los evento de ambos juntos
- Intersección de Eventos: Para dos eventos, la intersección corresponde a los puntos que ocurren en ambos
- Eventos Mutuamente Excluyentes (Disjuntos): $E_1 \cap E_2 = \emptyset$
- Eventos Colectivamente Exhaustivos: $E_1 \cup E_2 = S$

### Operaciones

$$ \cup: union$$
$$ \cap: intersección$$
$$ \bar{E}: complemento \ de \ E$$

#### Ley de Morgan

$$ \overline{(E_1 \cup E_2)} = \bar{E_1} \cap \bar{E_2}$$
$$ \overline{(E_1 \cap E_2)} = \bar{E_1} \cup \bar{E_2}$$

## Matemática de la Probabilidad

### Axiomas

- $\forall E \in S, P(E) \geq 0$
- $P(S) = 1$
- Si $E_1$ y $E_2$ son mutuamente excluyentes $P(E_1 \cup E_2) = P(E_1)+P(E_2)$

### Ley Aditiva

Para cualquier dos eventos
$$E_1 \cup E_2 = P(E_1)+P(E_2) - P(E_1 \cap E_2)$$

### Métodos de Conteo

Cuando S es finito se puede determinar la probabilidad de un suceso sumando las probabilidad de cada punto que componen el suceso

#### Principio de Multiplicación

Si un experimento esta compuesto de k experimentos de tamaños muestrales $n_i$
$S = n_1 \times n_2 \times...\times n_k$

#### Permutación

Si C es un conjunta de objetos, de cuantas maneras podemos seleccion r objetos

- Con Reemplazo: n^r
- Sin Reemplazo $n \times (n-1) \times (n-2) ...$

#### Combinación

Muestreo sin remplazo sin importar el orden
$$\binom{n}{r} = \frac{n!}{r! \times (n-r)!}$$

### Probabilidad Condicional

La probabilidad de que $E_1$ocurra dado $E_2$
$$P(E_1 |E_2) = \frac{P(E_1 \cap E_2)}{P(E_2)}$$
$$P(\bar{E}_1|E_2) = 1- P(E_1|E_2)$$
**POR LO GENERAL** $P(E_1|E_2) \neq P(E_2|E_1)$

### Independencia Estadística

Si dos eventos son independientes, la ocurrencia de uno no afecta las probabilidades del otro

Si $E_1$ y $E_2$ son independientes
$$P(E_1 \cdot E_2) = P(E_1) \cdot P(E_2)$$

### Ley Multiplicativa

$$
P(E_1 \cap E_2 \cap E_3) = \left\{
\begin{array}{c l}
P(E_3 | E_1 \cap E_2) \cdot P(E_2|E_1) \cdot P(E_1)\\
P(E_1 \cap E_2 | E_3) \cdot P(E_3)
\end{array}
\right.
$$

La independencias de pares NO IMPLICA independencia total

### Teorema de Probabilidades Totales

Si $E_i$ son eventos colectivamente exhaustivos y mutuamente excluyentes:

$$P(A) = \Sigma^n_{i=1} P(A \cap E_i) = \Sigma^n_{i=1} P(A|E_i)P(E_i)$$

### Teorema de Bayes

$$P(A|E_j) \cdot P(E_j) = P(E_j|A) \cdot P(A)$$
$$P(E_j|A) = \frac{P(A|E_j) \cdot P(E_j)}{P(A)}$$

# Modelos analíticos de fenómenos aleatorios

## Variables Aleatorias y Distribución de Probabilidad

### Eventos y Variables Aleatorias

Una variable aleatoria es un vehículo matemática para representar un evento

Se pueden escribir eventos como
$$E_1 = (a < X \leq b)$$
$$E_2 = (c < X \leq d)$$

### Distribución de Probabilidades
Si X es una variable aleatorio, su probabilidad acumulada puede ser escrita como
$$F_X(x) = P(X \leq x) \ \forall x \in \mathbb{R}$$
Cuando X es discreta su probabilidad puntual se denota como
$$p_X(x) = P(X=x)$$
Por lo que
$$F_X(x) = \Sigma_{x_i \leq x} p_X(x_i)$$

Cuando X es continua
$$P(a < X \leq b) = \int_a^b f_X(x)dx$$
$$F_X(x) = P(X \leq x) = \int_{- \infty}^x f_x(t) dt$$
$$f_X(x)=\frac{d}{dx}F_X(x)$$


