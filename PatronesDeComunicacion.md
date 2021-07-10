# Patrones de Comunicación

## Call & Return

- A llama a B
- B responde con un valor de retorno a A
- Natural para métodos sin efecto
- Puede también devolverse un objeto nuevo que represente el efecto


```js
//B
class ConversorAKms{
  convertir(millas){
    return 1.6 * millas
  }
}

//A
const conversor = new ConversorAKms();
console.log(conversor.convertir(10))

>> 16
```

```haskell
type Stack = [Int]

empty :: Stack
empty = []

push :: Int -> Stack -> Stack
push number stack = number : stack

pop :: Stack -> (Int, Stack)
pop [] = error "Stack underflow"
pop (number:stack) = (number, stack)

discard :: (Int, Stack) -> Stack
discard = snd . pop

peek :: (Int, Stack) -> Int
peek = fst . pop

use = peek . push 2 . push 1 $ empty
```

## Memoria Compartida

- Utilizar un espacio físico como medio de comunicación
  - Memoria
  - File System
  - etc
- A escribe y lee en C
- B escribe y lee en C
- Más dificil de testear
- Tener cuidado de no *"tocar"* el valor antes de que lo lea el que lo necesita

```js
//A
class ConversorAKms{
  convertir(millas){
    this.kms = 1.6 * millas; // C
  }
}

//B
const conversor = new ConversorAKms();
conversor.convertir(10);

console.log(conversor.kms)
```

## Continuaciones

- A llama a B con un código a ejecutar (callback)
- B continúa la ejecución
- En general para operaciones asincrónicas

```js

//B
function convertir(millas, callback){
  withDelay(() => callback(1.6 * millas))
}

//A
convertir(10, (kms) => console.log(kms))

console.log("Llamé al convertir")

>> "Llamé al convertir"
>> 16
```

## Eventos

- A se registra en B
- B avisa a A cuando hay eventos

```js

class Alumno {
  constructor(nombre){
    this.notas = []
    this.nombre = nombre
    this.emmiter = new Events.EventEmitter;
  }

  agregarNota(nota){
    this.notas.push(nota);
    if(nota >= 6) {
      this.emmiter.emit("Aprobado", {nota, alumno: this})
    } else {
      this.emmiter.emit("Desaprobado", {nota, alumno: this})
    }
  }
}

const unAlumno = new Alumno()

unAlumno.emmiter.on("Desaprobado", ({nota, alumno}) => {
  console.log(`${alumno} desaprobó con ${nota}`)
})

unAlumno.emmiter.on("Aprobado", ({nota, alumno}) => {
  console.log(`${alumno} aprobó con ${nota}`)
})

unAlumno.agregarNota(10)
```

## Streaming

- Cosificación de algo que me entrega un conjunto de valores
- Entiende map/filter/etc
- También entienden otros mensajes de bajo nivel, por ejemplo `on`
- No se le puede pedir el size, pero te puede ir entregando elementos de a uno
- Para grandes volúmenes de datos que no deseo tener en memoria (o infinitos)
- A llama a B
- B crea el stream
- A se comunica con el Stream

```js
infinitosTwits
  .map(twit => `Leyendo ${twit}`) 
  .subscribe(console.log); //Esto hace cuando los emito
```


