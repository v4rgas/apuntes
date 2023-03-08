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

## Modelación de fallo
1. Defino un $\Omega$ como el conjunto de todos los posibles valores que puedo poner como input
2. $F = 2^\Omega$
3. $P(E) = \frac{|E|}{|\Omega|}$

Luego busco la probabilidad de $P(E_{fail})$
