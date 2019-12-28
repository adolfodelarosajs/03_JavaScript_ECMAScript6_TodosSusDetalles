# Prototipos más poderosos y Destructuración

## Cambiar el prototipo de un objeto

La POO (Programación Orientada a Objetos) de JS se encontraba basada en prototipos y no en clases. (ES6 introduce Clases)

Los prototipos son un conjunto de normas para integrar **Programación Orientada a Objetos** en JS. Pero con los prototipos, nosotros somos capaces de realizar tareas como:

* Herencia
* Encapsulamiento
* Abstracción
* Polimorfismo

Con el método `Object.setPrototypeOf()` puedo pasar el prototipado de un objeto a otro, veamos el ejemplo:

```
let gato = {
  sonido(){
  	console.log("Miau!");
  },
  chillido(){
  	console.log("MIAU!!!!!");
  }
}

let perro = {
  sonido(){
  	console.log("Guau!");
  }
}

let angora = Object.create( gato );

console.log( Object.getPrototypeOf(angora) === gato );
angora.sonido();
angora.chillido();

//Asigno el prototipo de perro a angora
Object.setPrototypeOf(angora, perro);

console.log( Object.getPrototypeOf(angora) === gato );
angora.sonido();
angora.chillido();

//output
true
Miau!
MIAU!!!!!

false
Guau!
Uncaught TypeError: angora.chillido is not a function
    at app.js:27
```

Las primeras tres salidas hacen referencia a un gato, después se cambia el prototipado para que angora sea un perro, por eso en el sonido sale Guau! y como chillido no existe en el perro me muestra el error.

## Acceso al prototipo con la referencia "SUPER"

En ES5 es complicado llamar a un método de una función heredada, de un objeto que viene del prototipo. Por ejemplo:

```
let persona = {
  saludar(){
    return "Hola";
  }
}

let amigo = {
  saludar(){
    return Object.getPrototypeOf(this).saludar.call(this) + ", saludos!!!";
  }
}

Object.setPrototypeOf( amigo, persona );
console.log( amigo.saludar() );

//output
Hola, saludos!!!
```

La línea `return Object.getPrototypeOf(this).saludar.call(this) + ", saludos!!!";` invoca al método `saludar` del objeto `persona` con `Object.getPrototypeOf(this).saludar.call(this)`, esto a cambiado con ES6 usando `super`, es decir lo anterior se reduce a `super.saludar()`:

```
let persona = {
  saludar(){
    return "Hola";
  }
}

let amigo = {
  saludar(){
    return super.saludar() + ", saludos!!!";
  }
}

Object.setPrototypeOf( amigo, persona );
console.log( amigo.saludar() );

//output
Hola, saludos!!!
```

Si llamamos directamente el `super` y no hay una "herencia" de un prototipo nos dará un error:

```
let persona = {
  saludar(){
    return "Hola";
  }
}

let amigo = {
  saludar(){
    return super.saludar() + ", saludos!!!";
  }
}

console.log( amigo.saludar() );

//output
Uncaught TypeError: (intermediate value).saludar is not a function
    at Object.saludar (app.js:9)
    at app.js:13
```

## Destructuración de objetos

La siguiente tarea en programación es muy común y tediosa:

```
var ajustes = {
  nombre : "Adolfo de la Rosa", 
  email : "adolfodelarosa2012@gmail.com",
  facebook : "https://www.facebook.com/adolfo.delarosa.566",
  instagram : "https://www.instagram.com/adolfodelarosa2012/",
  premium : true
}

let nombre = ajustes.nombre;
let email = ajustes.email;
let facebook = ajustes.facebook;
let instagram = ajustes.instagram;
let premium = ajustes.premium;

console.log(nombre);
console.log(email);
console.log(facebook);
console.log(instagram);
console.log(premium);

//output
Adolfo de la Rosa
adolfodelarosa2012@gmail.com
https://www.facebook.com/adolfo.delarosa.566
https://www.instagram.com/adolfodelarosa2012/
true
```

ES6 ha creado la Destructuración de Objetos para extraer las propiedades de un objeto en variables:

```
var ajustes = {
  nombre : "Adolfo de la Rosa", 
  email : "adolfodelarosa2012@gmail.com",
  facebook : "https://www.facebook.com/adolfo.delarosa.566",
  instagram : "https://www.instagram.com/adolfodelarosa2012/",
  premium : true
}

//let nombre = ajustes.nombre;
//let email = ajustes.email;
//let facebook = ajustes.facebook;
//let instagram = ajustes.instagram;
//let premium = ajustes.premium;

let { nombre, email, facebook, instagram, premium } = ajustes;

console.log(nombre);
console.log(email);
console.log(facebook);
console.log(instagram);
console.log(premium);

//output
Adolfo de la Rosa
adolfodelarosa2012@gmail.com
https://www.facebook.com/adolfo.delarosa.566
https://www.instagram.com/adolfodelarosa2012/
true
```

Nótese como se simplifica el código para recuperar las propiedades en variables. Si no inicializamos la destructuración nos da error:

```
var ajustes = {
  nombre : "Adolfo de la Rosa", 
  email : "adolfodelarosa2012@gmail.com",
  facebook : "https://www.facebook.com/adolfo.delarosa.566",
  instagram : "https://www.instagram.com/adolfodelarosa2012/",
  premium : true
}

let { nombre, email, facebook, instagram, premium };

//output
app.js:9 Uncaught SyntaxError: Missing initializer in destructuring declaration
```

La relación que hace es por medio de los nombres, por lo que si yo pongo un nombre no existente en el objeto, al no encontrarlo le asignara un valor `undefined`:

```
var ajustes = {
  nombre : "Adolfo de la Rosa", 
  email : "adolfodelarosa2012@gmail.com",
  facebook : "https://www.facebook.com/adolfo.delarosa.566",
  instagram : "https://www.instagram.com/adolfodelarosa2012/",
  premium : true
}

let { nombre, email, facebook, instagram, dePago } = ajustes;

console.log(nombre);
console.log(email);
console.log(facebook);
console.log(instagram);
console.log(dePago);

//output
Adolfo de la Rosa
adolfodelarosa2012@gmail.com
https://www.facebook.com/adolfo.delarosa.566
https://www.instagram.com/adolfodelarosa2012/
undefined
```

Si lo que realmente quiero hacer es recuperar el valor en una variable con otro nombre lo hago de la siguiente forma:

```
var ajustes = {
  nombre : "Adolfo de la Rosa", 
  email : "adolfodelarosa2012@gmail.com",
  facebook : "https://www.facebook.com/adolfo.delarosa.566",
  instagram : "https://www.instagram.com/adolfodelarosa2012/",
  premium : true
}

let { nombre, email, facebook, instagram, premium:dePago } = ajustes;

console.log(nombre);
console.log(email);
console.log(facebook);
console.log(instagram);
console.log(dePago);

//output
Adolfo de la Rosa
adolfodelarosa2012@gmail.com
https://www.facebook.com/adolfo.delarosa.566
https://www.instagram.com/adolfodelarosa2012/
true
```

Puedo incluir variables que no son propiedades del objeto e inicializar su valor:

```
var ajustes = {
  nombre : "Adolfo de la Rosa", 
  email : "adolfodelarosa2012@gmail.com",
  facebook : "https://www.facebook.com/adolfo.delarosa.566",
  instagram : "https://www.instagram.com/adolfodelarosa2012/",
  premium : true
}

let { nombre, email, facebook, instagram, twitter = "https://twitter.com/Adolfodelarosa", premium:dePago } = ajustes;

console.log(nombre);
console.log(email);
console.log(facebook);
console.log(instagram);
console.log(twitter);
console.log(dePago);

//output
Adolfo de la Rosa
adolfodelarosa2012@gmail.com
https://www.facebook.com/adolfo.delarosa.566
https://www.instagram.com/adolfodelarosa2012/
https://twitter.com/Adolfodelarosa
true
```

Si se pone un valor por defecto a una variable que si tiene propiedad en el campo usa el valor de la propiedad ignorando el valor por defecto:

```
var ajustes = {
  nombre : "Adolfo de la Rosa", 
  email : "adolfodelarosa2012@gmail.com",
  facebook : "https://www.facebook.com/adolfo.delarosa.566",
  instagram : "https://www.instagram.com/adolfodelarosa2012/",
  twitter : "https://twitter.com/Adolfodelarosa",
  premium : true
}

let { nombre, email, facebook, instagram, twitter = "https://twitter.com/MrTrump", premium:dePago } = ajustes;

console.log(nombre);
console.log(email);
console.log(facebook);
console.log(instagram);
console.log(twitter);
console.log(dePago);

//output
Adolfo de la Rosa
adolfodelarosa2012@gmail.com
https://www.facebook.com/adolfo.delarosa.566
https://www.instagram.com/adolfodelarosa2012/
https://twitter.com/Adolfodelarosa
true
```

El orden en que pongo las variables no debe ser el mismo orden en que estan puestos las propiedades en el objeto, simplemente lo que hace es relacionar los nombres no importando el orden:

```
var ajustes = {
  nombre : "Adolfo de la Rosa", 
  email : "adolfodelarosa2012@gmail.com",
  facebook : "https://www.facebook.com/adolfo.delarosa.566",
  instagram : "https://www.instagram.com/adolfodelarosa2012/",
  premium : true
}

let { facebook, email, instagram, premium:dePago, nombre,   } = ajustes;

console.log(nombre);
console.log(email);
console.log(facebook);
console.log(instagram);
console.log(dePago);

//output
Adolfo de la Rosa
adolfodelarosa2012@gmail.com
https://www.facebook.com/adolfo.delarosa.566
https://www.instagram.com/adolfodelarosa2012/
true
```

Tampoco es necesario recuperar todas las propiedades puedo poner solo las variables que me interesen:

```
var ajustes = {
  nombre : "Adolfo de la Rosa", 
  email : "adolfodelarosa2012@gmail.com",
  facebook : "https://www.facebook.com/adolfo.delarosa.566",
  instagram : "https://www.instagram.com/adolfodelarosa2012/",
  premium : true
}

let { facebook, nombre  } = ajustes;

console.log(nombre);
console.log(facebook);

//output
Adolfo de la Rosa
https://www.facebook.com/adolfo.delarosa.566
```

## Destructuración de objetos anidados

Veamos como podemos destructurar objetos anidados:

```
let autoGuardado = {
  archivo: "app.js",
  cursor: {
    linea: 7,
    columna: 16
  },
  ultimoArchivo:{
    archivo: "index.html",
    cursor: {
      linea: 8,
      columna: 20
    }
  },
  otroNodo:{
  	subNodo:{
      cursor:{
        linea:11,
        columna:11
      }
  	}
  }
};

let { cursor } = autoGuardado;
console.log( cursor );

let { ultimoArchivo:{cursor:cursorUltimoArchivo} } = autoGuardado;
console.log( cursorUltimoArchivo );

let { otroNodo: { subNodo: { cursor: cursorSubNodo }}} = autoGuardado;
console.log( cursorSubNodo );

//FormaNormal
let cursorSubNodo2 = autoGuardado.otroNodo.subNodo.cursor;
console.log( cursorSubNodo2 );
```

Aquí estamos recuperando solo parte del objeto (cursor), en algunos casos es necesario ponerle un nombre (:nombre) para poder identificarlo.

Vemos que cuando lo que queremos recuperar la sintaxis se vuelve más enredada y tal vez la forma normal de recuperar el dato es más clara.

## Destructuración de arreglos

También podemos destructurar arrays, veamos el ejemplo:

```
let frutas = ["plátano", "pera", "uva"];

let [ fruta1, fruta2 ] = frutas;

console.log(fruta1);
console.log(fruta2);

//output
plátano
pera
```

Como podemos ver no es necesario recuperar todos los elementos, si lo que quisiera es recuperar el tercer elemento es necesario poner las comas para identificar su sitio:

```
let frutas = ["plátano", "pera", "uva"];
let [ , , fruta3 ] = frutas;
console.log(fruta3);

//output
uva
```

Si el nombre que utilizamos para hacer la destructuración contiene algun valor lo sustituye:

```
let frutas = ["plátano", "pera", "uva"];
let otraFruta = "manzana"
[ otraFruta ] = frutas;
console.log(otraFruta);

//output
plátano
```

Existe un problema clásico donde tenemos dos variables y queremos intercambiar sus valores, tenemos que echar mano de una variable auxiliar para hacerlo:

```
let a = 10;
let b = 20;
let aux;

console.log(a);
console.log(b);

aux = a;
a = b;
b = aux;

console.log(a);
console.log(b);

//output
10
20
20
10
```

Usando la destructuración de arreglos este problema se soluciona de una forma sencilla:

```
let a = 10;
let b = 20;
let aux;

console.log(a);
console.log(b);

aux = a;
a = b;
b = aux;

console.log(a);
console.log(b);

[a,b] = [b,a];

console.log(a);
console.log(b);

//output
10
20
20
10
10
20
```

## Destructuración de arreglos anidados

Consiste en destructurar arrays que estén incluidos en otros arrays:

```
let colores = ["rojo", ["verde", "amarillo"], "morado", "naranja"];
let [ color1, [color2] ] = colores;
console.log( color1 );
console.log( color2 );

//output
rojo
verde
```

Con la destructuración de arrays podemos usar el operador REST, veamos:

```
let colores = ["rojo", "verde", "amarillo", "morado", "naranja"];
let [ colorPrincipal, colorSecundario, ...restoColores ] = colores;
console.log( colorPrincipal );
console.log( colorSecundario );
console.log( restoColores );

//output
rojo
verde
["amarillo", "morado", "naranja"]
```

Si usamos el operador rest pero no hay elementos, tendremos un array vacío:

```
let colores = ["rojo", "verde"];
let [ colorPrincipal, colorSecundario, ...restoColores ] = colores;
console.log( colorPrincipal );
console.log( colorSecundario );
console.log( restoColores );

//output
rojo
verde
[]
```

## Valores por defecto en la destructuración

Los valores por defecto son valores que se dan a una variable para que en caso de no existir en los arrays u objetos asignar ese valor, pero si existe toman el valor original ignorando el por defecto.

En los arrays:

```
let frutas = ["Mango", "Uva"];
let [ fruta1, fruta2 = "Manzana", fruta3 = "Sandia"] = frutas;

console.log(fruta1);
console.log(fruta2);
console.log(fruta3);

//output
Mango
Uva
Sandia
```

En los objects:

```
let frutas = {
  fruta1: "Mango",
  fruta2: "Uva"
}
let { fruta1, fruta2 = "Manzana", fruta3 = "Sandia" } = frutas;

console.log(fruta1);
console.log(fruta2);
console.log(fruta3);

//output
Mango
Uva
Sandia
```

## Destructuración de parámetros

Un problema que se tiene en JS cuando tenemos que utilizar funciones que no hemos programado nosotros y que reciben un objeto es saber que propiedades tiene ese objeto, si nosotros la hicimos es más fácil:

```
function crearJugador( nickname, opciones ){
  opciones = opciones || {};

  let hp = opciones.hp,
      sp = opciones.sp,
      clase = opciones.clase;
  console.log(nickname, hp, sp, clase);
  // código para crear el jugador...
}

crearJugador( "Strider", {
  hp: 100,
  sp: 50,
  clase: "Mago"
});

//output
Strider 100 50 Mago
```

Una forma de solucionar esto es que en los parámetros de la función pongamos el objeto destructurado así podríamos identificar que se debe enviar.

```
function crearJugador( nickname, { hp, sp, clase } ){
  
  console.log(nickname, hp, sp, clase);
  // código para crear el jugador...
}

crearJugador( "Strider", {
  hp: 100,
  sp: 50,
  clase: "Mago"
});

//output
Strider 100 50 Mago
```

Pero qué pasa cuando no se pasa el parámetro de las opciones, esto de no pasar un argumento no daba ningún problema en JS, pero con las destructuraciones si que lo da:

```
function crearJugador( nickname, { hp, sp, clase } ){
  
  console.log(nickname, hp, sp, clase);
  // código para crear el jugador...
}

crearJugador( "Strider");

//output
app.js:1 Uncaught TypeError: Cannot destructure property `hp` of 'undefined' or 'null'.
    at crearJugador (app.js:1)
    at app.js:7
```

Una posible solución para evitar el error es asignarle un valor por defecto al objeto (un objeto vacío):

```
function crearJugador( nickname, { hp, sp, clase } = {} ){
  
  console.log(nickname, hp, sp, clase);
  // código para crear el jugador...
}

crearJugador( "Strider");

//output
Strider undefined undefined undefined
```

Al poner por default el objeto vacío llena cada propiedad con `undefined`.

Podría en lugar de asignarle un objeto vacío uno con los valores por defecto:

```
function crearJugador( nickname, 
  { hp, sp, clase } = {hp: 100, sp:50, clase:"Mago" } ){
  
  console.log(nickname, hp, sp, clase);
  // código para crear el jugador...
}

crearJugador( "Strider");

//output
Strider 100 50 Mago
```

Coge los valores por defecto y si le mando valores:

```
function crearJugador( nickname, 
  { hp, sp, clase } = {hp: 100, sp:50, clase:"Mago" } ){
  
  console.log(nickname, hp, sp, clase);
  // código para crear el jugador...
}

crearJugador( "Strider", {
  hp: 500,
  sp: 100,
  clase: "Warrior"
});

//output
Strider 500 100 Warrior
```

Si tiene valores por defecto pero le mando valores toma los valores enviados.

## Examen #6 

:+1:
