# Eventos y probabilidades

## Espacio de probabilidad $(\Omega, F,P)$

1. $\Omega$ es un conjunto de muestras
2. $F \subseteq 2^{\Omega}$ conjunto de eventos cerrado bajo $\cap, \cup$ y complemento (todos los posibles subconjuntos)
3. $P:F\rightarrow R$

## Axiomas

1. $\forall E \in F, 0\leq P(E) \leq 1$
2. $P(\Omega) = 1$
3. Para todo $E_i$ infinito, numerable y disjunto: $P(\bigcup_{i\geq1}E_i) = \sum_{i\geq1}P(E_i)$

## Lemas

1. Probabilidad del complemento
   $$P(E) = 1-P(\bar{E})$$
   $$P(\Omega) = 1 = P(E \cup \bar{E})$$
   $$P(E) + P(\bar{E})=1$$

2. Probabilidad de union
$$P(E_1 \cup E_2) = P(E_1) + P(E_2) - P(E_1 \cap E_2)$$
<!-- $$E_1 \cup E_2 = E_1/E_2 \cup E_2/E_1 \cup E_1\cap E_2$$ -->

3. Cota de unión
   $$P(\bigcup_{i\geq1}E_i) \leq \sum_{i\geq1}P(E_i)$$

4. Probabilidad condicional
   $$P(A|B) = \frac{P(A \cap B)}{P(B)}$$

Observación
$$P(A|B)$$
$$\Omega' = B$$
$$F' = \{A \cap B | A \in F\}$$

5. Suma de particiones

Sean ${E_1..E_n}$ particion de $\Omega$
$$P(E) = \sum_{i=1}^nP(E \cap E_i)$$
$$P(E) = \sum_{i=1}^nP(E) P(E|E_i)$$

## Modelación de fallo

1. Defino un $\Omega$ como el conjunto de todos los posibles valores que puedo poner como input
2. $F = 2^\Omega$
3. $P(E) = \frac{|E|}{|\Omega|}$

Luego busco la probabilidad de $P(E_{fail})$

## Verificacion de matrices

Sean A, B y C matrices de $n \times n$

input: A, B, C

output: True si $A \cdot B =_2 C$ (en modulo 2)

### Algoritmo determinista

Multiplico y comparo $O(n^{2.4})$
con el mejor algoritmo actual

### Algoritmo probabilistico

1. Genero $r \in \{0,1\}^n$
2. si $A \cdot (B \cdot r) =_2 C \cdot r$ return true else false

$O(n^2)$

## Decision aplazada

Analizar varios eventos aleatorios simultaneos es igual que analizar los eventos secuencialmente aplazado las decisiones futuras

# Min-Cut
- Input: $G(V,E)$ y $|v|\geq2$
- Output vertice de corte
## Algoritmo probabilistico

Mientras al cantidad de vertices sea mayor a dos, tomo uno y lo colapso

Probabilidad de error = $\frac{2}{|v|(|v|-1)}$

### Lema
Si el corte minimo es k, entonces $|E| \geq\frac{|V|k}{2}$

Para $v \in V: N(v) = \{u|\text{ es vecino de }v \}$
$$\forall v \in V: |N(v)|\geq k$$
$$\sum |N(v)| \geq k |v|$$
$$2 |E| \geq k |v|$$


### Cota importante
$$1-x \leq e^{-x}$$

# Variables aleatorias y esperanza
## Variables aleatorias
Sea $\Omega, F, P$ un espacio de probabilidades

Un variable alteatoria X es una función
$$X:\Omega \rightarrow R$$
$$(X=a) = \{s \in \Omega |X(s)=a\}$$
$$P(X=a) = \sum P(s)$$

## Esperanza
$$E[x] = \sum_i i P(X = i)$$
$$E[x] = \sum_{s \in \Omega} X(s)P(X=s)$$
$$E[cX] = cE[X]$$
### Linearidad
$$E[X+Y] = E[X] + E[Y]$$
### Desigualdad de Jensen
$$E[f(x)] \geq f(E[X])$$

## Independencia
Es independiente si y solo si
$$\forall a,b \in \mathbb{R}: P(X=a \cap Y=b) = P(X=a)P(Y=b)$$

# Bernoulli y Binomial
## Bernoulli
Es bernoulli si
$$\Omega = \{0,1\}$$
con $P(1) = p$

## Binomial
$$\Omega = \{0,1\}^n$$
$P(X=j) = \binom{n}{j}p^j(1-p)^{n-j}$

## Esperanza condicional
$$E(X|E) = \sum x P(X=x | E=)$$

$$E(X) = E(E(X|Y)) = \sum_i P(Y_i)E(X|Y_i)$$

### Lema

# Momentos y desviaciones
## Desigualdad de Markov $\forall a \in X: a > 0$
$$P(X \geq a) \leq \frac{E(X)}{a}$$

## Varianza
$$Var(X) = E((X-E(X))^2) = E(X^2) - E(X)^2$$
$$Var(X+Y) = Var(X)+Var(Y) + 2 Cov(X,Y)$$

## Covarianza
$$Cov(X,Y) = E((X-E(X))(Y-E(Y)))= E(X \cdot Y)-E(X)E(Y)$$

Si dos variables son independientes, COV(X,Y)= 0
$$E(X \cdot Y) = E(X)E(Y) $$

## Desigualdad de Chebyshev para a>0
$$Pr(|X-E(X)|\geq a) \leq \frac{Var(X)}{a^2}$$
$$Pr((X-E(X))^2\geq a^2) \leq \frac{Var(X)}{a^2}$$


# #DNF (pag 302)
$$\bigvee \bigwedge p_i$$

- Input formula en DNF
- Valuaciones que hace verdadero

## Algoritmo de aproximacion
Dado un $\epsilon, \delta$ encuentro $\bar{V}$
$$P(|\bar{V}-V|\geq \epsilon V) \leq \delta$$
en un tiempo polinomial en
$|\delta|, \frac{1}{e},\frac{1}{\delta}$

## Estimador no sesgado
La estrategia es un estimador no sesgado

Sea V el valor real
Sea $\Omega, F, P$ un espacio de probabilidades
X es un estimador no sesgado de V si
$$E(X) = V$$

$$Y = \frac{\sum_{i=1}^kX_i}{k}$$
$$Var(Y) = \frac{Var(X)}{k}$$
$$P(|Y-V| \geq \epsilon \cdot V)\leq \frac{1}{\epsilon^2 k}(\frac{E(X^2)}{E(X)^2}-1)$$
Quiero minimizar el segundo termino

## Primero estimador
$$X(\sigma) = 2^n \text{ si evaluacion es 1, sino 0}$$
$$E(X) = \sum_{\sigma} X(\sigma)P(\sigma) = \text{\#DNF}$$
