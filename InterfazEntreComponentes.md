# Interfaz entre Componentes

**Sistema**: Conjunto de partes que interactúan entre si. Se comunican mediante una interfaz.


### Interfaz entrante

- El componente es llamado por otro componente.


### Interfaz saliente

- El componente llama a otro componente.


```java
// Saliente. Fuera de mi responsabilidad.
BigDecimal dolarHoy = cotizador.obtenerCotizacion() 
```


Las interfaces pueden estar en el mismo ambiente, o bien puede estar en ambientes distintos.


# Sincronismo

- Invoco el componente
- Espero (Bloqueado)
- Termino la operación

```java
BigDecimal dolarHoy = cotizador.obtenerCotizacion() // Invoco
// Espero
System.out.println(dolarHoy) // Termino la operación
```

# Asincronismo

- Invoco el componente
- Me olvido (Puedo hacer otra cosa)

Ventaja:

- Puedo hacer otras cosas

Desventaja:

- ¿Si quiero el resultado?


| Mi componente                 | El otro componente         |
|-------------------------------|----------------------------|
| cotizador.obtenerCotizacion() |                            |
|                               |cotizador.setCotizacion(...)|
| cotizador.ultimaCotizacion()  |                            |

- Puede usarse un callback
- Puede usarse un buffer
  - A le pide algo a B a través del buffer
  - B le contesta al buffer
  - El buffer le contesta a A
  - B y Buffer puede ser la misma clase
  - Hay acomplamiento implícito
    - A está acoplado a B, aunque no hablen directamente





