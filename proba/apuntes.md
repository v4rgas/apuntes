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

## Medidas Descriptivas de Variables Aleatorias

### Valores Centrales

Valor medio/esperado:
$$E(X) = u_x = \int_{-\infty}^{\infty} x \cdot f_X(x) dx$$
o el promedio ponderado en el caso discreto

Moda: Valor más frecuente o con mayor probabilidad

Mediana: $F_X(x)=\frac{1}{2}$

Si la distribución es simétrica las tres medidas son parecidas

### Medidas de Posición

Percentil es el $x_p$ tal que
$$F_X(x_p)=p$$

### Función Generadora de Momentos

$$M_X(t) = E [ exp(tX) ]$$

### Medidas de Dispersion

Varianza: $Var(X) = E [ (X-u_x)^2 ]$

Desviación Estandar: $\sqrt{Var(X)}$

Coeficiente de variación: $\delta_X=\frac{\sigma}{u}$

Rango: $max-min$

IQR: $x_{0.75}-x_{0.25}$

### Medidas de asimetría

Skewness: $E[(X-u_x)^3]$

Coeficiente de asimetría: $\theta_X = \frac{Skewness}{desviación^3}$

Kurtosis: $E[(X-u_x)^4]$

Coeficiente de curtosis: $K_X = \frac{Kurtosis}{desviación^4}-3$

## Distribuciones de Probabilidad

### Distribución normal

La función densidad de Normal($\mu, \sigma$)
$$f_X(x) = \frac{1}{\sqrt{2 \pi \sigma^2}}exp\{ -\frac{1}{2}(\frac{x-\mu}{\sigma})^2\}$$

$\mu$ hace que la función se desplace en el eje x

$\sigma$ achata o levanta la curva

Cuando $\mu=0, \sigma=1$ corresponde a la Normal Estándar

Su acumulada se denota como
$$\phi(s) = F_S(s) = \int_{-\infty}^s\frac{1}{\sqrt{2 \pi}} e^{-x^2/2} dx$$
$$\phi(-s) = 1 - \phi(s)$$

Generalizando
$$F_X(x) = \phi(\frac{x-\mu}{\sigma})$$
para cualquier normal

Media, mediana y moda son iguales en está distribución

### Distribución Log-Normal

$$f_X(x)=\frac{1}{\sqrt{2 \pi}(\zeta x)} exp(-\frac{1}{2}(\frac{ln(x)-\lambda}{\zeta})^2)$$
Donde
$$\lambda = E(ln(X))$$
$$\zeta = \sqrt{Var(ln(X))}$$

Importante notar que

Mediana = $exp(\lambda r)$

$$P(X \leq x) = F_X(x) = \phi(\frac{ln(x)-\lambda}{\zeta})$$

$$E(X^z) = e^{\lambda z}M_y(z \zeta)$$

Hasta 0.3, la $COV = \zeta$

### Distribución Binomial

Probabilidad de cierta cantidad de exitos dada cantidad de intentos y una probabilidad

n: numero de experimentos

p: probabilidad de suceso

x: cantidad de ocurrencias

$$p_X(x) = \binom{n}{x} p^x(1-p)^{n-x}$$
$$F_X(x) = \sum p(x)$$
$$E(X) = np$$

### Distribución Geométrica

La probabilida un cierto numero de experimentos hasta el primer exito dada una probabilidad
$$P(N=n) = p(1-p)^{n-1}$$
$$E(N) = \frac{1}{p}$$

### Distribución Binomial Negativa

Probabilidad de que despues de una cierta cantidad de experimentos ocurran un numero dado de exitos con una probabilidad

$$P(T_k = x) = \binom{x-1}{k-1}p^k (1-p)^{x-k}$$
$$E(T_k) = \frac{k}{p}$$
$$Var(T_k) = \frac{k (1-p)}{p^2}$$

### Distribución de Poisson

La probabilida de una cierta cantidad de exitos dada una frencuencia media y una cantida de tiempo

v: Frecuencia media

t: Tiempo transcurrido

$$P(X_t = x) = \frac{(vt)^x e^{-vt}}{x!}$$

$$\lambda = vt$$

$$E(X) = \lambda$$

### Distribución Exponencial

Probabilidad de un cierto tiempo entre eventos Poisson

La probabilidad de un cierto tiempo hasta que la primera ocurrencia de un evento es equivalente a la probabildiad del evento no pasando hasta t en un proceso Poisson

$$P(T_1>t) = P(X_t = 0) = e^{-vt}$$
Con $X_t - Poisson(vt)$

Por lo que

$$F_T(t) = P(T_1 \leq t) = 1 - P(T_1 > t) = 1-e^{-vt}$$
$$f_{T_1} = v e^{-vt}$$

$$u_x = \frac{1}{v}, \sigma^2_X=\frac{1}{v^2}$$

si se encuentra desplazo en a entonces
$$u_x = \frac{1}{v} + a, \sigma^2_X=\frac{1}{v^2}$$
$$t \rightarrow x-a$$

### Distribución Gamma

La probabilida de un cierto tiempo hasta la ocurrencia de una dada cantidad de eventos puede ser descrita con Gamma

$$f_X(x) = \frac{v^k}{\Gamma(k)} x^{k-1} e^{-vk}$$
con
$$\Gamma(\alpha) = \int_0^{\infty} u^{\alpha - 1} e^{-u }du$$

### Distribución Hipergeométrica

Probabilidad de que una cantidad de elementos pertenezcan a cierta categoria dada una cantidad total de elementos y un cierto tamaño de muestra (muestreo sin reemplazo)

N: cantidad total de elementos

m: numero de elementos en cierta categoría

n: numero de muestras tomadas

x: cantidad de la muestra que pertenece a la categoria

$$p_X(x) = \frac{\binom{m}{x} \binom{N-m}{n-x}}{\binom{N}{n}}$$

## Multiples Variables Aleatorias

## Esperanza Condicional y Predicción

$$
E(Y|X=x)  = \left \{
    \begin{array}{c l}
    \sum y \cdot P(Y=y|X=x)\\
    \int y \cdot f_{Y|X=x}(y)dy
    \end{array}
    \right.


$$

$$
P(E_1 \cap E_2 \cap E_3) = \left\{
\begin{array}{c l}
P(E_3 | E_1 \cap E_2) \cdot P(E_2|E_1) \cdot P(E_1)\\
P(E_1 \cap E_2 | E_3) \cdot P(E_3)
\end{array}
\right.
$$

## Derivación de Distribuciones de Probabilidad
### Funciones de Variable Aleatorias
Si g es una funcion de una variable aleatoria X
$$Y = g(X)$$
entonces
$$P(Y=y) = P(X=g^{-1}(y))$$

$$f_Y(y) = f_x(g^{-1}(y)) \cdot (\frac{d}{dy} g^{-1}(y))$$

Si $g^{-1}$ no tiene solucion unica, entonces se deben super los $f_Y$ para todas las posibles soluciones

### Funciones de Multiples Variables Aleatorias
$$Z = g(X,Y)$$

## Suma de Variables Aleatorias Discretas

Si X e Y son dos variables aleatorias discretas, $Z=X+Y$
$$p_Z= \sum_{x+y=z} p_{x,y}(x,y) = \sum_{x} p_{x,y}(x,z-x)$$

Si son independientes
$$p_Z = p_X \cdot p_Y$$

### Suma de Variables Aleatorias Continuas

$$f_Z(z) = \int_{-\infty}^{\infty} f_{X,Y}(z-y,y)dy$$
$$f_Z(z) = \int_{-\infty}^{\infty} f_{X,Y}(x,z-x)dx$$
$$f_Z = f_X \cdot f_Y$$

### Suma de normales

$$X \pm Y = Normal(u_x \pm u_y, \sqrt{\sigma_x^2 + \sigma_y^2})$$


### Máximo estimador verosimil
