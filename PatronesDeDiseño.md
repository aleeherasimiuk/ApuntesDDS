# Patrones de Diseño

## Strategy

- Familia de algoritmos
- Cada algoritmo tiene su clase (o encapsulado)
- Se pueden usar e intercambiar de manera indistinta

<br>
<div align = center>

![img](http://www.plantuml.com/plantuml/png/ZP0nhi8m341tdy9Z__1x15GXmOIWLx0IWoor2JW92-Bkf0qATA9R_EpdsEv298rf7C0C50BsoixI0n2loaRybWBdW7EPzPKsV_0441TPpsGOTtFwO-t5qT2KzqdbCeMpKZuv9hxJwkfU_UXpoas6vM0Ik-ZNsWUv9R5K_9zEMH__D--LO1uLszHMyjLmmUkO2zhbVlcwBm00)
</div>

En donde 

```
Nueva>>precioFinal(precioOriginal){
  return precioOriginal
}


Promocion>>precioFinal(precioOriginal){
  return precioOriginal - descuento
}


Liquidación>>precioFinal(precioOriginal){
  return precioOriginal * 50%
}
```


En este caso, una `Prenda` puede cambiar de `Estado`, puede pasar por ejemplo de `Nueva` a `Promoción`, o de `Nueva` a `Liquidación`.

Si se usase herencia, ese cambio no sería posible, pues una `Prenda` que hereda de `Nueva`, es más complicado que pase a ser una `Prenda` en `Liquidación`.

Alguien debe *"manualmente"* cambiar el `Estado` de la `Prenda`

Para comportamiento compartido también se puede usar, por ejemplo Clases Abstractas.

En el ejemplo:

- Context: Prenda
- Strategy: Estado
- ConcreteStrategy
  - Nueva
  - Promocion
  - Liquidacion

## Template Method

- Se define el *Esqueleto*, el algoritmo, en una Superclase y cada subclase define algunas partes específicas, pero sin cambiar la estructura general.

<div align = center>

![img](http://www.plantuml.com/plantuml/png/ROyn2i9044NxESMMMkGA4WYvWTYFauci93Co-sEZtbsZ5hBOFXxlyzi23YppH7mi21QaWnhuHYdb-U81tMpfGVIclVMZ2lBLKxCqzzt79Pcub5GPQo5KE4x-K-ZSzXnd1VyfLCPQ4DSnIrQ3VkaAsP077hh5mQNQQTM6rvyd)

</div>

```

Venta>> importe(){
  // Otras cosas
  return recargo(items.sum(item -> item.importe()))
}

VentaConTarjeta>> recargo(){
  return precioBase * (coeficienteTarjeta * cantidadCuotas + 1.01)
}

VentaEnEfectivo>> recargo(){
  return precioBase
}

```

¿Es solo una herencia?

El algoritmo general está en `Venta`, pero el recargo lo define cada una de las subclases.

- Paso concreto: `items.sum(item -> item.importe())`
- Hook method: `recargo`


¿Por qué no es un Strategy?

Una venta en efectivo no puede *cambiar* para ser una venta con tarjeta.

# Patrones creacionales

## Problemas al construir objetos

- Asociar sus dependencias
  - Pueden ser varias
  - Pueden relacionarse entre ellas
  - Consistentes entre sí
- Validaciones
  - No nulos
  - Consistencia
  - Muchos parámetros => Muchas validaciones
- Creación por partes
  - Tengo algunas dependencias pero no todas
  - Si todavía no las tengo, o dependen de la clase que estoy construyendo
  - El constructor debería recibir sólo las dependencias que son necesarias para funcionar
  - Problemas al tener múltiples constructores
  - El constructor debe construir un objeto válido
- Valores predefinidos
- Familias de objetos
  - Varios objetos que forman un todo.
    - Uniforme
      - Parte superior
      - Parte inferior
      - Calzado
    - Debe ser consistente entre sí, (mismos rasgos, mismo "objetivo").
    - Semánticamente debería instanciarlos juntos
- Grafos complejos
  - Objetos que necesitan objetos que necesitan objetos...
  - Gran árbol de dependencias
  - Dependencias circulares

---

## Diagrama Base

<br>
<div align = center>

![img](http://www.plantuml.com/plantuml/png/RP11JyGW48Nl_0gEUjZ-1BE4LhD9BKsxNhmOGed1GDbCmOF6_-vWxTIOFGryx_7cjSSbQKWvU6Rd7FYZJXQSzf-CSz4-g5frU6f59t3OPAggOb-C6Q-NXVTWvqaNQ_XzXiVaBd4LoWLqADD47w47DnjtqNu3epKPVU0YAmWpNYEctDpkAywOLbjQzC-qTfORAuPU3EVcPNpgc-6a1WHIjC-YLYITkljD4b8seyBBk2-KfLW9WlWMSRnRc2nE_P5zSQrL-C4VxkXd4GI4VnkJCA7mato5q0FYz47iY307_mq0)

</div>

<br>

---


## Builder

Se trata de un *"Borrador"* en donde se resuelven, por ejemplo las siguientes cuestiones:

- Creación por partes
- Validaciones
- Evitar inconsistencia
- Favorecer tener objetos completamente construidos


### **SEPARA LA CONSTRUCCIÓN DE UN OBJETO COMPLEJO EN SU REPRESENTACIÓN**


### Opción 1: Replicar estado y luego construir el objeto (Estado en borrador)

<div align = center>

![img](http://www.plantuml.com/plantuml/png/TOv12W8n34NtFSKiExSOT2q8pWLYMY6GJahQhiIxss07KNJJ9lcFl2obWjQMs253ATD1L268dZLk0UvkcXBKg0JSoqfPNe4r2ib-53QSDgGCl0yk4LldbCJrKImv8jq8GLVAuj-Bvi_qD6H-jtxbno5BjjcZTR-8OV_7h7Qwh_PQSct4mS_kuPP-Kaly1000)

</div>

```
BorradorPrenda >> especificarTipo(tipo){
  if(tipo == null) throw new TipoNoPuedeSerNuloException()
  
  this.tipo = tipo
}
...

BorradorPrenda>> build(){
  prenda = new Prenda(tipo, colorPrimario, trama, material)
  if(colorSecundario)
    prenda.setColorSecundario(colorSecundario)

}

```

En esta opción queda atada la implementación de prenda a la implementación del borrador.<br>
También debe repetirse todo el estado y puede ser mucho código repetido<br>
La prenda se mantiene inmutable

### Opción 2: Instanciar la prenda y luego setear los atributos directamente (Estado en prenda)

<div align = center>

![img](http://www.plantuml.com/plantuml/png/TK_1gi8m4BpdAt9C3-yN7aNFWc1_ODr6MKYQi4bEuh-xsrPAHI-RsSmmEzEN2bnR30O3b68FYHd6n6VsqmWFOssUaH7aI_P8DqBWWN9oLSQYw_Ri2QfdWk3Y2ZxuOmfwWI8m9OUwVLDSF3On_wKP62AugbEPQwCmkiuyZTcNi7__Ta5hnwtREv_0hOmkOgelha4df3ohS9_Bw9Tut9EupBTxSMWnl000)

</div>

```
BorradorPrenda >> especificarTipo(tipo){
  if(tipo == null) throw new TipoNoPuedeSerNuloException()
  
  this.prenda.especificarTipo(tipo)
}

...

BorradorPrenda >> build(){
  if(!prenda.esValida()) throw new PrendaInválidaException()

  return prenda
}

```

En este caso la prenda no es accesible desde fuera, pero debe permitirse que la Prenda pueda ser construida por partes, y esto puede dar lugar a prendas no válidas.<br>
La prenda debería usarse SOLAMENTE a través del borrador.


### Semántica

- **Abstraer** la **instanciación** de un objeto
- Separa la construcción de un objeto complejo en su representación
- Le podemos asignar más responsabilidades que al constructor se iba a dificultar
- Permite valores por defecto


### Construcciones por Default

```java
class BuilderComida{

  var ingredientes = []

  void cuartoDeLibra(){
    this.agregarPan()
    this.agregarHamburguesa()
    this.agregarQueso()
    this.agregarPan()
  }

  Comida build(){
    return new Comida(ingredientes)
  }
}


BuilderComida builder = new BuilderComida()
builder.cuartoDeLibra() // Abstraigo la construcción
Comida comida = builder.build()
```

### Interfaz fluida

```java

Comida comida = 
  new BuilderComida()
    .agregarPan()
    .agregarHamburquesa()
    .agregarQueso()
    .agregarPan()
    .build()

```

Cada método retorna `this`.

## Factory Method

- Define una interfaz para crear un objeto, pero deja a las `subclases` decidir qué clase concreta instanciar
- Caso puntual de Template Method: Delega construcción
- Da un punto de extensión para definir la clase a instanciar
- No es lo mismo que Creation Method (que es un método de clase)
- Gasto la herencia por un `new`. Analizar otras opciones


<div align=center>

![img](http://www.plantuml.com/plantuml/png/hOv13e8m44NtFSKiTS4L28ahMPaG3_214adAr4od6wjtbqHG2R9pr_uty-ONGI4Q1sTG1nKqIXN6Xqs6g4CjVGCvSzzv6Unk_nMU86ghUBNIUrcJ8tThSe2xeVLZzZ3cTOUod6Q_QHn2U7_JPwe2bn5CMSpk-THmGgM_S52_OBDfiPp-LtwXdmiaiwdVjF-K5vRREBWd)

</div>

```java

abstract class Sastre{

  Uniforme fabricarUniforme(){ // Puede tener más lógica
    return new Uniforme(
      this.fabricarParteSuperior(),
      this.fabricarParteInferior(),
      this.fabricarCalzado()
    );
  }

  abstract fabricarParteSuperior();
  abstract fabricarParteInferior();
  abstract fabricarParteCalzado();

}

class SastreSanJuan extends Sastre{

  Prenda fabricarParteSuperior(){
    borrador = new Borrador(CHOMBA);
    borrador.especificarColor(...);
    borrador.especificarMaterial(PIQUÉ);
    return borrador.build();
  }

  Prenda fabricarParteInferior(){
    borrador = new Borrador(PANTALON);
    borrador.especificarColor(...);
    borrador.especificarMaterial(ACETATO);
    return borrador.build();
  }

  ...
}

```

## Abstract Factory

- Define una interfaz que ofrece objetos de una misma familia que *deberían* tener que ver entre sí.
- Pueden utilizarlo distintos objetos
- Favorece extensibilidad para agregar nuevas familias
- Utiliza composición (No gasta la herencia)


<div align = center>

![img](http://www.plantuml.com/plantuml/png/hOyn3i8m34Ntd29Z6UWHgafCT4AgE81f7H4fTP3jB5JSdGggg0MwiIL--k_RjIYmfY4OJuhie4FRWYZZQAnrS67V0P-05TjqG_QHYzqdEPBx9WS8T-ZZD7iOyrRQVMNFv5ta0KqNG2H_fxRypef2Nh65eGLg4f0jKrdDUywaYLSIZIiqTwoYVpmhwvVzezdKRzf_ockaFWtX2G00)
</div>

```java
class Uniforme{
  static fabricar(Sastre sastre){
    return new Uniforme(
      sastre.fabricarParteSuperior(),
      sastre.fabricarParteInferior(),
      sastre.fabricarCalzado()
    );
  }
}

Uniforme uniforme = Uniforme.fabricar(sastreSanJuan);
```

Al sastre le pido la familia de objetos consistente.
El Sastre del colegio San Juan me construye la parte superior del San Juan, la parte inferior del San Juan y también el calzado de San Juan
Se pueden utilizar los métodos en otros contextos que no tengan que ver con la creación de un uniforme.


### Factory Method vs Abstract Factory

- Factory Method lo delega a sus subclases y el Abstract Factory a otras clases de otra jerarquía
- Abstract Factory trabaja con una familia de objetos, por eso se separa en un objeto aparte
- Factory Method trabaja sobre una instancia específica que puede llegar a variar, y lo resuelve con subclases
- En abstract factory no se mezcla la creación con el uso.


## Singleton

- Se usa cuando *no queda otra*
- Es necesario que haya un único punto de acceso.
  - Por ejemplo semáforos, registro de clientes, etc
- ESTADO GLOBAL!!!
  - Analizar testeabilidad.
  - Debe reiniciarse el estado global antes de cada test.

```java
class DragonQueConcedeDeseos{

  static final INSTANCE = new DragonQueConcedeDeseos();

  static DragonQueConcedeDeseos getInstance(){
    return this.INSTANCE;
  }
}
```
## Otros

- Hay más patrones creacionales como Prototype.




---

## Facade

- Ocultar detalles a otro subsistema u otros componentes
- El sistema externo no conoce internamente al sistema.
- No caer en *Man in the middle*

```java
// Man in the middle. No oculta nada
public void inactivar(Cliente cliente){
  cliente.inactivar()
}
```

- Usar clases abstractas o interfaces no implican un *Facade*



## Adapter

- Tenemos un componente externo
- No lo podemos modificar
- Nosotros solo modelamos la interfaz saliente

<div align=center>

![img](http://www.plantuml.com/plantuml/png/NOw_IiTG38NtF4N65lGDIcdfug0eA3Z7t5WZzq_9pHL417VVqYSHMntyZScNdEzCrScyfGWSgLQyiV8Y4eejKHEz0I2kudUIDZ7oPjJrn-gY9GaK_iQbvA2i9KlTFrsPQjVZrySdl0F0_rmy7t5cRlT2_YGsRmgQNUoHM4x0Nk5IgmzNj0stRfhNCxn-U1RsSVnH_zvs1qDMV-rXbqwIGqln6m00)
</div>

- Interfaz de AccuWeatherApi con muchos smells, como por ejemplo obsesión con los primitivos.
- Lo adapto mediante ProveedorClima

```java
class ProveedorClima{

  int getTemperatura(){
    AccuWeatherAPI apiClima = new AccuWeatherAPI();
    List<Map<String, Object>> condicionesClimáticas = apiClima.getWeather("Buenos Aires, Argentina");

    return condicionesClimaticas.get(0).get("Temperature").get("Value").toDegrees();
  }

}

```

Finalmente se pueden hacer adapters para cada API:

<div align=center>

![img](http://www.plantuml.com/plantuml/png/VL3DIWD13BxFK-Iu2zedA4DBBmgA1GNFCHDhP6TcoMG4AGNllgLFuluew2BcbkJxIRvDCLIhiNFKOIcmr_p27BBkEebuT0xWm7R7iMT5gufDEuykQkI0uRlTvHI492Sk4zE4i3GjVBozkvCRIfAnFY8nR8dgFQCziMvxcIokfDZw6llrhQcamndSF3mpcVUZz1UscNT0Og-j6qL_t_fJTdy9vtvXz3zyLLT-yVnVYInenvFRS0f3V9y0)

</div>


### Conclusiones

- Adapta interfaces que en principio no son compatibles
- Quiero interactuar con clases imprevistas, que muy probablemente sus interfaces sean distintas

### En el diagrama
- Cliente: Código que llama al adapter
- Adaptee: API
- Target: ProveedorClima
- Adapter: ProveedorClimaAccuWeatherAPI


## Patrón Command | Reificar comportamiento

- Diferir operaciones en el tiempo
- Modelar un objeto que representa una operación
- La creación y la evaluación se hacen en dos momentos distintos
- Ahora el agregarPrenda o quitarPrenda pasaron a ser un objeto
- Cuando genero una sugerencia, en lugar de llamar al método agregarPrenda, creo un objeto que representa el agregar, que se aplicaría o no en otro momento.
- Usar lambdas me pueden hacer perder expresividad (Tipan como Consumer, Function, etc). Pierdo la abstracción que me representa la operación de mi dominio. Son más dificiles de persistir. Y no puedo agregar más métodos.

```java
abstract class Sugerencia{

  EstadoSugerencia estado;
  Prenda prenda;


  void aplicarEnGuadarropas(Guardarropas guardarropas){
    this.estado = ACEPTADA;
    this.aplicarOperacion(Guardarropas guardarropas);
  }

  void deshacer(Guardarropas guardarropas){
    Validate.is(this.estado, ACEPTADA);
    this.deshacerOperacion(guardarropas)
  }

  abstract void aplicarOperacion(Guardarropas guardarropas);
  abstract void deshacerOperacion(Guardarropas guardarropas);


}

class SugerenciaAgregar extends Sugerencia{

  void aplicarOperacion(Guardarropas guardarropas){
    guardarropas.agregarSugerencia(prenda);
  }

  void deshacer(Guardarropas guardarropas){
    guardarropas.quitarSugerencia(prenda);
  }

}

class SugerenciaQuitar extends Sugerencia{

  void aplicarOperacion(Guardarropas guardarropas){
    guardarropas.quitarSugerencia(prenda);
  }

  void deshacer(Guardarropas guardarropas){
    guardarropas.agregarSugerencia(prenda);
  }

}

class Guardarropas{

  List<Sugerencia> sugerencias;

  void agregarSugerencia(Sugerencia sugerencia){
    this.sugerencias.add(sugerencia);
  }

  void quitarSugerencia(Sugerencia sugerencia){
    this.sugerencias.remove(sugerencia);
  }
}

```

- Command: **Sugerencia** (Objeto que entiende el mensaje que realiza la Operación, Interface o Abstract).
  - Operación: **aplicarEnGuardarropas()**.
- ConcreteCommand: **SugerenciaAgregar** y **SugerenciaQuitar**. Realizan la operación.
- Receiver: **Guardarropas**.
  - Acción: **agregar(), quitar()**.
- Invoker: **UI**.
- Client: **UI** (Conoce al Receptor e instancia al Concrete Command).



## Observer

- Observadores se suscriben a un evento
- El sujeto cuando ocurre el evento notifica a los observadores
- Los observadores son los interesados
- El sujeto conoce a los interesados
- Los interesados se guardan en algún lado
- El observado notifica a los observadores
- Es conveniente que **No aborten el flujo de ejecución**
  - Los observadores no deben romper
- No debería importar el orden de los observadores
- Es conveniente que se piensen como que solo se dispara la operación (Fire and forget)

```java

// En algun lugar
void actualizarAlertas(){
  this.alertas = proveedorClima.getAlertas(ciudad);
  RepositorioUsuarios.getInstance().todos().forEach(
    usuario.hayAlertas(this.alertas)
  );
}

// Usuario
void hayAlertas(alertas){
  this.interesados.forEach(
    interesado -> interesado.hayAlertas(alertas)
  )
}

```

- Evento `getAlertas()`
- Notificación: el contenido de `hayAlertas(alertas)`
- El usuario es el interesado que tiene "interesados", porque decide qué cosas hacer

Sujeto: Tiene la colección de observers
Observer: Es la interfaz que entiende `notify()` (o `hayAlertas()`)
ConcreteObserver: Cada una de las clases que implementan Observer


Ejemplo Nefli:

<div align = center>

![img](http://www.plantuml.com/plantuml/png/VP71IiH038RlUOgmfnPa7q7MYo08BWhUnqvM0zEap6G4MNnt2T8bLDnBIaA-lr_QGxDKhSy5M8pgAKs4pxHKpNqohMS0n3VLGabmAUbhuFpAcVG6PvPk-Y0yiOw0-AcSiPakmmXhM-cTcr5zagEpNXvz85Hn2Stu5tZn92yNmBEl0FSit3w6tyS5EeNMiUzmPzGvhu4gnt2c0n2GA1GG7s-_pDraFPLjGDIB4Qj-V6_u3xZVt-AkjxJ3QFtFYkjSNzbu01n8Sf_B3m00)
</div>



```java
RepoPelis.getInstance().pendientes(); // Devuelve las pendientes
reproductor.registerOnStop(new StopHandler());
reproductor.registerOnFinish(new FinishHandler());

class RepoPelis{

  List<Peli> pelis;

  List<Peli> pendientes(){
    this.pelis.filter(peli -> peli.estaPendiente)
  }

}

class Peli{

  int idVideo;
  int minutoActual = 0;

  boolean estaPendiente(){
    return minutoActual != 0;
  }

  void continuarViendo(){
    reproductor.play(idVideo, minutoActual);
  }
}

class StopHandler implements StopListener{

  void onStop(idVideo, minutoActual){
    peli = RepoPelis.getInstance().findPeli(idVideo);
    peli.setMinutoActual(minutoActual);
  }
}

class FinishHandler implements FinishListener{

  void onFinish(idVideo){
    peli = RepoPelis.getInstance().findPeli(idVideo);
    peli.setMinutoActual(0);
  }
}
```


## Repositorio

- Conoce a todas las instancias de un tipo.
- Puede retornarlas todas.
- Retorar segun criterio.
- Agregar/remover.


## State

- Cosificar estado
- Estado que tiene comportamiento
- El estado sabe transicionar entre estados

<div align = center>

![img](http://www.plantuml.com/plantuml/png/LOynhi8m44HxdsBB_mjo11G9aDBGSO75MT0YhotPQnhWxXYIOATzysQawPDYr2pEi5UA5xG4XyB6y300DWZ5Fy5aW-9_0RTynGQyZB4EKuBZH6gdoesr2rx9Jnfx1SztSks86xA4-8dtg7HYeUAnvR14LHvGpf6_QuKjoz3bxhcT7vYbjQQrkURp1zZyI4z-pEBU)
 </div>

- La mascota no sabe comer y jugar, sino que depende del estado
- A diferencia del Strategy, el comportamiento está asociado al Estado y no al Algoritmo
- El strategy se setea arbitrario
- El state sabe cómo transicionar entre cada uno de los estados
- Los cambios de estados no son arbitrarios
- Que se llame Estado no implica que sea State
- Validaciones que dan error no necesariamente devienen en un State


```java
class Deuda{

  Estado estado;

  void pagar(){
    estado.pagar(this)
  }
}

class Pendiente{
  void pagar(Deuda deuda){
    deuda.setEstado(new Pagado())
  }
}

class Pagado{
  void pagar(Deuda deuda){
    throw new YaEstaPaga()
  }
}
```

Esta solución tiene pinta de State pero no lo es, ya no son polimórficos, hay uno que anda pero otro no. Quizá sea más sencillo modelarlo con un booleano.



## Composite

- Modelar una estructura recursiva polimórfica

<div align = center>

![img](http://www.plantuml.com/plantuml/png/VP2n2i9038RtUuhGIOM-GYbsSXFi9Wvf6wHmPyeb9qZrtJrf1NiKfy2NvF_WBqNHQt3Mj1P5uEZvGQto-38pgT4JZWO06r6FhKB7eR44LzAAHRbcHRlP6aWN2JkuWQNN_ivhntiKYdofFVLki1Jbwyxql68t11B5-36HymNqC-Lln8yByf0_Y-dPB4CNgszFa1UGxRAPbdJhOfTl)


</div>

- La tarea simple son los nodos terminales (Finalizan la estructura recursiva)
- Las tareas compuestas tienen un conjunto de tareas que pueden ser simples o compuestas
- Trato a todas por igual (Todas saben decir el costo total)

```java

class Proyecto{

  List<Tarea> tareas;

  public double getCostoTotal(){
    return this.getCosto() + tareas.sum(tarea -> tarea.getCostoTotal());
  }
}

class TareaSimple{
  public double getCostoTotal(){
    return this.getCosto();
  }
}

class TareaCompuesta{

  List<Tarea> subtareas;

  public double getCostoTotal(){
    return this.getCosto() + subtareas.sum(tarea -> tarea.getCostoTotal());
  }
}
```

- Client: Proyecto - Usa de forma polimórfica a los nodos
- Component: Tarea - El genérico
- Leaf: TareaSimple - La hoja - Nodo terminal
- Composite: TareaCompuesta - Nodo compuesto


















