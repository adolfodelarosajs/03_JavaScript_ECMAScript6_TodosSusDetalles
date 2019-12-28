# Clases

## Pre-Introducción a las Clases

Una **clase** es una abstracción de algo que existe en la vida real.

El nombre de una clase va en mayúscula por ejemplo **Carro**, una clase puede tener **propiedades** que son características que describen a nuestra clase, en el caso del Carro pueden ser:

* puertas
* color
* marca
* modelo
* fabricante

Las clases también tienen **métodos** que son acciones que pueden realizar las clases, en el caso del Carro:

* encender()
* apagar()
* acelerar()
* frenar()

Cuando se crea un objeto a partir de una clase se dice que estamos instanciando a la clase.

#### Herencia

La herencia es específica de la programación orientada a objetos, donde una clase nueva se crea a partir de una clase existente. La herencia (a la que habitualmente se denomina subclase) proviene del hecho de que la subclase (la nueva clase creada) contiene los atributos y métodos de la clase primaria. La principal ventaja de la herencia es la capacidad para definir atributos y métodos nuevos para la subclase, que luego se aplican a los atributos y métodos heredados.

## Introducción a las clases

Las clases en ES5 se simulaban con funciones de primer orden:

```
function Persona( nombre ){
  this.nombre = nombre;
  this.gritarNombre = function(){
    console.log( this.nombre.toUpperCase() );
  }
}

Persona.prototype.decirNombre = function(){
  console.log( this.nombre );
}

let persona = new Persona("Penélope");

persona.gritarNombre();
persona.decirNombre();

//output
PENÉLOPE
Penélope
```

Pero esto tiene ciertos problemas porque a fin de cuenta esto es una función es una función y la podemos llamar como tal es decir sin usar `new` pero el comportamiento es diferente si se usa o si no, por lo que esto puede traer problemas. Tendríamos que añadir más código `persona instanceof Persona` para saber si realmente es una instancia del objeto, como se observa esta simulación de clase implica meter validaciones para hacer que trabaje bien nuestro código, en ES6 han surgido las clases para evitar todo esto.

## Declaración básica de una clase en ES6

Veamos la creación de la clase con ES6

```
class Persona{
  
  constructor(nombre){
    this.nombre = nombre;
  }

  decirNombre(){
    console.log( this.nombre );
  }

  gritarNombre(){
    console.log( this.nombre.toUpperCase() );
  }
}

let persona = new Persona("Penélope");

persona.gritarNombre();
persona.decirNombre();

//output
PENÉLOPE
Penélope
```

Obsérvese que una clase puede contener un `constructor` el cual nos permite inicializar nuestra instancia de clase, los métodos ya no es necesario declararlos como funciones. Otro punto importante es que una `class` simpre la debemos invocar con `new` lo que previene muchos errores y si no lo hacemos directamente nos marca un error:

```
Uncaught TypeError: Class constructor Persona cannot be invoked without 'new'
    at app.js:16
```

Podemos hacer las siguientes preguntas:

```
console.log( persona instanceof Persona );
console.log( persona instanceof Object );

console.log( typeof Persona );
console.log( typeof Persona.prototype.decirNombre );

//output
true
true
function
function
```

Lo único extraño es que al preguntar el tipo de la Persona `typeof Persona` nos regrese `function` tal vez lo mejor era que regresara `class`.

## ¿Por qué usar la sintaxis de clase?

1. Las clase funcionan muy parecido a la declaración LET, solo están allí hasta que la función alcanza la declaración.

2. Todo el código dentro de la clase funciona en modo estricto "strict mode".

3. Todos los métodos no son enumerables.

4. Todos los métodos internos no tienen un constructor.

5. Llamar una clase sin el `new` dará un error.

6. Intentar renombrar el nombre de la clase dentro de algún método de la misma dará error.

7. Tienen métodos estáticos y privados.

## Clases como expresiones

Simplemente es otra forma de crear la clase asociándosela a una variable.

Primero veamos el ejemplo de una función hecha expresión:

```
let miFuncion = function(){
  console.log("Mi función....");
}

let otraFuncion = miFuncion;

console.log( typeof otraFuncion );
otraFuncion();

//output
function
Mi función....
```

Veamos el ejemplo de la clase:

```
let Persona = class{
  constructor(){
    this.nombre = "Penélope";
    this.edad   = 33;
  }

  escribirDatos(){
    console.log(`${nombre} - ${edad}`);
  }
}

let persona = new Persona();

console.log( persona );
console.log( typeof persona );
console.log( persona instanceof Persona );

//output
Persona {nombre: "Penélope", edad: 33}
object
true
```

## Clases como parámetros

Vemos un primer ejemplo donde pasamos una clase anónima como parámetro a una función:

```
function creadorClases( definicionClase ){
  return new definicionClase();
}

let objeto = creadorClases( class{
  constructor(){
    this.nombre = "Penélope";
    this.edad = 30;
  }
  saludar(){
    console.log("Hola!!!")
  }
});

objeto.saludar();

//output
Hola!!!
```

En el siguiente ejemplo definiremos la clase y después la pasaremos como parámetro a una función la cual verifica que efectivamente lo que se recibe sea una clase de cierto tipo, de lo contrario mandara un error:

```
class Cuadrado{
  constructor(lado){
    this.lado = lado;
  }
  getArea(){
    return this.lado * this.lado;
  }
}

function imprimirCuadrado( cuadrado ){
  if( !(cuadrado instanceof Cuadrado) ){
    console.error("El parámetro enviado no es de tipo Cuadrado");
    return
  }
  console.log( cuadrado.getArea() );
}

let mesa = new Cuadrado(10);

imprimirCuadrado( mesa );
imprimirCuadrado( "10" );

//output
100
El parámetro enviado no es de tipo Cuadrado
```

## Examen #10 

:+1:
