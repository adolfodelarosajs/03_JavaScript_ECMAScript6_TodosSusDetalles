# Promesas

## Problemática

JavaScript en sus orígenes siempre fue síncrono, es decir todo se ejecuta en secuencia.

[Explicación Promesas](https://platzi.com/blog/que-es-y-como-funcionan-las-promesas-en-javascript/)

A continuación vamos a simular la característica de Asíncronia de JS.

```
function tareaAsincrona(){
  setTimeout(function(){
    console.log("Proceso Asíncrono terminado");
  }, 1300);
}
tareaAsincrona();
console.log("Código secuencial");

//output
Código secuencial
Proceso Asíncrono terminado
```

Cuando el proceso asíncrono se ejecuta esta en un estado pendiente de resolver, tiene la posibilidad de terminar bien, es decir que la promesa se haya resuelto (resolve) o que se haya rechazado (reject). Y dependiendo de una u otra cosa ejecutara una tarea particular una vez que la tarea asíncrona termina.

```
function tareaAsincrona(){
  setTimeout(function(){
    console.log("Proceso Asíncrono terminado");
    let comoFue = true;
    if(comoFue){
      resolve();
    }else{
      reject();
    }
  }, 1300);
}
tareaAsincrona();
console.log("Código secuencial");

function resolve(){
  console.log("Todo OK!");
}
function reject(){
  console.log("Todo MAL!");
}

//output
Código secuencial
Proceso Asíncrono terminado
Todo OK!
```

## Promesas en ES6

La nueva sintaxis de ES6 nos permite crear promesas usando la clase `Promise()` la cual espera dos argumentos de tipo función, el primer argumento es la función que se ejecutara en caso de que todo allá ido bien, por convención se le llama `resolve` y el segundo se ejecutara si falla conocido como `reject`. Veamos el siguiente ejemplo:

```
function tareaAsincrona(){
  let promesa = new Promise ( (resolve, reject) => {
    setTimeout(function(){
      console.log("Proceso Asíncrono terminado");
      let comoFue = true;
      if(comoFue){
        resolve();
      }else{
        reject();
      }
    }, 1300);
  });
  return promesa;
}

tareaAsincrona().then( function(){
  console.log("Todo OK!");
}, 
function(){
  console.error("Todo MAL!");
});
console.log("Código secuencial");

//output
Código secuencial
Proceso Asíncrono terminado
Todo OK!
```

La diferencia principal con el primer ejemplo es que aquí la función nos regresa como resultado una `promesa`, la cual cuenta con el método `then()` dentro del cual le pasamos de manera anónima las dos funciones que corresponden al `resolve` y `reject` (puede ser opcional). Una promesa podría regresar otra promesa.

