# Tareas programadas

- El paso del tiempo **NO ES UN EVENTO**
- Existen herramientas para ejecutar tareas cada cierto tiempo

Por ejemplo:

- Dentro del proceso (while / timers)
- Sistema operativo / plataforma

Ej: `0 * * * * java -jar /home/app/tarea.jar`

- Es deseable que sean idempotentes


```java
public class Vigía{

  public static void main(String[] args){
    System.out.println("Revisando si hay cuentas nuevas");

    RepositorioTransferencias.getInstance().pendientesAl(LocalDateTime.now())
      .forEach({
        it -> System.out.println("Realizando transferencia desde " + it.getUsuario() + " a " + it.getDestinatario()));
        it.ejecutar();
      });

  }

}
```