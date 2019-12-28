# Profundizando en las clases

## Métodos estáticos y métodos computados

### Métodos estáticos

Los métodos estáticos permiten ser ejecutados sin instanciar la clase. Para que un método sea estáticos debe ir precedido de la palabra `static`.

```
class Persona{
  constructor( nombre ){
    this.nombre = nombre;
  }
  decirNombre(){
    console.log( this.nombre );
  }
  static crear( nombre ){
    return new Persona(nombre);
  }
}

let yo = Persona.crear( "Adolfo" );
console.log( yo );

//output
Persona {nombre: "Adolfo"}
```

### Métodos computados

Los métodos computados son aquellos que su nombre se define en una variable y se invocan con la notación de punto:

```
let nombreMetodo = "gritarNombre";

class Persona{
  constructor( nombre ){
    this.nombre = nombre;
  }
  decirNombre(){
    console.log( this.nombre );
  }
  [nombreMetodo](){
    console.log( this.nombre.toUpperCase() ); 
  }
  static crear( nombre ){
    return new Persona(nombre);
  }
}

//output
let yo = Persona.crear("Adolfo");
console.log( yo.decirNombre() );
console.log( yo.gritarNombre() );
```

## Herencia de las Clases

La herencia es transferir propiedades y métodos de una clase Padre a una clase Hija. Para decir que una clase extiende do otra se usa la palabra `extends`, con simplemente eso ya hereda las propiedades y métodos, veamos el ejemplo:

```
class Cuadrilatero{
  constructor(alto, largo){
    this.alto = alto
    this.largo = largo;
  }
  getArea(){
    return this.alto * this.largo;
  }
}

class Cuadrado extends Cuadrilatero{
  constructor(lado){
    super( lado, lado);
  }
}

class Rectangulo extends Cuadrilatero{
  constructor(alto, ancho){
    super( alto, ancho);
  }
}

let cuadrado = new Cuadrado( 3 );
let rectangulo = new Rectangulo( 3, 4);

console.log(cuadrado.getArea());
console.log(rectangulo.getArea());

console.log( cuadrado instanceof Cuadrado);
console.log( cuadrado instanceof Rectangulo);
console.log( cuadrado instanceof Cuadrilatero);

//output
9
12
true
false
true
```

Las clases hijas cuentan con su propio constructor, pero como la funcionalidad será la misma que la del padre lo invocamos usando la palabra `super`. Por otro lado tanto `Cuadrado` como `Rectangulo` heredan el método `getArea()` por eso sus instancias pueden utilizar este método aun que en sus clases no se definió pero si se heredo.

## Sobrescribiendo funciones del padre

Las clases hijas heredan las propiedades y métodos de la clase padre, pero si incluimos un método en la clase hija que ya existía en la clase padre lo estaremos sobrescribiendo.

Continuemos con el ejemplo de la sección pasada:

```
class Cuadrilatero{
  constructor(alto, largo){
    this.alto = alto
    this.largo = largo;
  }
  getArea(){
    return this.alto * this.largo;
  }
}

class Cuadrado extends Cuadrilatero{
  constructor(lado){
    super( lado, lado);
  }
}

class Rectangulo extends Cuadrilatero{
  constructor(alto, ancho){
    super( alto, ancho);
  }
}

class Rombo extends Cuadrilatero{
  constructor(d1, d2){
  	super(d1, d2)
  }
  getArea(){
    return (this.alto*this.largo)/2;
  } 
}

class Romboide extends Cuadrilatero{
  constructor(d1, d2){
  	super(d1, d2)
  }
  getArea(){
    return super.getArea();
  } 
}

let cuadrado = new Cuadrado( 3 );
let rectangulo = new Rectangulo( 3, 4);
let rombo = new Rombo( 3, 4);
let romboide = new Romboide( 5, 4);

console.log(cuadrado.getArea());
console.log(rectangulo.getArea());
console.log(rombo.getArea());
console.log(romboide.getArea());

//output
9
12
6
20
```

En el caso del Rombo hemos sobrescrito el método `getArea()` y en el caso del Romboide como no encontramos la formula estamos usando la del Cuadrilátero.

## Examen #11 

:+1:
