# Bibliotecas

- Abstracciones reutilizables
- Funcionalidad completa

Ejemplo:

```java
StringUtils.reverse(unString)
```


```java
File resizedImage = new Resizer(this)
      .setTargetLength(1080)
      .setQuality(80)
      .setOutputFormat("JPEG")
      .setOutputFilename("resized_image")
      .setOutputDirPath(storagePath)
      .setSourceImage(originalImage)
      .getResizedFile();
```

Este segundo ejemplo tiene una solución más acorde a la programación orientada a objetos.


## Control directo

Las bibliotecas son de control directo, es decir, yo utilizo la funcionalidad que ofrece.


# Framework

- El componente igualmente está hecho por un tercero
- Funcionalidad incompleta
- Provee un marco de trabajo
- Define estructura
- Dan punto de extensibilidad
- Yo debo completar la parte faltante con mi funcionalidad
- No resuelve problemas en particular
- Ejemplo: JUnit


## Control inverso

- El Framework me llama a mí.
- Sabe los lugares a donde debe llamar. Ej: `@Test`





