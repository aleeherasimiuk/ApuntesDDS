# Diseño, metodologías y arquitectura


## ¿Qué es diseñar?

- Modelar una solucion
- Plantear una solución en base a requerimientos
- ¿Qué componentes hay?
- ¿Qué responsabilidades van a tener?
- Puede ser de Sistemas: RRHH | Procesos | Software (Engloba a la organización)
- Teniendo en cuenta el contexto
  - Un Banco (cumple regulaciones)
  - Un avión (debe ser confiable y rápido)
  - Un negocio (tiene restricciones)


- Diseño de Software
  - Sobre esos componentes
  - Parte automatizada

- Arquitectura
  - Son las cosas más generales de cómo se implementa el sistema
    - Cuantas aplicaciones
    - ¿Mobile? ¿Desktop? ¿Web?
    - ¿Está dividido en subsistemas?
    - ¿Tengo una o varias BBDD?
    - Paradigma
    - Cómo se distribuyen los componentes a alto nivel
  
- Aspectos físicos
  - Servidor propio, o en la nube
  - Temas de red


- Aspectos lógicos
  - Dónde se implementan las reglas de negocio
  - Seguridad en todo el sistema


## Tres capas en las que se hará foco

- Presentación
  - UI o Interface para que interactúe con otro sistema
  
- Dominio
  - Se resuelven las reglas del negocio

- Persistencia
  - Guarda los datos del dominio


## Qué se define en cada **PARADIGMA**

- Estructurado
  - ¿Qué estructuras?
  - ¿Qué funciones?
  - ¿Cómo se invocan?

- Funcional
  - ¿Qué estructuras?
    - Data
    - Tupla
  - ¿Qué funciones?
  - ¿Cómo se invocan?

- Objetos
  - ¿Qué objetos?
  - ¿Qué mensajes?
  - ¿Cómo se conocen?


## Cualidades del *Software*

- Heurísticas para determinar qué tan bueno es el diseño
- Guía


## Enfoque iterativo/incremental

- Los sistemas evolucionan
- No *existe* el ciclo de vida en cascada
  - Es una metodología que no acepta errores
- Se piensan requerimientos de partes que necesitará el sistema y luego se irán añadiendo sobre eso, nuevas funcionalidades



## Preguntas

Q: ¿Cómo definirías Sistema?<br>
A: Conjunto de partes que se relacionan entre sí para lograr un objetivo común <br>

Q:¿Qué implica diseñar? <br>
A: Identificar, establecer relaciones y reconocer las responsabilidades de los componentes del  sistema<br>

Q:¿Cómo se relaciona el Diseño de Sistemas y el Diseño de Software? <br>
A: El diseño de Software está englobado dentro del de sistemas <br>

Q: ¿Existe un único diseño del sistema que sea mejor que todos? <br>
A: No <br>

Q: ¿Puedo estar diseñando sin hacer ningún diagrama? <br>
A: Si <br>

Q: ¿Diseñar una funcionalidad sin conocer la tecnología trae algún inconveniente? <br>
A: Si, ya que diseñar es tomar decisiones y descartar decisiones técnicas produce un diseño pobre <br>

Q: ¿El diseño implica detallar cada una de las decisiones? <br>
A: No, implica tomarlas.<br>
















