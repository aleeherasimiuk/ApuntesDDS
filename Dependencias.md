# Dependencias

- Todo aquello que necesita un objeto para funcionar.
- Haremos foco en aquellas que no son nuestra responsabilidad.


## Conocimiento Directo

- Conozco a la dependencia, a quien le envío el mensaje

```java
void notificarNota(){
  new Mailer().send(this.notas.last(), "Nueva Nota");
}
```


```java
void notificarNota(){
  new Mailer.getInstance().send(this.notas.last(), "Nueva Nota");
}
```

## Service Locator

- Global
- Intercambiable

Registro una dependencia que luego puedo usar.

```java
ServiceLocator.instance().registrar(Mailer.class, new MailerGmail());

void notificarNota(){
  getMailer().send(this.notas.last(), "Nueva nota");
}

Mailer getMailer(){
  return ServiceLocator.instance().get(Mailer.class);
}

```
```java
class ServiceLocator{
  Map<Class, Object> dependencias = new Hashmap<>();

  void registrar(Class clazz, Object instance){
    dependencias.put(clazz, instance);
  }

  Object get(Class clazz){
    return dependencias.get(clazz);
  }
}
```

## Inyección de dependencias (Conocimiento Indirecto)

- Al ser parametrizado, no sabés de qué es la instancia.
- Polimorfismo.

```java
class Alumno{

  Alumno(mailer){
    this.mailer = mailer;
  }

  void notificarNota(){
    this.mailer.send(this.notas.last(), "Nueva nota")
  }
}
```
O bien puede inyectarse como parámetro en `notificarNota(Mailer mailer)`















