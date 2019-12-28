# Nuevos métodos con cadenas de caracteres – Strings

## Segmentos de caracteres - startsWith - endsWith - includes

### startsWith

Antes para saber si una palabra empezaba con una letra había que apoyarse en métodos auxiliares para lograrlo. Con esta nueva función del ES6 lo podemos saber directamente inclusive nos sirve para saber si comienza con un conjunto de letras o palabras:

```
var saludo = "Hola Mundo!";

console.log( saludo.substr(0,1) === "H" );
console.log( saludo.match(/^H/)[0]==="H" );

console.log( saludo.startsWith("H"));
console.log( saludo.startsWith("Hola"));

//output
true
true
true
true
```

### endsWith

Nos va a permitir ver si una cadena termina con una secuencia de caracteres, regresando `true` si se cumple o `false` si no se cumple:

```
var saludo = "Hola Mundo!";
console.log( saludo.endsWith("Mundo!"));
console.log( saludo.endsWith("."));

//output 
true
false
```

### includes

Antes para saber si una cadena contenía un texto usábamos la función `indexOf("texto")` si lo encontraba regresaba 0 y si no -1.

Se ha incluido la función `includes` para saber si contiene o no un texto y nos regresa `true` cuando lo contiene y `false` cuando no.

```
var saludo = "Hola Mundo!";

console.log( saludo.includes("Mundo"));
console.log( saludo.includes("World"));

//output
true
false
```

Las tres funciones anteriores pueden contener un segundo parámetro numérico que indica a partir de que posición buscar o en el caso de `edsWith` donde finaliza:

```
var saludo = "Hola Mundo Cruel!";

console.log( saludo.startsWith("Mundo",5) );
console.log( saludo.includes("Cruel", 7) );
console.log( saludo.endsWith("Mundo",10) );

//output
true
true
true
```

## Repeticiones de strings - Repeat

La función `repeat()` repite un texto las veces que se indique:

```
let texto ="Hola"
console.log( texto.repeat(2) );
console.log( "Hello".repeat(2) );

//output 
HolaHola
HelloHello
```

Vamos a suponer que queremos tener las siguientes columnas:

```
Cristiano   | 879789899
Messi       | 999999999
Neymar      | 765476534
```

Es decir después del nombre se insertan espacios en blanco según el tamaño del nombre para que quede todo alineado.

```
const ESPACIOS = 12;

let nombres = ["Cristiano", "Messi", "Neymar"];
let telefonos = ["879789899", "999999999", "765476534"];

for ( i in nombres ){
  let dif = ESPACIOS - nombres[i].length;
  console.log( nombres[i] + " ".repeat(dif) + " | " + telefonos[i]);
}

//output
Cristiano    | 879789899
Messi        | 999999999
Neymar       | 765476534
```

## Plantillas literales - Templates Literal

En JS siempre hemos tenido una funcionalidad limitada en el uso de los strings comparados con otros lenguajes, es por eso que surgen los templates.

Un ejemplo en PHP.

```
$nombre = "Fernando";
$miTexto = "Saludos $nombre";

//output
Saludos Fernando
```

En JS sería así:

```
var nombre = "Fernando";
var telefono = "999-88-11-32"

var mensaje = "El teléfono de " + nombre + " es " + telefono;

//output
El teléfono de Fernando es 999-88-11-32
```

A veces el crear este tipo de mensajes se complica cuando se deben incluir comillas, apóstrofes u otros delimitadores. Sin contar que hay que escapar dichos caracteres que nos pueden romper nuestro string.

En ES6 se incluyerón los Template Literals, para los cuales usamos el caracter \`\`.

```
function obtenerNombre(){
  return "Salma Hayek";
}

let nombre = "Penélope"
let apellido = "Cruz";

let nombreCompleto = "Nombre completo: " + nombre + " " + apellido;
console.log( nombreCompleto );

let nombreCompleto2 = `Nombre completo: ${nombre} ${apellido}`;
console.log( nombreCompleto2 );

let nombreCompleto3 = `Nombre completo: ${obtenerNombre()}`;
console.log( nombreCompleto3 );

//output
Nombre completo: Penélope Cruz
Nombre completo: Penélope Cruz
Nombre completo: Salma Hayek
```

Entre las llaves se pueden poner expresiones validad JS que se evaluaran antes de poner el resultado:

```
let mensaje = `El cuadrado de ${ 3 + 4} es ${ (3 + 4) * (3 + 4)}`;
console.log(mensaje);

//output
El cuadrado de 7 es 49
```

Cuando tenemos múltiples líneas los Templates Literals también nos simplifican la vida:

```
let multiLinea = "<h1 class='red'>Título</h1>\n"
               + "<p>Hola Mundo</p>";

console.log( multiLinea);

let multiLinea2 = `<h1 class='red'>Título</h1>
<p>Hola Mundo</p>`;

console.log( multiLinea2);

//output
<h1 class='red'>Título</h1>
<p>Hola Mundo</p>

<h1 class='red'>Título</h1>
<p>Hola Mundo</p>
```

## Templates con tags

Los Templates Literals tienen tags asociados a una función que nos permite validar los datos o dar un formato antes de asignar la cadena a una variable.

```
let unidades = 5, 
    costo_unitario = 10;

let mensaje = `${unidades} lápices cuestan ${unidades * costo_unitario} euros`;

console.log( mensaje );

//output
5 lápices cuestan 50 euros
```

Podemos asociar a nuestro Template Literal un tag como sigue:

`let mensaje = etiqueta`${unidades} lápices cuestan ${unidades * costo_unitario} euros`;`

El tag `etiqueta` representa una función que se va a disparar justo cuando se este construyendo el Template Literal, como esa función aún no la hemos definido, si ejecutamos el programa nos dirá justo eso:

```
Uncaught ReferenceError: etiqueta is not defined
    at app.js:4
``` 

Por lo cual vamos a definir la función `etiqueta`.

```
function etiqueta(){
}

let unidades = 5, 
    costo_unitario = 10;

let mensaje = etiqueta`${unidades} lápices cuestan ${unidades * costo_unitario} euros`;
console.log( mensaje );

//output
undefined
```

Como la función no regresa nada la salida que tenemos es `undefined`.

Si la función retornara algo, sería lo que se muestre:

```
function etiqueta(){
  return "Mensaje XXXXX";
}

let unidades = 5, 
    costo_unitario = 10;

let mensaje = etiqueta`${unidades} lápices cuestan ${unidades * costo_unitario} euros`;

console.log( mensaje );

//output
Mensaje XXXXX
```

Todas las funciones en JS aun que no tengan puestos los parámetros explícitamente reciben un arreglo de parámetros que se almacenan en el objeto `arguments`:

```
function etiqueta(){
  console.log(arguments);
  return "Mensaje XXXXX";
}

let unidades = 5, 
    costo_unitario = 10;
let mensaje = etiqueta`${unidades} lápices cuestan ${unidades * costo_unitario} euros`;
console.log( mensaje );

//output
Arguments(3) [Array(3), 5, 50, callee: ƒ, Symbol(Symbol.iterator): ƒ]  0: (3)
	0: ""
	1: " lápices cuestan "
	2: " euros"
	length: 3
    raw: (3) ["", " lápices cuestan ", " euros"]
    __proto__: Array(0)
  1: 5
  2: 50
  callee: ƒ etiqueta()
  length: 3
  Symbol(Symbol.iterator): ƒ values()
  __proto__: Object

Mensaje XXXXX
```

Como se puede observar `arguments` es un array que en su primera posición contiene un array que contiene en cada uno de sus elementos los textos que se encuentran en el Template Literals:

```
0: ""
1: " lápices cuestan "
2: " euros"
```

La primera cadena siempre será un texto vacío.

Las posiciones 1 y 2 de `arguments` son los literales que se tenían con ${} en nuestro Template Literals, en este caso:

```
5
50
```

Esta forma de recuperar los datos es un poco complicada existe otra forma un poco más sencilla`, que consiste en poner explícitamente el nombre de los parámetros donde queremos recibir la información:

```
function etiqueta(literales, ...substituciones){
  console.log(literales);
  console.log(substituciones);
  return "Mensaje XXXXX";
}

let unidades = 5, 
    costo_unitario = 10;
let mensaje = etiqueta`${unidades} lápices cuestan ${unidades * costo_unitario} euros`;
console.log( mensaje );

//output
(3) ["", " lápices cuestan ", " euros", raw: Array(3)]
(2) [5, 50]
Mensaje XXXXX
```

Una vez que ya recuperamos los arreglos de los datos podemos manipularlos de alguna manera para modificar el mensaje de salida:

```
function etiqueta(literales, ...substituciones){
  var resultado = "";
  for (let i=0; i < literales.length; i++){
    resultado+= literales[i];
    if( i == 0 )
      resultado+= "<" + substituciones[i] + ">"
    if( i == 1 )
      resultado+= substituciones[i] + "€"
    if (i===2){
      substituciones[2]="";
    }
  }
  return resultado
}

let unidades = 5, 
    costo_unitario = 10;
let mensaje = etiqueta`${unidades} lápices cuestan ${unidades * costo_unitario} euros`;
console.log( mensaje );

//output
<5> lápices cuestan 50€ euros
```

## Usando valores "raw" (crudos) en templates literales

Existe el tag `String.raw` que podemos asociar a un Template Literal para recuperar su valor tal cual sin interpretar los caracteres de escape.

```
let mensaje = `Hola \nMundo\r`;
let mensaje2 = String.raw `Hola \nMundo\r`;

console.log(mensaje);
console.log(mensaje2);

//output
Hola 
Mundo

Hola \nMundo\r
```

## Examen #2 

:+1:
