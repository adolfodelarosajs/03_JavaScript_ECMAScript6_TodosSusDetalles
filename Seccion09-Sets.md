# Sets

## Introducción - Set

Históricamente JS sólo ha tenido un tipo de colección de datos los array.

Y aun que muchos digan que los objetos son una colección de pares de valores, no son realmente una colección de datos.

En otros leguajes tenemos:

1. Listas
2. Arreglos
3. Colecciones
4. Mapas
etc.

Pero claro, en JS hay librerías de terceros que permiten utilizar otro tipo de colecciones, pero al final terminan en arreglos u objetos comunes.

Supongamos que tenemos un arreglo de flores con:

Rosa
Lirio
Margarita
....
Orquídea

Si llegamos e introducimos otra Orquídea JS lo permite por que en el array puede haber elementos duplicados. Si quisiéramos no tener elementos repetidos tendríamos que validar elemento por elemento para ir validando si no es igual al que queremos insertar y si hay muchos elementos esta tarea puede ser pesada.

Los **Sets** son una lista ordenada de valores sin duplicados. Permiten un rápido acceso a la data que contienen.

## Creando sets - agregando items y buscando elementos

La manera de crear un Set es la siguiente:

```
let items = new Set();
console.log( items );

//output
Set(0) {}
```

Tenemos un set vacío en cual nace con varios métodos en su prototype.
Para añadir elementos a un set lo podemos hacer de la siguiente manera:

```
let items = new Set();
items.add(10);
items.add(9);
items.add(8);
items.add(7);
console.log( items );
console.log( items.size );

//output
Set(4) {10, 9, 8, 7}
4
```

¿Qué pasa si metemos valores repetidos?

```
let items = new Set();
items.add(10);
items.add(9);
items.add(8);
items.add(7);
items.add(7);
items.add(7);
items.add(7);
items.add(7);
items.add(7);
items.add(7);
console.log( items );
console.log( items.size );

//output
Set(4) {10, 9, 8, 7}
4
```
No marca ningún error simplemente los ignora, y esto:

```
let items = new Set();
items.add(10);
items.add(9);
items.add(8);
items.add(7);
items.add(7);
items.add("7");
items.add(7);
items.add(7);
items.add(7);
items.add(7);
console.log( items );
console.log( items.size );

//output
Set(5) {10, 9, 8, 7, "7"}
5
```

El elemento "7" lo ha insertado ya que internamente Set valida con `Object.is` si un elemento existe y como "7" no es igual al 7 que ya existía lo inserta.

Podemos incluir datos en un set desde su creación:

```
let items2 = new Set([10, 9, 8, 7, 7, 7, 7, 7, 7, 7, 7, 6, 5]);
console.log( items2 );
console.log( items2.size );

//output
Set(6) {10, 9, 8, 7, 6, 5}
6
```

Podemos buscar si existe un elemento en un set usando el método `has()`, nos regresara `true` si existe o `false` sino existe:

```
let items = new Set([10, 9, 8, 7, 7, 7, 7, 7, 7, 7, 7, 6, 5]);
console.log( items );
console.log( items.has(1) );
console.log( items.has(10) );
console.log( items.has("7") );
console.log( items.has(7) );

//output
Set(6) {10, 9, 8, 7, 6, 5}
false
true
false
true
```

## Removiendo valores

Para eliminar un elemento del set usamos el método `delete()`.

```
let items = new Set([10, 9, 8, 7, 7, 7, 7, 7, 7, 7, 7, 6, 5]);
console.log( items );
console.log( items.size );

items.delete(7);

console.log( items );
console.log( items.size );

//output
Set(6) {10, 9, 8, 7, 6, 5}
6
Set(5) {10, 9, 8, 6, 5}
5
```

Si intenta borrar un elemento que no existe no hace nada.

Para eliminar todos los elementos de un set usamos el métod `clear()`:

```
let items = new Set([10, 9, 8, 7, 7, 7, 7, 7, 7, 7, 7, 6, 5]);
console.log( items );
console.log( items.size );

items.clear(7);

console.log( items );
console.log( items.size );

//output
Set(6) {10, 9, 8, 7, 6, 5}
6
Set(0) {}
0
```

## forEach() - en los Sets

Recorriendo un Set con un `forEach()`:

```
let personas = new Set(["Penélope", "Salma", "Nikole"]);

personas.forEach( function ( valor, llave, setOriginal ){
  console.log( valor, llave, setOriginal );
  console.log( personas === setOriginal );
});

//output
Penélope Penélope Set(3) {"Penélope", "Salma", "Nikole"}
true
Salma Salma Set(3) {"Penélope", "Salma", "Nikole"}
true
Nikole Nikole Set(3) {"Penélope", "Salma", "Nikole"}
true
```

Dado que la función `forEach()` se utiliza en los Maps, vemos que recibe tres parámetros en la función que procesa (valor, llave, setOriginal), aquí estamos pintando los 3 en cada caso, pero si solo queremos recorrer los valores son mandamos imprimir eso:

```
let personas = new Set(["Penélope", "Salma", "Nikole"]);

personas.forEach( function ( valor, llave, setOriginal ){
  console.log( valor);
});

//output
Penélope
Salma
Nikole
```

## Convertir un Set en Array

Para pasar los elementos de un Set a un Array usamos el operador Spread:

```
let numeros = [1,2,3,4,5,6,7];
let setNumeros = new Set( numeros );
console.log( setNumeros );

let arrayNumeros = [...setNumeros];
console.log( arrayNumeros );

//output
Set(7) {1, 2, 3, 4, 5, …}
(7) [1, 2, 3, 4, 5, 6, 7]
```

Esto se podía aplicar en una función que elimine duplicados de un array:

```
function eliminarDuplicados( arreglo ){
  let set = new Set( arreglo );
  return [...set]
}
let numeros = [1,2,3,1,4,5,6,7,2,4,3,5,8,9,1,2,3,5,4];
let numerosUnicos = eliminarDuplicados(numeros);
console.log(numerosUnicos);

//output
(9) [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

Podríamos simplificar aún más nuestro código:

```
function eliminarDuplicados( arreglo ){
  return [...new Set( arreglo )]
}
let numeros = [1,2,3,1,4,5,6,7,2,4,3,5,8,9,1,2,3,5,4];
console.log(eliminarDuplicados(numeros));

//output
(9) [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## WeakSets

Trabajan de una manera similar a los Sets, pero el comportamiento interno es diferente.

Características principales

1. En un weekset ADD(), HAS(), REMOVE() dan un error si enviamos como parámetro algo que no sea un objeto.
2. No tienen manera de hacer repeticiones o ciclos for in.
3. Los weeksets no tienen keys(), values() por lo que no hay manera vía programación de saber cuantos elementos hay dentro.
4. No tienen un for-each tampoco.
5. No tienen propiedad size.

Sirven para almacenar referencias a los objetos que es su principal uso, veamos un ejemplo:

```
let set = new WeakSet();

let persona1={
  nombre: "Juan Carlos"
}

let persona2={
  nombre2: "María Perez"
}

set.add(persona1);
set.add(persona2);

console.log(set);

//output
WeakSet {{…}, {…}}
  __proto__: WeakSet
  [[Entries]]: Array(2)
  0: Object
  1: Object
  length: 2
```

## Examen #8 

:+1:
