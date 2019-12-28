# Funciones en ECMAScript 6

## Parámetros por defecto

Las funciones pueden recibir parámetros por defecto.

Veamos el siguiente ejemplo de función:

```
function saludo( mensaje, tiempo ){
  setTimeout(function(){
    console.log( mensaje );
  }, tiempo);
}

saludo();

//output
undefined
```

Como no estamos mandando ningún argumento, los parámetros tienen un valor `undefined` y la función `setTimeout` se ejecuta inmediatamente y al finalizar la función como no tiene return regresa `undefined`.

Esta funcionalidad no es la que queremos ejecutar, deberíamos validar los parámetros recibidos para saber lo que hay que hacer.

```
function saludo( mensaje, tiempo ){
  mensaje = mensaje || "Hola Mundo";
  setTimeout(function(){
    console.log( mensaje );
  }, tiempo);
}

saludo();

//output
Hola Mundo
```

```
function saludo( mensaje, tiempo ){
  mensaje = mensaje || "Hola Mundo";
  setTimeout(function(){
    console.log( mensaje );
  }, tiempo);
}

saludo("Hola España");

//output
Hola España
```

Otra forma de hacerlo es de la siguiente forma:

```
function saludo( mensaje, tiempo ){
  mensaje = ( typeof mensaje ) !== "undefined" ? mensaje : "Hola Mundo";

  setTimeout(function(){
    console.log( mensaje );
  }, tiempo);
}

saludo();

//output
Hola Mundo
```

Una forma más sencilla y clara es gracias a ES6 que nos permite poner un valor por defecto para un parámetro en caso de que no se le mande un valor.

```
function saludo( mensaje = "Hola Mundo", tiempo = 1500 ){
  setTimeout(function(){
    console.log( mensaje );
  }, tiempo);
}

saludo();

//output (Después de 1.5 seg.)
Hola Mundo
```

Si quisiéramos tener una función con el mensaje opcional y el tiempo fijo tendríamos:

```
function saludo( mensaje, tiempo = 1500 ){

  setTimeout(function(){
  	console.log( mensaje );
  }, tiempo);
}

function saludoMensaje( mensaje ){
  saludo(mensaje);
}

saludoMensaje("Este es mi saludo");

//output (Después de 1.5 seg.)
Este es mi saludo
```

Los parámetros opcionales pueden ser también objects, fuction o arrays.

Por ejemplo:

```
function saludar( fn ){
  fn();
}

saludar();

//output
app.js:2 Uncaught TypeError: fn is not a function
    at saludar (app.js:2)
    at app.js:5
```

Ya que `fn` no es una función, deberíamos validar si lo que se recibe es de tipo function se invoca pero en este caso lo que esta recibiendo es un `undefined`.

```
function saludar( fn ){
  if( typeof fn === "undefined" ){
    console.error('No es una función');
    return;
  }
  if( typeof fn === "function" ){
    fn();
    return true;
  }
}

//output
No es una función
```

Como se menciono anteriormente también se puede poner como parámetro por default una función:

```
function saludar( fn = function(){ console.log("Hola Mundo"); }){
  if( typeof fn === "undefined" ){
    console.error('No es una función');
    return;
  }
  if( typeof fn === "function" ){
    fn();
    return true;
  }
}

saludar();

//output
Hola Mundo
```

En el ejemplo anterior estamos poniendo una función anónima pero bien podría ser una función declarada:

```
function saludar( fn = fnTemporal){
  if( typeof fn === "undefined" ){
    console.error('No es una función');
    return;
  }
  if( typeof fn === "function" ){
    fn();
    return true;
  }
}

function fnTemporal(){
  console.log("Hola Mundo FN");
}

saludar();

//output
Hola Mundo FN 
```

Nótese que se a puesto la función sin llaves al asignarla `fn = fnTemporal` por que no deseamos que se ejecute.

Vamos a añadir otro parámetro opcional esta vez de tipo object:

```
function saludar( fn = fnTemporal, persona = {nombre: "Fernando"}){
  if( typeof fn === "undefined" ){
    console.error('No es una función');
    return;
  }
  if( typeof fn === "function" ){
    fn();
    console.log(persona);
    return true;
  }
}

function fnTemporal(){
  console.log("Hola Mundo FN");
}

saludar();

//output
Hola Mundo FN
{nombre: "Fernando"}
```

Si el objeto se le pasa como argumento se imprimirá lo que le mandemos.

```
function saludar( fn = fnTemporal, persona = {nombre: "Fernando"}){
  if( typeof fn === "undefined" ){
    console.error('No es una función');
    return;
  }
  if( typeof fn === "function" ){
    fn();
    console.log(persona);
    return true;
  }
}

function fnTemporal(){
  console.log("Hola Mundo FN");
}

var persona = {
  nombre: "Salma",
  apellido: "Hayek"
 }

saludar( function(){ console.log("A mí me gusta..."); }, persona);

//output
A mí me gusta...
{nombre: "Salma", apellido: "Hayek"}
```

## Cómo los valores por defecto afectan el objeto "arguments"

Recordemos que cuando creamos una función esta siempre tendrá una propiedad llamada `arguments` la cual contiene todos los valores de los argumentos cuando fue invocada (NO DE LOS PARÁMETROS), por ejemplo:

```
function argumentos(a,b){
  console.log(arguments);
}

argumentos(1, 2, 3, "Salma", {a:90, b:60, c:90});

//output
Arguments(5) [1, 2, 3, "Salma", {…}, callee: ƒ, Symbol(Symbol.iterator): ƒ]
```

Pero que pasa cuando usamos los parámetros por default:

```
function argumentos(a = 1, b = 2){
  console.log(arguments);
}

argumentos();

//output
[]
```

Como no se mandaron argumentos se tiene en `arguments` un array vacío, los valores por default de los parámetros no afectan a `arguments`.

## Parámetros REST - Parámetros sin nombre

El parámetro **rest** es indicado con tres puntos (...) seguido del nombre que le asignaremos a dicho parámetro.

Ese parámetro se convierte en un arreglo que contiene el "resto" de los parámetros pasados a la función.

De ahí se origina el nombre "REST".

Vamos a suponer que queremos meter en un array que se va a pasar como argumentos, una lista de alumnos que también se pasan como argumentos, para que al final en el array se contenga todo el listado de alumnos.

Primero lo vamos a hacer con ES5:

```
//array_alumnos, alumno1, alumno2, alumno3, ....
function agregar_alumnos(){
  console.log( arguments );
  for( var i = 1; i < arguments.length; i++ ){
    arguments[0].push( arguments[i] );
  }
  return arguments[0];
}

var alumnos_inicial = ["Fernando"];
var alumnos_final = agregar_alumnos( alumnos_inicial, "María", "Pedro", "Susana", "Juan");

console.log( alumnos_final );

//output
Arguments(5) [Array(1), "María", "Pedro", "Susana", "Juan"]
(5) ["Fernando", "María", "Pedro", "Susana", "Juan"]
```

La función realiza su objetivo, pero queda un poco en el misterio los parámetros que recibe ya que lo estamos trabajando todo mediante `arguments`, vamos a realizar esta misma funcionalidad con ES6 usando el parámetro REST:

```
//array_alumnos, alumno1, alumno2, alumno3, ....
function agregar_alumnos( arreglo_alumnos, ...alumnos ){
  console.log( arguments );
  for( let i = 0; i < alumnos.length; i++ ){
    arreglo_alumnos.push( alumnos[i] );
  }
  return arreglo_alumnos[0];
}

let alumnos_inicial = ["Fernando"];
let alumnos_final = agregar_alumnos( alumnos_inicial, "María", "Pedro", "Susana", "Juan");

console.log( alumnos_final );

//output
Arguments(5) [Array(1), "María", "Pedro", "Susana", "Juan"]
(5) ["Fernando", "María", "Pedro", "Susana", "Juan"]
```

La salida es exactamente la misma, pero al usar los parámetros es más clara ver lo que realiza la función.

## Restricciones del parámetro REST

1. Sólo puede existir un parámetro **rest** en la función.
2. El parámetro **rest** debe ir siempre como último parámetro.

```
function juntar_nombres(...nombres, apellidos){
  //código
}

//output
Uncaught SyntaxError: Rest parameter must be last formal parameter
```

```
function juntar_nombres(...nombres, ...apellidos){
  //código
}

//output
Uncaught SyntaxError: Rest parameter must be last formal parameter
```

## El operador "Spread"

`Muy cercano a "REST", se encuentra el operador "SPREAD".

Mientras que el parámetro "REST" permite especificar argumentos independientes que serán combinados en un arreglo, el operador "SPREAD" permite especificar un arreglo que será separado y cada item enviado será un argumento independiente a la función.

Vamos a suponer que tenemos varios números y queremos saber cuál es el mayor de ellos:

```
var num1 = 10,
    num2 = 30,
    num3 = 20;

var max = Math.max( num1, num2, num3);
console.log( max );

//output
30
```

Aquí estamos usando la función `max()` del objeto `Math` que nos permite recuperar el mayor valor.

Pero qué pasaría si en lugar de tener un listado de números tuviera un array de números.

```
var numeros = [10, 30, 20];
var max = Math.max( numeros );
console.log( max );

//output
NaN
```

Con ES5 usamos la función `apply` la cual pasa como primer parámetro el objeto que se tomará como el nuevo `this` seguido de los demás argumentos que se proporcionan como un array:

```
var numeros = [10, 30, 20];
var max = Math.max.apply( Math, numeros );
console.log( max );

//output
30
``` 

Y esto funciona pero sinceramente no es muy claro por qué funciona, por que regresa el valor máximo.

Con ES6 usando el operador Spread puede ser que nos sea más claro visualizar lo que hace el código.

```
let numeros = [10, 30, 20];
let max = Math.max( ...numeros );
console.log( max );

//output
30
```

Como que aquí se intuye que la función va pasando cada uno de los parámetros y se queda con el valor mayor, es más claro.

## Romper la relación de referencia de los objetos

Como sabemos en JS todos los objetos son pasados por REFERENCIA, veamos el siguiente ejemplo:

```
let persona1 = {
  nombre: 'Penélope',
  edad: 33
}

let persona2 = persona1;

console.log( persona1 );
console.log( persona2 );

//output
{nombre: "Penélope", edad: 33}
{nombre: "Penélope", edad: 33}
```

Podríamos pensar que tenemos dos personas con los mismos datos, cambiemos los datos de las persona2:

```
let persona1 = {
  nombre: 'Penélope',
  edad: 33
}

let persona2 = persona1;

persona2.nombre = "Salma";
persona2.edad = 30;

console.log( persona1 );
console.log( persona2 );

//output
{nombre: "Salma", edad: 30}
{nombre: "Salma", edad: 30}
```

Los datos se han cambiado en las dos personas, realmente no es así los datos se han cambiado en la localidad donde están almacenados los datos y como los dos objetos persona apuntan a la misma referencia tienen los mismos datos.

Gracias al operador Spread podemos hacer una copia de los datos de un objeto a otro y no de su referencia:

```
let persona1 = {
  nombre: 'Penélope',
  edad: 33
}

let persona2 = {...persona1};

console.log( persona1 );
console.log( persona2 );

persona2.nombre = "Salma";
persona2.edad = 30;

console.log( persona1 );
console.log( persona2 );

//output
{nombre: "Penélope", edad: 33}
{nombre: "Penélope", edad: 33}

{nombre: "Penélope", edad: 33}
{nombre: "Salma", edad: 30}
```

## Añadir propiedades a objetos a partir de otros objetos

Vamos a suponer que tenemos el siguiente ejemplo:

```
let persona1 = {
  nombre: 'Penélope',
  edad: 33
}

let persona2 = {
  nombre: 'Salma',
  edad: 32,
  direccion: "Paris, Francia",
  conduce: true,
  vehiculo: true,
  vegetariano: false,
  casado: true

};

console.log( persona1 );
console.log( persona2 );

//output
{nombre: "Penélope", edad: 33}
{nombre: "Salma", edad: 32, direccion: "Paris, Francia", conduce: true, vehiculo: true, …}
```

Tenemos 2 objetos, `persona2` con más campos `persona1`, si yo quisiera que `persona1` también tuviera los campos que tiene `persona2` una forma de hacerlo sería:

```
persona1.direccion = persona2.direccion;
persona1.conduce = persona2.conduce;
persona1.vehiculo = persona2.vehiculo;
persona1.casado = persona2.casado;

console.log( persona1 );
console.log( persona2 );

//output
{nombre: "Penélope", edad: 33, direccion: "Paris, Francia", conduce: true, vehiculo: true, …}
{nombre: "Salma", edad: 32, direccion: "Paris, Francia", conduce: true, vehiculo: true, …}
```

Listo ya hemos añadido los campos que faltaban, pero ¿qué pasa si cambio los datos en `persona2`? ¿se cambian también en `persona1`?

```
persona2.direccion ="Veracruz, México"
persona2.casado = false;

console.log( persona1 );
console.log( persona2 );

//output
{nombre: "Penélope", edad: 33, direccion: "Paris, Francia", conduce: true, vehiculo: true, …}
{nombre: "Salma", edad: 32, direccion: "Veracruz, México", conduce: true, vehiculo: true, …}
```

Solo se cambian en `persona2`, por que los objetos son totalmente independientes.
Esto funciona pero tiene la desventaja de que si tiene muchos campos hay que escribir cada uno de ellos, existe una forma más sencilla con el operador Spread de ES6.

```
let persona1 = {
  nombre: 'Penélope',
  edad: 33
}

let persona2 = {
  nombre: 'Salma',
  edad: 32,
  direccion: "Paris, Francia",
  conduce: true,
  vehiculo: true,
  vegetariano: false,
  casado: true

};

persona1 = {
  ...persona2
}

console.log( persona1 );
console.log( persona2 );

//output
{nombre: "Salma", edad: 32, direccion: "Paris, Francia", conduce: true, vehiculo: true, …}
{nombre: "Salma", edad: 32, direccion: "Paris, Francia", conduce: true, vehiculo: true, …}
```

Esto lo que hace es pasarle todos los campos del objeto `persona2` a `persona1` inclusive los que ya estaban en `persona1` y sustituye sus valores, si quisiéramos conservar los valores de `persona1` lo haríamos así:

```
persona1 = {
  ...persona2,
  ...persona1
}

console.log( persona1 );
console.log( persona2 );

//output
{nombre: "Penélope", edad: 33, direccion: "Paris, Francia", conduce: true, vehiculo: true, …}
{nombre: "Salma", edad: 32, direccion: "Paris, Francia", conduce: true, vehiculo: true, …}
```

## Diferencias entre el "Spread" y el "Rest"

REST: Junta los elementos en un array.

SPREAD: Esparce los elementos como si fueran enviados de forma separada.

Veamos el siguiente ejemplo:

```
function saludarRest(saludo, ...nombres){
  for( i in nombres ){
  	console.log( `${saludo} ${nombres[i]}` );
  }

}

function saludarSpread( saludo, ...nombres){
  console.log( `${saludo} ${nombres}` );
}

saludarRest("Hola", "Penélope", "Salma", "Nikole");

let personas = ["Messi", "Cristiano", "Neymar"];
saludarSpread("Que tal!", personas );

//output
Hola Penélope
Hola Salma
Hola Nikole
Que tal! Messi,Cristiano,Neymar
```

El Spread no es muy claro, lo que hace es sacar los elementos del array y mostrarlos como una lista de valores, veamos otro ejemplo:

```
let partes = ["brazos", "piernas"];
let cuerpo = ["cabeza", "cuello", ...partes, "pies", "dedos"];

console.log( cuerpo );

//output
(6) ["cabeza", "cuello", "brazos", "piernas", "pies", "dedos"]
```

Aquí podemos ver nuevamente como spread saca los elementos de un array y los lista (dentro de otro array).

## Aclarando el doble comportamiento de las funciones

En ECMAScript 5 y versiones anteriores, las funciones sirven con un doble propósito de ser llamadas con o sin la palabra reservada "new".

Cuanado llamamos a la función con "new", el valor de "this" dentro de la función es un nuevo objeto y ese nuevo objeto es retornado.

Cuando llamamos a la función sin "new", simplemente hacemos el llamado de la función y esperamos el retorno de algún valor procesado que puede ser un objeto, undefined o null.

Veamos el ejemplo:

```
function Persona(nombre){
  this.nombre = nombre;
}

var persona = new Persona("Penélope");
var noEsPersona = Persona("Bengi");

console.log(persona);
console.log(noEsPersona);

//output
Persona {nombre: "Penélope"}
undefined
```

Vamos a pensar que solo tenemos:

```
function Persona(nombre){
  this.nombre = nombre;
}

var noEsPersona = Persona("Bengi");

console.log(noEsPersona);

//output
undefined
```

¿Qué ha pasado con nuestro `this.nombre`?

`this` apunta al objeto global window por lo que si en la consola escribimos:

```
window.nombre
"Bengi"
```

Allí es donde a ido a parar nuestra propiedad.

Esto si no tenemos cuidado nos puede dar errores lógicos difíciles de encontrar, por ejemplo:

```
var nombre = "Salma";
function Persona(nombre){
  this.nombre = nombre;
}
var noEsPersona = Persona("Bengi");

console.log(noEsPersona);
console.log(nombre);

//output
undefined
Bengi
```

Viendo el código podría pensarse que al imprimir el nombre se obtendría "Salma" sin embargo por la forma en que trabaja JS se imprime "Bengi". Por lo que podemos observar que se tienen diferentes resultados si se llama a la función con "new" o sin él.

Podríamos exigir que este tipo de funciones (Funciones de primer orden) siempre sean ejecutadas si son llamadas con "new", veamos el siguiente ejemplo con ES5:

```
function Persona(nombre){
  if ( this instanceof Persona ){
    this.nombre = nombre;
  }else{
  	throw new Error('Esta función, debe de ser utilizada con el new');
  }
}
var noEsPersona = Persona("Bengi");
console.log(noEsPersona);

//output
Uncaught Error: Esta función, debe de ser utilizada con el new
    at Persona (app.js:7)
    at app.js:12
```

Parece ser que ya hemos solucionado el problema, pero había casos en que aun la situación fallaba, por ejemplo:

```
function Persona(nombre){
  if ( this instanceof Persona ){
    this.nombre = nombre;
  }else{
  	throw new Error('Esta función, debe de ser utilizada con el new');
  }
}

var persona = new Persona("Penélope");
var noEsPersona = Persona.call(persona, "Bengi");

console.log(noEsPersona);

//output
undefined
```

En `var noEsPersona = Persona.call(persona, "Bengi");` estoy haciendo la llamada a la función sin "new" y no se dispara la excepción la ejecución de la función continua, esto es un error que en ES5 no se podía resolver.

### NEW.TARGET Meta Propiedad

Una meta propiedad, es una propiedad de un no-objeto, que provee información adicional relacionada con su procedencia (como el new).

cuando el constructor de la función es llamada, new.target se llena con el operador new.

Si la  función "call()" es ejecutada, "new.target" no estará definida ya que no se ejecutó el constructor.

Veamos:

```
function Persona(nombre){
  if ( typeof new.target !== "undefined" ){
    this.nombre = nombre;
  }else{
  	throw new Error('Esta función, debe de ser utilizada con el new');
  }
}

var persona = new Persona("Penélope");
var noEsPersona = Persona.call(persona, "Bengi");

console.log(noEsPersona);

//output
Uncaught Error: Esta función, debe de ser utilizada con el new
    at Persona (app.js:5)
    at app.js:10
```

`new.target` nos asegura que nuestra función se esta llamando con "new" lo que lo hace más seguro que `instanceof()`.

## Examen #3 

:+1:
