# Cualidades del Diseño

- Sirven como heurísticas para decidir
- Sirven como argumento para justificar un diseño
- Tensión entre cualidades
  - Favorecer una cualidad, puede perjudicar a otra.
  - Síndrome de la frazada corta


## Acoplamiento

- Nivel de conocimiento entre componentes
- No es deseado ni el acoplamiento 0 ni muy alto
- Buscamos el necesario para funcionar


## Cohesión

- Si su objetivo es claro y no tiene responsabilidades *extra*


## Simplicidad

- Cuán fácil se entiende el diseño
- Hay complejidad inherente al problema y complejidad del diseño
- Ataca la complejidad del diseño.
- **KISS**: Keep it simple. Stupid!
- **YAGNI**: You aren't gonna need it


## Robustez

- ¿Qué pasa cuando el sistema no se usa de la forma más correcta?
- ¿Qué pasa si hay algún error?
- No sentir que la aplicación se *rompió*
- Manejo y tratamiento de errores


## Testeabilidad

- ¿Qué tan fácil es testear la funcionalidad de forma unitaria?


## Flexibilidad

- Extensibilidad
  - ¿Qué tan fácil es ageregar nuevas funcionalidades al sistema?
  
- Mantenibliidad
  - ¿Qué tan fácil es arreglar problemas?

## Redundancia mínima

- No repetir lógica
- No duplicar abstracciones
- DRY: Don't repeat yourself

## Consistencia

- Ante problemas simlares tomar soluciones similares


## Mutaciones controladas

- Tratar de controlar y tener bien definidos los cambios del sistema
- Setters controlados
- No variables globales (o tener métodos que los modifiquen)
- A menos mutaciones es más sencillo testear


## Escalabilidad

- Soportar más operaciones sin degradar la eficiencia del sistema
























