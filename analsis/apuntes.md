# Analisis de eficiencia de un algoritmo
## Teorema Maestro
$$T(n) = c, n=0$$
$$= a \cdot T(\lfloor n/b \rfloor) + f(n), n \geq 1$$

Se tiene que

$$f(n) \in O(n^{log_b(a)-\epsilon}), T(n) \in \Theta(n^{log_b(a)})$$

$$f(n) \in \Theta(n^{log_b(a)}), T(n) \in \Theta(n^{log_b(a)}log_2(n))$$

$$f(n) \in \Omega(n^{log_b(a)+\epsilon}), T(n) \in \Theta(f(n))$$
