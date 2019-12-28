# Funciones de Flecha - Arrow Functions ( => )

## Arrow Functions - Funciones de Flecha ( => )

Este tipo de funciones tienen una sintaxis variada depende de lo que se quiera realizar.

Pero normalmente tienen la misma estructura:

1. Función con argumentos
2. Seguido de una flecha gorda =>
3. Seguido del cuerpo de la función

¿Para qué sirven?

1. Menos código que es más simple de interpretar.
2. No hay un nuevo "this" dentro de las funciones.
3. No hay constructores, no tiene new.
4. No puedes cambiar el valor del "this" aun que uses call(), apply() o bind().
5. No hay objeto "arguments"
6. Entre otras cosas...

Técnicamente hablando

Son funciones definidas con una sintaxis que usa una flecha "=>", pero se comportan de una forma muy diferente a las funciones normales en ECMAScript 5.

Tienen 6 características principales:

1. No hay creación de this, super, arguments y new.target
2. No puede ser llamado con new.
3. No tienen prototipos prototype
4. No puedes cambiar "THIS"
5. No hay objeto "arguments"
6. No pueden tener nombres de parámetros duplicados.

## Ejemplos de funciones de flecha

Función con un parámetro y retorna un valor:

```
//Función normal
var miFuncion = function( valor ){
  return valor;
}

//Función de Flecha equivalente
let miFuncionFlecha = valor => valor;

console.log( miFuncion(5) );
console.log( miFuncionFlecha(5) );

//output
5
5
```

Función con dos parámetros y que retorna un valor:

```
//Función normal
var sumar = function( num1, num2 ){
  return num1 + num2;
}

//Función de Flecha equivalente
let sumarFlecha = (num1, num2) => num1 + num2;

console.log( sumar(5, 10) );
console.log( sumarFlecha(5, 10) );

//output
15
15
```

Función que no recibe ningún parámetro pero retorna un valor:

```
var saludar = function(){
  return "Hola Mundo!";
}

let saludarFlecha = () => "Hola Mundo!";

console.log( saludar() );
console.log( saludarFlecha() );

//output
Hola Mundo!
Hola Mundo!
```

Función con varias líneas de código:

```
var saludarPersona = function( nombre ){
  var saludo = "Hola " + nombre
  return saludo;
}

let saludarPersonaFlecha = nombre => {
  let saludo = `Hola ${nombre}`
  return saludo;
}
console.log( saludarPersona("Penélope") );
console.log( saludarPersonaFlecha("Penélope") );

//output
Hola Penélope
Hola Penélope
```

Función que retorna un object:

```
var getLibro = function(id){
  return {
    id: id,
    nombre: "Harry Potter"
  }
}

let getLibroFlecha = id => ({ id: id, nombre: "Harry Potter"});

console.log( getLibro(1) );
console.log( getLibroFlecha(1) );

//output
{id: 1, nombre: "Harry Potter"}
{id: 1, nombre: "Harry Potter"}
```

Nótese que en la función de flecha para retornar el objeto fue necesario envolverlo entre (), porque si sólo hubiéramos puesto el objeto, las llaves se confundirían pensando que es un grupo de líneas de código por eso fue necesario envolverlo entre ().

## Creando funciones anónimas

Creamos una función anónima normal y otra de función anónima de flecha:

```
//Funcion anónima normal
var saludo = function(nombre){
  return "Hola " + nombre;
}("Penélope");

//Función anónima de flecha
let saludoFlecha = ( nombre => `Hola ${nombre}`)("Penélope");

console.log(saludo);
console.log(saludoFlecha);

//output
Hola Penélope
Hola Penélope
```

## No hay cambios en el objeto "this"

Vamos a ver el siguiente código:

```
var manejador = {
  id: "123",
  init: function(){
  	document.addEventListener("click", function(event){
      this.clickEnPagina( event.type );
  	}, false );
  },
  clickEnPagina: function(type){
    console.log("Manejando " + type + " para el id: " + this.id );
  }
}

manejador.init();

//output
Uncaught TypeError: this.clickEnPagina is not a function
    at HTMLDocument.<anonymous> (app.js:5)
```

El error que nos da es porque `this` no apunta al `manejador` sino más bien apunta al objeto `document`.

Para solucionar el problema en ES5 debemos indicarle mediante un `bind()` el `this` que debe tomar como referencia:

```
var manejador = {
  id: "123",
  init: function(){
  	document.addEventListener("click", (function(event){
  	  console.log(this);
      this.clickEnPagina( event.type );
  	}).bind(this));
  },
  clickEnPagina: function(type){
    console.log("Manejando " + type + " para el id: " + this.id );
  }
}

manejador.init();

//output
{id: "123", init: ƒ, clickEnPagina: ƒ}
Manejondo click para el id: 123
```

Como solucionamos este problema del `this` con ES6:

```
var manejador = {
  id: "123",
  init: function(){
  	document.addEventListener("click", 
      event => this.clickEnPagina( event.type ), false);
  },
  clickEnPagina: function(type){
    console.log("Manejando " + type + " para el id: " + this.id );
  }
}

manejador.init();

//output
Manejando click para el id: 123
```

Mucho más sencillo manejando las funciones de flechas para el manejo del `this`.

Nota: El parámetro `false` de la función `addEventListener` es para que no se ejecute inmediatamente el código al cargar el JS, por default ese es su valor por lo que se podría quitar de estos ejemplos.

## Funciones de Flecha y Arreglos

Vamos a ver la forma de ordenar un array con ES5:

```
var arreglo = [5, 10, 11, 2, 1, 9, 20];
console.log( arreglo );
var arregloOrdenado = arreglo.sort( function(a,b){
  return a-b;
});

console.log( arregloOrdenado );

//output
(7) [5, 10, 11, 2, 1, 9, 20]
(7) [1, 2, 5, 9, 10, 11, 20]
```

Usando las funciones de flecha:

```
var arreglo = [5, 10, 11, 2, 1, 9, 20];
console.log( arreglo );

let arregloOrdenadoFlecha = arreglo.sort((a,b)=>a-b);
console.log( arregloOrdenadoFlecha );

//output
(7) [5, 10, 11, 2, 1, 9, 20]
(7) [1, 2, 5, 9, 10, 11, 20]
```

## Identificando funciones de flecha y otros ejemplos

Podríamos identificar el tipo de una función de fecha si fuera necesario:

```
var restar = (a,b) => a-b;

console.log( typeof restar );
console.log( restar instanceof Function );

//output
function
true
```

Recordemos que una función de flecha no se puede usar con el operador "new".

```
var restar = (a,b) => a-b;
var restar2 = new restar(1,2);

//output
app.js:2 Uncaught TypeError: restar is not a constructor
    at app.js:2
```

Debe usarse sin "new":

```
var restar = (a,b) => a-b;
var restar2 = restar(1,2);
console.log(restar2);

//output
-1
```

Las funciones de flecha no tienen el objeto `arguments`:

```
((a,b) => {
  console.log( arguments[0]);
})(5,4);

//output
Uncaught ReferenceError: arguments is not defined
    at app.js:2
    at app.js:3
```
Pero si mi función anónima estuviera contenida en una función la cosa cambia:

```
function sumar(x,y){
  ((a,b) => {
    console.log( arguments[0]);
  })(5,4);
}
sumar(10,20);

//output
10
```

La función de flecha sigue sin tener el objeto `arguments` pero como este si existe en la función `sumar` es el que toma para imprimir.

## Examen #4 

:+1:
