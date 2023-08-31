# Definiciones
## Arquitectura de sistema de software
Set de estructuras requeridas para razonar acerca de un sistema
## Analogia a la construcción
- Se utilizan patrones y estilos ampliamente probados y usados
- Reconoce anti-patrones y reduce posibilida dde error
- Se enfoca en antender necesidades del futuro usuario
- Requiere comunicarse con un equipo amplio
- No basta ocn saber programar (o construir)

## Arquitecto
- Toma decisiones de grano grueso
- Liga requiriemintos a propuestas arquitectonicas
- Piensa en trade-offfs
- Mantiene a todos involucrados en su vision
- Programa

## Cohesion
Partes que deben ir juntas en un mismo modulo tienen buena cohesión

Connascence existe si el cambio de un componente requiere un cambio en el otro para que el sistema funcione

## Acomplamiento
Grado de interdependencia de dos o más compoenentes de software

### Tipos
- Contenido
- Comun
- Datos
- Control 
- Stamp 

## Componentes
- Encapsulan un subconjutno de funcionalidad
- Altamente cohesionado con connascense interna
- Expone interfaces explicitas

Es una agrupación logica de software

## Unidad o quanta
Un artefacto independiente el cual se puede ejecutar sola
- Cohesion funcional alta
- Tiene todo lo necesario para funcionar
- Agrupación mayor de componentes
- Deployable por si misma

## Documentando la arquitectura
Parte del rol de comunicador implica documentar a varios niveles

- Vistas
- Codigo
- Comentarios
- Informes
- ADR

## Vistas
Coleccion de hechos de un sistema representado de manera grafica

- 4+1
- C4
- UML

# Atributos de calidad
## Metodos de identificación de componentes
- Modelo actor/accion (desde RUP)
    - usuario x hace y con rol z
    - ayuda a capturar requisitos
    - muy cercano a historia de usuario
- Event Storming
    - Que eventos gobiernan la app
    - Ejemplo en proyecto
    - Buen alcance de complejidad

## Stakeholder
- Peticiones ilimitadas
- Poca o nula consideracion por otras areas
- Vistas parciales del negocio

## Atributos de calidad (NFR)
- Son resultados de decidiones arquitectonicas
- Requisitos no funcionales, "valores"
- Atributos no tecnicos

### Estructural
- Se manifiestan durante el desarrollo del codigo
- Deuda tenica puede afectar mucho

Ejemplos:
- Mantenibilidad
- Flexibilidad
- Extensibilidad
- Integrabilidad

### Operacional
- Influciencian runtime
- Se manifiestan con la operación del sistema

Ejemplos:
- Escalabilidad
- Confiabilidad
- Performance

### Transversal
- Seguridad
- Usabilidad
- Soporte

# Atributos de calidad transversales
- Involucran varias partes de un sistema a varios niveles
- Son mas dificiles de clasificar
- Incorporan varios elementos de dominio

## Elasticidad
Capacidad de responder a la demanda operacional y obtener recurso de acuerdo a necesidades

Requiere:
- Estrucutra orientada
- Capacidad operacional
- Enterno compatible

## Seguridad
- Completamente transversal

Conceptos claves
- Autorización/Autenticación
- AC
- IAM
- Minimo privilegio
- Defensa en profundidad
- Fail-safe
- Separación de responsabilidades
- Diseño abierto

## Reusabilidad
- Capacidad de un sistema/componente de ocuparse de otros sistemas
- Funcionalidad compartida
- Exceso de reusabilidad puede provocar fracaso arquitectonico

## Soporte (muy general)
- Capacidad de responder a requisitos
- Mediado por SLA, Tickets/Hora

## Eficiencia (muy general)
- 

## Usabilidad (muy general)
- Nivel de entrenamiento requerido por usuarios para lograr sus metas

## Diagramación
- Forma de transmision de conocimiento
- Tiene un relato de funcionamiento
- Debe estar enfacado en una audiencia especifica

### Modelo
- Representación abstracta de un sistema

### Meta-modelo
- Modelo de modelos
- Permiten definir lenguajes de modelación

### Tipos de diagramación
#### 4+1
- Vista logica
- Vista de desarollo
- Vista de proceso
- Vista fisica: Como se instala el sistema, como se ejecuta y diagramas de despliegue

#### C4
- Menos estrucutrado que UML

Se divide en niveles
1. Context
2. Containers
3. Components
4. Code

#### UML
Existen muchos tipos de diagramas UML, entre ellos estan

- De Componentes:
    - Cada componente posee un estereotipo que lo caracteriza
    - Existen interfaces que muestran quien consume a quien
    - Existen conectores de delegacion que descomponen una funcionalidad

### Metodos clave de diagramación
- Inductivo: detalle a lo general
- Deductivo: general al detalle
- Referencia: Existen componentes reusables parecidos

# Introducción a los patrones de diseño
## Estilos
- Coleccion de decisiones de diseño arquitectonico
### Monolito
- Sisetma contenido en una sola quanta
- Bloque de sofware, base de datos
### Estilos comunes
#### Influenciados por el lenguaje
- Main y subrutinas
    - Subrutinas individuales y encapsuladas
    - Main cede el control a estas subrutinas
    - Datos: parametros de llamada de funcion
    - Topologia: Centralizado en rutina main: ida y vuelta
    - Modularidad: Si se respeta la interfaz se puede modifica la subrutina
    - Poca extensiblidad y escalabilidad, posibilidad de bloqueo, performance limitada

- Estilo de capas
    - Capas fuertemente cerradas y ordenadas
    - Cada capa encapsula una funcionalidad
    - Datos: Parametros y paquetes
    - Topologia: Lineal
