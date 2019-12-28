# Adiciones a los Objetos

## Extensiones de objetos literales

Los objetos en ES6 han recibido muchas mejoras, ya que casi cualquier cosa JS es algún tipo de objeto.

#### Objetos literales en ES6 (nombre/valor)

Este tipo de estructura es el patrón más utilizado en JS. (JSON se construyó de esta sintaxis)

Dichos objetos se encuentran en casi cualquier script en el internet, casi el 99% de los programas los utilizan en algún punto.

Vamos a ver un ejemplo en ES5:

```
function crearPersona( nombre, apellido, edad ){
  return {
    nombre: nombre,
    apellido: apellido,
    edad: edad
  }
}

var persona = crearPersona("Penélope", "Cruz", 30);
console.log( persona );

//output
{nombre: "Penélope", apellido: "Cruz", edad: 30}
```

En ES6 lo podemos simplificar:

```
function crearPersona( nombre, apellido, edad ){
  return {
    nombre,
    apellido,
    edad
  }
}

var persona = crearPersona("Penélope", "Cruz", 30);
console.log( persona );

//output
{nombre: "Penélope", apellido: "Cruz", edad: 30}
```

Nótese que al momento de retornar, si el nombre y el valor tiene los mismos nombres se pueden obviar poniéndolo solamente una vez.

## Métodos concisos

Veamos un ejemplo de un método en ES5:

```
var persona = {
  nombre: "Penélope",
  getNombre: function(){
  	console.log( this.nombre );
  }
}
persona.getNombre();

//output
Penélope
```

Para tener un método conciso basta con quitar `: function` de la declaración del método y listo:

```
var persona = {
  nombre: "Penélope",
  getNombre(){
  	console.log( this.nombre );
  }
}
persona.getNombre();

//output
Penélope
```

## Nombres de propiedades computadas o procesadas

Ejemplo ES5:

```
var persona = {};
var apellido = "apellido";

persona["primer nombre"] = "Penélope";
persona[apellido] = "Cruz";

console.log(persona["primer nombre"]);
console.log(persona[apellido]);

//output
Penélope
Cruz
```

Es lo mismo que lo siguiente:

```
var persona = {
  "primer nombre": "Penélope",   
  "apellido": "Cruz"
};
console.log(persona["primer nombre"]);
console.log(persona["apellido"]);

//output
Penélope
Cruz
```

La notación de punto no sería posible para obtener el nombre de la persona por espacio en blanco que contiene, por eso usamos los corchetes.

Con ES6 puedo utilizar la notación de corchetes en el nombre de la propiedad, veamos el siguiente ejemplo:

```
let apellido = "primer apellido";

var persona = {
  "primer nombre": "Penélope",   
  [apellido]: "Cruz"
};
console.log(persona["primer nombre"]);
console.log(persona[apellido]);

//output
Penélope
Cruz
```

Esto tiene sus ventajas, por ejemplo si en los campos de una BD se maneja un sufijo:

```
var suFijo = "_IKEA";
var persona = {
   ["primer" + suFijo]: "Penélope",
   ["segundo" + suFijo]: "Salma"
}
console.log( persona["primer_IKEA"] );
console.log( persona["segundo" + suFijo] );

//output
Penélope
Salma
```

## Nuevo método: Object.is()

Normalmente usamos `==` o ``===` para ver si dos valores son iguales, en ES6 tenemos un nuevo método para comparar `Object.is()`.

```
console.log( +0 == -0 );
console.log( +0 === -0);
console.log( Object.is(+0, -0) );

console.log("==========");

console.log( NaN == NaN );
console.log( NaN === NaN );
console.log( Object.is( NaN, NaN ) );

console.log("==========");

console.log(5 == 5 );
console.log(5 == "5" );

console.log(5 === 5 );
console.log(5 === "5" );

console.log( Object.is(5,5) );
console.log( Object.is(5,"5") );

//output
true
true
false
==========
false
false
true
==========
true
true
true
false
true
false
```

## Nuevo método: Object.assign()

El nuevo método `Object.assign()` nos permite transmitir todas las propiedades de un objeto a otro. Primero vamos a ver un ejemplo de cómo se hace con ES5:

```
function transmitirPropiedades( objReceptor, objTransmisor ){
  Object.keys( objTransmisor ).forEach( function(key){
    objReceptor[key] = objTransmisor[key];
  });
  return objReceptor;
}

var objReceptor = {};
var objTransmisor = { nombre: "mi-archivo.js"};

console.log( transmitirPropiedades( objReceptor, objTransmisor ) );

//output
{nombre: "mi-archivo.js"}
```
En ES6 se han incluido los **get**, vamos a usarlos para escribir nuestro anterior codigo pero con get:

```
function transmitirPropiedades( objReceptor, objTransmisor ){
  Object.keys( objTransmisor ).forEach( function(key){
    objReceptor[key] = objTransmisor[key];
  });
  return objReceptor;
}

var objReceptor = {};
var objTransmisor = { get nombre(){ return "mi-archivo.js"} };

console.log( objTransmisor.nombre );
console.log( transmitirPropiedades( objReceptor, objTransmisor ) );

//output
mi-archivo.js
{nombre: "mi-archivo.js"}
```

Nótese que aun que se transfirieron la propiedad de un objeto a otro los dos objetos en su estructura son diferentes:

```
objReceptor
 {nombre: "mi-archivo.js"}
   nombre: "mi-archivo.js"
   __proto__: Object
objTransmisor
 {}
   nombre: (...)
   get nombre: ƒ nombre()
   arguments: (...)
   caller: (...)
   length: 0
   name: "get nombre"
   __proto__: ƒ ()
   [[FunctionLocation]]: 
   [[Scopes]]: Scopes[1]
   __proto__: Object
```

El `objetoTransmisor` cuenta con el método `get nombre`, pero el `objReceptor` sólo recibió la propiedad nombre.

Bueno usando ES6 ya no necesitamos que programemos la función `transmitirPropiedades()` ya que es precisamente lo que hace el método `Object.assign()`.

```
var objReceptor = {};
var objTransmisor = { get nombre(){ return "mi-archivo.js"} };

console.log( Object.assign( objReceptor, objTransmisor ) );

//output
{nombre: "mi-archivo.js"}
```

Nótese como se reduce nuestro código.

## Orden de enumeración de las propiedades de los objetos

Veamos el siguiente objeto:

```
var objeto = {
  nombre: "Penélope",
  apellido: "Cruz",
  edad: 30,
  bio: "Actriz",
  2:"Nada en particular"
}
```

Antes de ES6 el orden era como cada navegador lo quería, con ES6 el orden es:

* 2
* nombre
* apellido
* edad
* bio

Se siguen las siguientes reglas:

* Todas las llaves van en orden ascendente.
* Todas las llaves tipo string, van ordenadas en la manera que fueron agregadas al objeto.
* Todos los símbolos en el orden que fueron agregados al objeto.

Veamos el siguiente código y veamos el orden en que son desplegadas las propiedades:

```
var objeto = {
  c: 1,
  0: 1,
  x: 1,
  15: 1,
  r: 1,
  3: 1,
  b: 1
}

objeto.d = 1;
objeto["2"] = 1;
objeto["a"] = 1;

console.log(objeto);
console.log( Object.getOwnPropertyNames( objeto ).join(", "));
console.log( Object.keys( objeto ));
console.log( JSON.stringify( objeto ));
for( i in Object.keys( objeto )){
  console.log( Object.keys( objeto)[i]);
}

//output
{0: 1, 2: 1, 3: 1, 15: 1, c: 1, x: 1, r: 1, b: 1, d: 1, a: 1}
0, 2, 3, 15, c, x, r, b, d, a
(10) ["0", "2", "3", "15", "c", "x", "r", "b", "d", "a"]
{"0":1,"2":1,"3":1,"15":1,"c":1,"x":1,"r":1,"b":1,"d":1,"a":1}
0
2
3
15
c
x
r
b
d
a
```

Como podemos observar todas las salidas siguen el nuevo estándar para ordenar los campos de los objetos:

## Examen #5 

:+1:
