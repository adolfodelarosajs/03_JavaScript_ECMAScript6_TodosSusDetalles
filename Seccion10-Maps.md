# Maps

## Introducción a los Maps

Nuevo tipo de colección de datos que por mucho tiempo fue necesaria en JS.

Es una colección de datos key/value pair.

Al igual que Strong Set o Sets... los Maps tienen:

1. has()
2. delete()
3. clear()
4. size()
5. iteracciones

## Mapas y sus métodos

La forma de crear un map vacio es la siguiente:

```
let mapa = new Map();
console.log( mapa );

//output
Map(0) {}
```

Para añadir elementos al mapa usamos el método `set()`, podemos insertar cualquier primitivo u objeto en el map.

```
let mapa = new Map();

mapa.set( "nombre", "Penélope");
mapa.set( "edad", 33);

console.log( mapa );
console.log( mapa.size );

//output
Map(2) {"nombre" => "Penélope", "edad" => 33}
2
```

Obsérvese que un map es muy parecido a un object con la diferencia que la key va entre comillas.

Si queremos recuperar el valor de algún elemento del map usamos el método 'get(key)' y si queremos saber si una llave existe en el map usamos el método `has(key)`, veamos el código:

```
console.log( mapa.get("nombre") );
console.log( mapa.get("edad") );

console.log( mapa.has("nombre") );
console.log( mapa.has("apellido") );

//output
Penélope
33
true
false
```

Para eliminar un elemento del mapa usamos el método `delete(key)`:

```
let mapa = new Map();

mapa.set( "nombre", "Penélope");
mapa.set( "edad", 33);

console.log( mapa );
console.log( mapa.size );

mapa.delete("edad");

console.log( mapa.has("edad") );
console.log( mapa.get("edad") );
console.log( mapa );
console.log( mapa.size );

//output
{"nombre" => "Penélope", "edad" => 33}
2
false
undefined
Map(1) {"nombre" => "Penélope"}
1
```

Para eliminar todos los elementos de un mapa usamos el método `clear()`:

```
let mapa = new Map();

mapa.set( "nombre", "Penélope");
mapa.set( "edad", 33);

console.log( mapa );
console.log( mapa.size );

mapa.clear();

console.log( mapa );
console.log( mapa.size );

//output
Map(2) {"nombre" => "Penélope", "edad" => 33}
2
Map(0) {}
0
```

Si en un mapa no se define el valor o la llave de un elemento los datos se rellenan con `undefined`:

```
let mapa = new Map();

mapa.set( "nombre", "Penélope");
mapa.set( "edad", 33);
mapa.set( "apellido" );
mapa.set( );

console.log( mapa );
console.log( mapa.size );
console.log( mapa.get("apellido") );

//output
Map(4) {"nombre" => "Penélope", "edad" => 33, "apellido" => undefined, undefined => undefined}
4
undefined
```

También permite poner objetos como llave y como valor:

```
let mapa = new Map();

mapa.set( {nombre: "Penélope"}, { profesion: "Actriz"});
mapa.set( {}, {arreglo: "Vacío"});

console.log( mapa );
console.log( mapa.size );
console.log( {nombre: "Penélope"} );

//output
Map(2) {{…} => {…}, {…} => {…}}
2
{nombre: "Penélope"}
```

## Inicializaciones de los mapas

Para inicializar el mapa en el momento en que se crea debemos mandar como parámetro un array, que contendrá cada par key/value dentro de un array, esto es:

```
let mapa = new Map([ ["nombre", "Penélope"], ["edad", 33] ]);
console.log( mapa );

//output
Map(2) {"nombre" => "Penélope", "edad" => 33}
```

Como ya habíamos visto los mapas aceptan pares de valores que parece no tienen sentido como el siguiente ejemplo:

```
let mapa = new Map([ ["nombre", "Penélope"], ["edad", 33], [null, 12345], [undefined] ]);
console.log( mapa );
console.log( mapa.get(null) );
console.log( mapa.get(undefined) );

//output
Map(4) {"nombre" => "Penélope", "edad" => 33, null => 12345, undefined => undefined}
12345
undefined
```

## forEach() De los mapas

Nos permite recorrer los elementos de un map:

```
let mapa = new Map([ ["nombre", "Penélope"], ["edad", 33] ]);

mapa.forEach( function( valor, llave, mapaOrigen ){
  console.log(`Llave: ${llave}, valor: ${valor}`);
  console.log(mapaOrigen);
});

//output
Llave: nombre, valor: Penélope
Map(2) {"nombre" => "Penélope", "edad" => 33}
Llave: edad, valor: 33
Map(2) {"nombre" => "Penélope", "edad" => 33}
```

Vamos a recorrer el mapa usando una función de flecha dentro del forEach:

```
let mapa = new Map([ ["nombre", "Penélope"], ["edad", 33] ]);
mapa.forEach( (valor, llave) => console.log(`${llave}: ${valor}`) );

//output
nombre: Penélope
edad: 33
```

Si solo quiero trabajar con los valores:

```
let mapa = new Map([ ["nombre", "Penélope"], ["edad", 33] ]);
mapa.forEach( valor => console.log(`${valor}`) );

//output
Penélope
33
```

## Nuevo ciclo - FOR-OF

La forma tradicional de recorrer un array es la siguiente:

```
let numeros = [100,20,30,50,200];

for( let i=0; i < numeros.length; i++ ){
  console.log( numeros[i] );
}

//output
100
20
30
50
200
```

Una forma más condensada es usando el for-in:

```
let numeros = [100,20,30,50,200];

for( let i in numeros ){
  console.log( numeros[i] );
}

//output
100
20
30
50
200
```

Con ES6 ha nacido una nueva forma de recorrer las colecciones de datos:

```
let numeros = [100,20,30,50,200];

for( let numero of numeros ){
  console.log( numero );
}

//output
100
20
30
50
200
```

Veamos un ejemplo para recorrer un array de objetos:

```
let personas = [
  {nombre: "Penélope", edad: 33 },
  {nombre: "Salma", edad: 35 },
  {nombre: "Nikole", edad: 40 },  
];

for( let persona of personas ){
  console.log( persona.nombre, persona.edad );
}

//output
Penélope 33
Salma 35
Nikole 40
```

Recorramos una colección tipo Set con objetos:

```
let personas = new Set();

personas.add( {nombre: "Penélope", edad: 33 } );
personas.add( {nombre: "Salma", edad: 35 } );
personas.add( {nombre: "Nikole", edad: 40 } );

for( let persona of personas ){
  console.log( persona.nombre, persona.edad );
}

//output
Penélope 33
Salma 35
Nikole 40
```

Recorramos una colección tipo Set con strings:

```
let personas = new Set();

personas.add( "Penélope" );
personas.add( "Salma" );
personas.add( "Nikole" );

for( let persona of personas ){
  console.log( persona);
}

//output
Penélope
Salma
Nikole
```

Vamos a rerrcorrer un Map:

```
let personas = new Map( [["nombre", "Penélope"], ["nombre", "Salma"], ["nombre", "Nikole"]]);

for( let persona of personas ){
  console.log( persona );
}

//output
(2) ["nombre", "Nikole"]
```

A pesar que en el nombre tengo tres elementos solo me imprime el último, ¿por qué pasa esto? El problema esta en la construcción de este mapa ya que no deberían tener nombres de propiedades repetidas y en este caso se repite el nombre "nombre", por eso imprime el último que encuentre.

Vamos a cambiar el ejemplo para que tenga más sentido.

```
let personas = new Map( [["nombre", "Penélope"], ["apellido", "Cruz"], ["edad", 33]]);

for( let persona of personas ){
  console.log( persona );
}

//output
["nombre", "Penélope"]
["apellido", "Cruz"]
["edad", 33]
```

Solo queda una cosa por recordar la variable `persona` que se define dentro del `for` se puede hacer con `let` o con `var`, en el primer caso su ámbito es local al `for` en el segundo es global por lo que yo después del ciclo puedo imprimir su valor.

## Examen #9

:+1:
