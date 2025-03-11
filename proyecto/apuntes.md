# Desarrollo de Sistemas Complejos

## Cloud Computing

- Subir es gratis, bajar es costoso.
- Costos *cloud* vs *on-premise*: Se debe usar tecnologías *cloud* cuando la demanda no es uniforme.
- *Over-allocation*: Medida de cuántas CPU se están asignando respecto a cuántas realmente se están utilizando.

## Redes

- MPLS: Conexión por labels en vez de direcciones IP. Utilizado en sistemas grandes.
- Un sistema no se puede considerar seguro si tiene un punto único de fallo (*i.e.* centralización).
- Universalidad de la nube: Poder acceder desde cualquier proveedor
- Mesh != repetidor
  - Mesh (malla) es una conexión distribuida entre distintos routers. Es más seguro, ya que no es centralizado, por lo que ante la falla de un nodo, la conexión sigue estable.

## Ciberseguridad

- Los estándares se adaptan a las necesidades de los usuarios, ya que la adaptación no es fácil.
- Costos: Siempre se puede mejorar la seguridad. Se debe considerar si los datos son lo suficientemente críticos como para justificar la inversión.
  - Si no son extremadamente críticos (*e.g.* datos de una universidad), se debe invertir en seguridad, pero menos que si fueran datos extremadamente sensibles (*e.g.* datos de clínicas o bancos).
- Ingeniería social

## Complejidad computacional

- NP-completo vs NP-hard: (N. de la R. **NP significa Polinomial No-Determinista. En este punto, el profe está puro hueando**).
  - Problema NP:
    - Un problema NP es un problema que puede **resolverse** a través de una máquina de Turing no-determinista en tiempo polinomial y puede **evaluarse** a través de una máquina de Turing determinista en tiempo polinomial.
    - Más sencillamente, un programa NP es un problema difícil de resolver y fácil de verificar.
  - Problema NP-Hard: Todo problema NP se puede asociarse (*i.e.* reducirse) a un problema NP-Hard
  - Problema NP-Completo: Problema NP-hard que es a su vez NP.
  - Dada esta construcción, **todo problema NP-completo es NP-hard**, pero no necesariamente al revés.
- Paralelizar puede ser una gran ventaja, pero no todo es paralelizable.

## Gestión TI

- Comprar vs Desarrollar: Desarrollar es conveniente **solo si** eso genera una ventaja competitiva (*i.e.* Te otorga algo que los demás no pueden tener)
- Es importante identificar los cuatro prinicipios del manifiesto ágil en un proyecto de software.
  - **Individuos e interacciones** sobre procesos y herramientas: Se debe priorizar a las personas que forman parte del proyecto. Interactuando entre personas se obtienen cosas que no se logran mediante un correo.
  - **Software funcional** sobre documentación extensiva: Siempre hay que agregar valor al producto de software, incluso en las primeras iteraciones.
  - **Negociación con el cliente** sobre acuerdos contractuales: Los contratos generalmente no consideran todas las variables involucradas en un proyecto.
  - **Respuesta al cambio** sobre seguir un plan: Los proyectos naturalmente cambian. No responder a ello reduce la confianza del cliente.
