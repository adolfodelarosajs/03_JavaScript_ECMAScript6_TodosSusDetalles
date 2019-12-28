# ¿Cómo funcionan las declaraciones y las uniones (binding)?

## Declaraciones de variables VAR vs LET

**Scope**: El ámbito de una variable (llamado "scope") es la zona del programa en la que se define la variable.

JS define dos ámbitos para las variables:

* Global
* Función

Ejemplo del uso de VAR:

```
console.log( saludo );
var saludo = "Hola Mundo";

//output
undefined
```

Ejemplo del uso de LET:

```
console.log( saludo );
let saludo = "Hola Mundo";

//output
Uncaught ReferenceError: Cannot access 'saludo' before initialization
    at app.js:1
```

Cuando declaramos una variable con `let` no podemos usarla antes de declararla.

Veamos otro ejemplo con VAR:

```
if ( false ){
  var mensaje = "Hola Mundo";
}

console.log(mensaje);

//output
undefined
```

Aquí JS hace el barrido de todo el código y ve que hay una variable *mensaje* declarada, pero como no se ejecuta el código no contiene valor por eso nos muestra `undefined`. Si la condición fuera `true` la salida sería `Hola Mundo`.

Veamos qué pasa si usamos LET:

```
if ( true ){
  let mensaje = "Hola Mundo";
}

console.log(mensaje);

//output
Uncaught ReferenceError: mensaje is not defined
    at app.js:5
```

Aquí la condición si se cumple y dentro del `if` se declara la variable mensaje, pero fuera del `if` esa variable no existe por eso nos muestra el mensaje de error.

`let` crea la variable solo en el `scope` donde ha sido declarada.

## No hay re-declaraciones

Cuando declaramos una variable con `var` la podemos volver a re-declarar nuevamente las veces que se quiera y no nos marca ningún error:

```
var mensaje = "123";
var mensaje = "Hola";
var mensaje = "Hi";
var mensaje = "Hello";
```

Pero cuando se intenta volverla a declarar con un `let` nos regresa un error:

```
var mensaje = "123";
let mensaje = "Hola";
var mensaje = "Hi";
var mensaje = "Hello";

//output
Uncaught SyntaxError: Identifier 'mensaje' has already been declared
```

¿Entonces el siguiente código también nos regresará error?

```
let mensaje = "Hola";

if( true ){
  let mensaje = "Hello";
}

console.log( mensaje );

//output
Hola
```

Aquí realmente no estamos re-declarando la variable mensaje, más bien estamos creando otra variable en otro ámbito o scope diferente, por eso no hay ningún error.

Y si usamos `var` ¿también estamos usando otro ambito?

```
var mensaje = "Hola";

if( true ){
  var mensaje = "Hello";
}
console.log(mensaje);

//output
Hello
```

Con `var` se declara de manera glogal no existe un ámbito local, vea:

```
if( true ){
  var mensaje = "Hello";
}
console.log(mensaje);

//output
Hello
```

## Declaración de constantes

Las constantes se definen con `const`, las constantes se deben inicializar cuando son definidas y su valor no puede cambiar.

```
const IVA;

//output
Uncaught SyntaxError: Missing initializer in const declaration
```
La forma correcta de declarar la constante es:

```
const IVA = 21;
console.log( IVA );

//output
21
```

Si tratamos de modificar el valor de una constante nos marcara el siguiente error:

```
const IVA = 21;
console.log( IVA );

IVA = 9;
console.log( IVA );

//output
Uncaught TypeError: Assignment to constant variable.
    at app.js:4
```

Las constantes se declaran en el ámbito al que pertenece.

```
const IVA = 21;

if ( true ){
  const IVA = 9;
  console.log( IVA );
}

console.log( IVA );

//output
9
21
```

La constante también puede definir objetos:

```
const PERSONA = {
  nombre: "Penélope",
  apellido: "Cruz"
}

console.log( PERSONA );

//output
{nombre: "Penélope", apellido: "Cruz"}
```

Si intentamos redefinir los valores del objeto PERSONA nos marcara un error:

```
const PERSONA = {
  nombre: "Penélope",
  apellido: "Cruz"
}

console.log( PERSONA );

PERSONA = {
  nombre: "Salma",
  apellido: "Hayek"
}

console.log( PERSONA );

//output
Uncaught TypeError: Assignment to constant variable.
    at app.js:8
```

Sin embargo si modificamos individualmente los valores si que lo permite:

```
const PERSONA = {
  nombre: "Penélope",
  apellido: "Cruz"
}

console.log( PERSONA );

PERSONA.nombre = "Salma";
PERSONA.apellido = "Hayek";

console.log( PERSONA );
```

¿Debería considerarse un error?

## Declaraciones de variables en ciclos

Qué pasa cuando declaramos una variable en un ciclo:

```
for( var i = 0; i <= 10; i++ ){
  //código
}

console.log( i );

//output
11
```

Como ya vimos si usamos `var` se declara de forma global, por eso una vez que termina el ciclo seguimos teniendo acceso a la variable.

Y si en lugar de usar `var` usamos `let`:

```
for( let i = 0; i <= 10; i++ ){
  //código
}

console.log( i );

//output
Uncaught ReferenceError: i is not defined
    at app.js:5
```

Al usar `let` la variable solo vive en su ámbito.

## Declaración de funciones en ciclos

Veamos el siguiente ejemplo:

```
// Array
var funciones = [];

//Llenar el array con funciones
for( var i = 1; i <= 10; i++ ){
  funciones.push( function(){ console.log(i); })
}

//Recorrer el array invocando cada función
funciones.forEach( function(func){
  func();
});

//output
10 11  //10 veces 11
```

No salió lo que esperábamos (1, 2, 3, 4, 5, 6,7 ,8 , 9, 10).

Esto pasa por que cuando salimos del ciclo `for` el valor de `i` tiene el valor de 11, y cuando se invoca el llamado de la función que dice que imprima `i` eso es lo que hace imprimir `i` es decir imprimir 11.

Por lo que si quisiera imprimir realmente del 1-10, ¿Cómo lo haría?:

```
// Array
var funciones = [];

//Llenar el array con funciones
for( var i = 1; i <= 10; i++ ){
  funciones.push( 
    (function(valor){
      return function(){
        console.log(valor);
      }
    })(i)
  );
}

//Recorrer el array invocando cada función
funciones.forEach( function(func){
  func();
});


//output
1 2 3 4 5 6 7 8 9 10
```

De esta manera tan enredada podemos obtener el resultado deseado, pero el código es complicado de entender.

Con ES6 y el uso del `let` es muy sencillo hacerlo sin complicar el código:

```
var funciones = [];

//Llenar el array con funciones
for( let i = 1; i <= 10; i++ ){
  funciones.push( function(){ console.log(i); })
}

//Recorrer el array invocando cada función
funciones.forEach( function(func){
  func();
});

//output
1 2 3 4 5 6 7 8 9 10
```

## Examen #1 - Introducción al ES6 

:+1:
