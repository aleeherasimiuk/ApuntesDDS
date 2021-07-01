# Manejo de Errores

- Ante un error se puede propagar, para que lo maneje el método que lo invocó.
"No hacer nada"

- Pueden tratarse, "atrapar" el error.

- Pueden evitarse mediante alguna condición

- Ocultarlo. No suele ser una buena idea


## Fail fast

Intentar fallar lo antes posible y estar cerca de la causa.

----

```java

if (fileExists(path)){
  f = openFile(path);
} else {
  f = createFile(path);
}

f.write("bla");

```

En esta solución, existe *FAIL FAST* ya que si el archivo no existe, `f` no queda en `null` deviniendo luego en `NullPointerException`.
También se está evitando lanzar la excepción y no es necesario `try/catch`

---

## Call & Return


- Se retornan valores indicando el estado
  - Ej: 0 OK. 1 ERROR.
  - Verificar que no coincidan con la *imagen* de la función
    - ¿Cómo avisar una división por 0?
      - 0 No se puede porque una división puede dar cero, o cualquier otro número

```c
FILE* pf;

int errnum;

pf = fopen("unexist.txt", "rb");

if(pf == NULL){
  errnum = errno;
  fprintf(stderr, "Value of errno: %d\n", errno);
  fprintf(stderr, "Error opnening file: %s\n, strerror(errnum)");

  return errno;

} else {
  fclose(pf);
}

return 0;
```

## Excepciones

- Simples de usar
- Se propagan abortando el flujo de ejecución
- Tienen
  - Stacktrace
  - Mensaje
  - Tipo


## ¿En quién confiamos?

- Se confía en lo interno
- Se desconfía en aquello que interactúa con usuarios u otros sistemas





