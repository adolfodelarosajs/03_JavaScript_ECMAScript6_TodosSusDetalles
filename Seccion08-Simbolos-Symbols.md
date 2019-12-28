# Símbolos – Symbols

## Introducción a los Símbolos

Los Símbolos son un dato de dato único... nuevo... diferente...

Los tipos de datos existentes en ES5 son:

1. undefined
2. null
3. boolean
4. number
5. string
6. object

Con ES6 se ha añadido un tipo más:

7. symbols (primitivo)

Ejemplo:

[mozilla](https://www.mozilla-hispano.org/es6-en-detalle-simbolos/)

Supongamos que estamos escribiendo una librería JS que usa transiciones CSS para hacer elementos DOM moverse por pantalla.

Te habrás dado cuenta que aplicar múltiples transiciones CSS a un sólo elemento al mismo tiempo no funciona.

¿Cómo resolvemos esto?

Detectar cuando el elemento esta en movimiento.

Tu librería ya debería saber si un elemento se está moviendo, ¡es el código que hace que se moviera en primer lugar!


```
for ( var i in elementos ){
  if(elemento.isMoving){
    animarSuavemente(elemento);
  }
  elemento.isMoving = true;
}
```

Aquí tenemos una serie de problemas potenciales relacionados al hecho que tu código no es el único usando el DOM.

* Otro código usando el for-in o Object.keys() podría tropezarse con la propiedad que creaste.

* Algún otro desarrollador astuto pudo haber creado una librería con la misma técnica y tu librería no se llevaría bien con la librería existente.

* Algún otro desarrollador más astuto aún podría pensar hacer esto en el futuro y tu librería no funcionaría bien con la futura librería.

* El comité de estándares podría decidir agregar un método llamado .isMoving() a todos los elementos. (Aquí si estamos fregados)

¿Cómo podemos evitar esto?

Usando algo así:

```
if(element.--$animacion_Fernando$POR_FAVOR_NO_USAR$isMoving__){
  animarSuavemente(element);
}
element.__$animacion_Fernando$POR_FAVOR_NO_USAR$isMoving__=true;
```

Pero esta codificación es muy poco recomendable.

Otra forma posible sería usar criptografía:

```
var isMoving = SecureRandom.generateName();
....
if(elemento[isMoving]){
  animarSuavemente(elemento);
}
elemento[isMoving] = true;
```

Esto podría funcionar, las colisiones son virtualmente imposibles y el código se ve bien.

Pero esto tiene el problema de que todas las propiedades generadas de manera aleatoria o encriptada para prevenir esas posibles colisiones, para evitar que alguien use los mismos nombres de variable o propiedades el mantenimiento y la depuración se harían muy complicadas.

¿Por qué esto es tan difícil?
¡Solo queremos un simple booleano!

La respuesta son los **Símbolos**, son perfectos para poner nombres de propiedades y así asegurarse de que no van a colisionar con los nombres de otras propiedades de otros elementos que se encuentren en otras librerías o en el mismo código.

## Símbolos y propiedades


El tipo de datos **symbol** es un tipo de datos primitivo. La función `Symbol()` devuelve un valor de tipo **symbol**.

Cada valor symbol devuelto por `Symbol()` es único. Un valor symbol puede usarse para identificar las propiedades de un objeto; Este es el único propósito de este tipo de datos.

Para crear un `Symbol` no se usa la palabra `new` ya que no tienen un constructor:

```
let primerNombre = new Symbol();

//output
app.js:1 Uncaught TypeError: Symbol is not a constructor
    at new Symbol (<anonymous>)
    at app.js:1
```

Veamos el siguiente ejemplo:

```
let primerNombre = Symbol();
let persona = {}

persona[primerNombre] = 'Penélope';
console.log( persona.primerNombre);
console.log( persona[primerNombre]);

//output
undefined
Penélope
```

Observemos que la notación de punto para un `symbol` no es válida, deben usarse como si fuera una propiedad computada (entre []).

Lo anterior equivale a :

```
let primerNombre = Symbol();
let persona = {
  [primerNombre]: 'Penélope'
}
console.log( persona[primerNombre]);

//output
Penélope
```

A los Symbols les podemos poner un alias que se pone pasándole un parámetro cuando se crea:

```
let primerNombre = Symbol('Este es el primer nombre');
let segundoNombre = Symbol();
let persona = {
  [primerNombre]: 'Penélope'
}
console.log( primerNombre );
console.log( segundoNombre );


//output
Symbol(Este es el primer nombre)
Symbol()
```

Los símbolos que creemos siempre son diferentes, veamos:

```
let simbolo1 = Symbol('simbolo');
let simbolo2 = Symbol('simbolo');
console.log( simbolo1 == simbolo2 );
console.log( simbolo1 === simbolo2 );
console.log( Object.is(simbolo1, simbolo2) );

//output
false
false
false
```

Podemos recuperar el tipo de una variable para saber si es Symbol:

```
let simbolo1 = Symbol('simbolo');
console.log( typeof simbolo1 );

//output
symbol
```

No podemos manejar un Symbol como si fuera un string, no se puede convertir:

```
let simbolo1 = Symbol('simbolo');
console.log( "Mi símbolo: "  + simbolo1 );

//output
Uncaught TypeError: Cannot convert a Symbol value to a string
    at app.js:2
```

Tampoco lo permite si usamos backtick:

```
let simbolo1 = Symbol('simbolo');
console.log( `Mi símbolo: ${simbolo1}` );

//output
Uncaught TypeError: Cannot convert a Symbol value to a string
    at app.js:2
```

## Compartiendo símbolos - Symbol.for() y Symbol.keyFor()

El método `Symbol.for(key)` busca símbolos existentes en el registro de símbolos global que se han incluido en todo el tiempo de ejecución de la key dada y lo devuelve si se encuentra. De lo contrario, se crea un nuevo símbolo en el registro con esta key.

El método `Symbol.keyFor(sym)` recupera la clave de símbolo dado del registro de símbolo global.

Llagara un punto donde tengamos que compartir símbolos dentro de nuestro programa ya sea en el mismo archivo, en otro archivos, etc.

ES6 ha creado un sitio donde están todos los símbolos creados.

Cundo creamos un símbolo y queremos verificar que no exista o si existe usar el ya creado previamente usamos `Symbol.for()`, veamos el siguiente código:

```
let userID = Symbol.for("userID");
let objeto = {};

objeto[userID] = "12345";

console.log( objeto[userID] );
console.log( userID );

//output
12345
Symbol(userID)
```

Supongamos que en otro archivo de JS necesitamos hacer referencia a este símbolo:

```
let userID2 = Symbol.for("userID");

console.log( userID == userID2 );
console.log( userID === userID2 );
console.log( Object.is(userID, userID2) );

//output
true
true
true
```

Ya que los símbolos en este caso son exactamente los mismos puedo usar este segundo valor para recuperar los datos del objeto:

```
console.log( objeto[userID2] );
console.log( userID2 );

//output
12345
Symbol(userID)
```

Veamos el siguiente ejemplo se crea un símbolo en el repositorio global de símbolos con un identificador,
después se imprime el identificador asociado al símbolo.
Se hace exactamente lo mismo al crear un segundo símbolo pero como ya existe un símbolo en el repositorio toma ese valor, por lo que claramente podemos ver que los dos símbolos son exactamente iguales.
Por último se crea un tercer símbolo pero no toma en cuenta el repositorio por lo que este símbolo es completamente nuevo y no se ha incluido en el repositorio por lo que al buscarlo allí no existe y nos retorna `undefined`.

```
let id = Symbol.for("id unico");
console.log( Symbol.keyFor( id ));

let id2 = Symbol.for("id unico");
console.log( Symbol.keyFor( id2 ));

console.log( id === id2 );

let id3 = Symbol("id unico");
console.log( id3 );
console.log( Symbol.keyFor( id3 ));

//output
id unico
id unico
true
Symbol(id unico)
undefined
```

## Coerción de los símbolos

Recordemos que la Coerción es la conversión de un tipo a otro. Los símbolos son un poco inflexibles con la coerción.

Veamos el siguiente ejemplo de como interactúan los diferentes tipos de JS sin problema hasta que usamos symbol:

```
let id = Symbol.for("id");
let numero = 10;
let texto = "10"
let bool = true;

console.log( numero + texto );
console.log( numero + Number(texto) );
console.log( numero + bool );
console.log(id);
console.log( texto + id);

//output
1010
20
11
Symbol(id)
Uncaught TypeError: Cannot convert a Symbol value to a string
    at app.js:10
```

Si solo quisiéramos imprimir el valor de symbol en pantalla podríamos usar:

```
console.log(" Mi Symbol: ",  id);

//output
Mi Symbol:  Symbol(id)
```

Y así no me daría error.

Pero si realmente quiero recuperar en valor del symbol debo usar String() para hacer la coerción:

```
console.log(" Mi Symbol: " + String(id));

//output
Mi Symbol: Symbol(id)
```

Siempre se recupera con Symbol(...), este uso no es muy común recordemos que los symbols sirven para identificar cosas de manera única, son como una referencia o llave a una localidad de memoria y eso no lo deberíamos suman con algo más, pero si realmente lo necesitamos recuperamos ya hemos visto como hacerlo.

## Recuperando las propiedades símbolo

Veamos un ejemplo de como recuperamos las propiedades en un objeto con ES5:

```
let persona = {
  nombre: "Penélope",
  apellido: "Cruz",
  edad : 31,
  ["actividad"]:"actriz"
}

for( key in persona ){
  console.log( key, persona[key]);
}

//output
nombre Penélope
apellido Cruz
edad 31
actividad actriz
```

Hemos recuperado todas nuestras propiedades sin problema, pero que pasa si alguna o algunas propiedades del objeto están identificadas con un symbol:

```
let id = Symbol.for("id");
let rodando = Symbol.for("rodando");

let persona = {
  [id]: 1022,
  nombre: "Penélope",
  apellido: "Cruz",
  edad : 31,
  ["actividad"]:"actriz",
  [rodando]: "La última amiga..."
}

for( key in persona ){
  console.log( key, persona[key]);
}

//output
nombre Penélope
apellido Cruz
edad 31
actividad actriz
```

Observamos que las propiedades identificadas con symbol no se despliegan. Estas propiedades se encuentran en otro lado, para recuperarlas usamos el método `Object.getOwnPropertySymbols()`.

```
let id = Symbol.for("id");
let rodando = Symbol.for("rodando");

let persona = {
  [id]: 1022,
  nombre: "Penélope",
  apellido: "Cruz",
  edad : 31,
  ["actividad"]:"actriz",
  [rodando]: "La última amiga..."
}

let simbolos = Object.getOwnPropertySymbols(persona);
console.log( simbolos);

for(i in simbolos){
  console.log( simbolos[i]);
}

for(i in simbolos){
  console.log( Symbol.keyFor(simbolos[i]), persona[simbolos[i]]);
}

//output
[Symbol(id), Symbol(rodando)]

Symbol(id)
Symbol(rodando)

id 1022
rodando La última amiga...
```

De esta manera tenemos que hacer dos barridos uno para las propiedades normales y otra para las propiedades identificadas con Symbols:

```
let id = Symbol.for("id");
let rodando = Symbol.for("rodando");

let persona = {
  [id]: 1022,
  nombre: "Penélope",
  apellido: "Cruz",
  edad : 31,
  ["actividad"]:"actriz",
  [rodando]: "La última amiga..."
}

for( key in persona ){
  console.log( key, persona[key]);
}

let simbolos = Object.getOwnPropertySymbols(persona);
for(i in simbolos){
  console.log( Symbol.keyFor(simbolos[i]), persona[simbolos[i]]);
}

//output
nombre Penélope
apellido Cruz
edad 31
actividad actriz
id 1022
rodando La última amiga...
```

## Examen #7

:+1:
