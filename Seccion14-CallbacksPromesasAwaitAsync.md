# Callbacks, Promesas, Await y Async

## Callbacks

Los callbacks son el mecanismo tradicional de hacer tareas a destiempo, son funciones comunes y corrientes, no son asíncronos a menos que contengan tareas asíncronas, simplemente demoran la acción de una función hasta que realiza una tarea previa.

Veamos el siguiente ejemplo:
Normalmente un callback tiene parámetros el primero de ellos es el posible error que podamos recibir y el segundo es el elemento con que se trabajo previamente.

```
const getUsuarioById = ( id, callback ) => {
    const usuario ={
    nombre: 'Fernando',
        id
    };
    callback( null, usuario );
};

getUsuarioById(1, (err, user)=> {
    console.log('Usuario de base de datos', user);
});

//output
Usuario de base de datos { nombre: 'Fernando', id: 1 }
```

Vamos a modificar el código para generar un error:

```
const getUsuarioById = ( id, callback ) => {
    const usuario ={
        nombre: 'Fernando',
        id
    };
    if( id === 20 ){
        callback(`El usuario con el id ${id}, no existe`);
    }else{
        callback( null, usuario );
    }
};

getUsuarioById(20, (err, user)=> {
    if( err ){
        return console.log(err);
    }
    console.log('Usuario de base de datos', user);
});

//output
El usuario con el id 20, no existe
```

## Problemas con los Callbacks

Veamos el siguiente ejemplo:

```
const empleados = [{
    id: 1,
    nombre: 'Ronaldo'
},{
    id: 2,
    nombre: 'Salma'
},{
    id: 3,
    nombre: 'Nikole'
}];

const salarios = [{
    id: 1,
    salario: 1000
},{
    id: 2,
    salario: 2000
}];

const getEmpleado = ( id, callback ) => {
    const empleadoDB = empleados.find( empleado => empleado.id === id);

    if ( !empleadoDB ){
        callback(`No existe empleado con el id ${id}`);
    } else {
        callback( null, empleadoDB);
    }
};

//Uso
getEmpleado( 5, ( err, empleado ) => {
    if ( err ) { return console.log(err); }

    console.log('empleado', empleado);
});

//output para 1, 3 y 5
empleado { id: 1, nombre: 'Ronaldo' }
empleado { id: 3, nombre: 'Nikole' }
No existe empleado con el id 5
```

Vamos a añadir una callback adicional para obtener el salario de un empleado:

```
const empleados = [{
    id: 1,
    nombre: 'Ronaldo'
},{
    id: 2,
    nombre: 'Salma'
},{
    id: 3,
    nombre: 'Nikole'
}];

const salarios = [{
    id: 1,
    salario: 1000
},{
    id: 2,
    salario: 2000
}];

const getEmpleado = ( id, callback ) => {
    const empleadoDB = empleados.find( empleado => empleado.id === id);

    if ( !empleadoDB ){
        callback(`No existe empleado con el id ${id}`);
    } else {
        callback( null, empleadoDB);
    }
};

const getSalario = ( empleado, callback ) => {    
    const salarioDB = salarios.find( salario => salario.id === empleado.id);
    if ( !salarioDB ){        
        callback(`No se encontró un salario para el empleado ${empleado.nombre}`);
    } else {        
        callback( null, {
            nombre: empleado.nombre,
            salario: salarioDB.salario,
            id: empleado.id
        });
    }
};

//Uso
getEmpleado( 4, ( err, empleado ) => {
    if ( err ) { return console.log(err); }
    
    getSalario(empleado,  ( err, resp ) => {
        if ( err ) { return console.log(err); }
        console.log(`El salario de ${ resp.nombre } es de ${resp.salario}`);
    });
});

//output para 1,2,3,4
El salario de Ronaldo es de 1000
El salario de Salma es de 2000
No se encontró un salario para el empleado Nikole
No existe empleado con el id 4
```

Como vemos el código trabaja bien, pero el problema con los callback es que a medida que se van necesitando más, estos se van indentando uno detrás de otro y esto hace algo complicado seguir el código, por lo que nacieron las promesas.

## Promesas en lugar de callbacks

Veamos el ejemplo anterior pero hecho con Promesas:

```
const empleados = [{
    id: 1,
    nombre: 'Ronaldo'
},{
    id: 2,
    nombre: 'Salma'
},{
    id: 3,
    nombre: 'Nikole'
}];

const salarios = [{
    id: 1,
    salario: 1000
},{
    id: 2,
    salario: 2000
}];

const getEmpleado = ( id ) => {
    return new Promise ( (resolve, reject) => {
        const empleadoDB = empleados.find( empleado => empleado.id === id);
        if ( !empleadoDB ){
            reject(`No existe empleado con el id ${id}`);
        } else {
            resolve( empleadoDB );
        }
    });
};

getEmpleado(4)
    .then( empleado => {
        console.log('El empleado es: ', empleado );
    })
    .catch( err => console.log(err) );

//output para 1,2,3,4
El empleado es:  { id: 1, nombre: 'Ronaldo' }
El empleado es:  { id: 2, nombre: 'Salma' }
El empleado es:  { id: 3, nombre: 'Nikole' }
No existe empleado con el id 4
```

Vamos a añadir el segundo callback para los salarios:

```
const empleados = [{
    id: 1,
    nombre: 'Ronaldo'
},{
    id: 2,
    nombre: 'Salma'
},{
    id: 3,
    nombre: 'Nikole'
}];

const salarios = [{
    id: 1,
    salario: 1000
},{
    id: 2,
    salario: 2000
}];

const getEmpleado = ( id ) => {
    return new Promise ( (resolve, reject) => {
        const empleadoDB = empleados.find( empleado => empleado.id === id);

        if ( !empleadoDB ){
            reject(`No existe empleado con el id ${id}`);
        } else {
            resolve( empleadoDB );
        }
    });
};

const getSalario = ( empleado ) => {    
    return new Promise ( (resolve, reject) => {
        const salarioDB = salarios.find( salario => salario.id === empleado.id);
        if ( !salarioDB ){        
            reject(`No se encontró un salario para el empleado ${empleado.nombre}`);
        } else {        
            resolve({
                nombre: empleado.nombre,
                salario: salarioDB.salario,
                id: empleado.id
            });
        }
    });
};

getEmpleado(5)
    .then( empleado => {
        getSalario( empleado ).then( resp => {
            console.log(resp);
        });
    })
    .catch( err => console.log(err) );

//output para 1,2,3,4
{ nombre: 'Ronaldo', salario: 1000, id: 1 }
{ nombre: 'Salma', salario: 2000, id: 2 }
(node:5548) UnhandledPromiseRejectionWarning: No se encontró un salario para el empleado Nikole
No existe empleado con el id 4
```

Aquí lo que podemos ver es que dentro de una promesa llamamos otro callback el cual sigue indentando nuestro código, podemos escribir el código para que una promesa llame otra promesa y se vea más claro:

```
getEmpleado(1)
    .then( empleado => {
        return getSalario( empleado );
    })
    .then( resp => {
        console.log( resp );
    })
    .catch( err => console.log(err) );

//output 
{ nombre: 'Ronaldo', salario: 1000, id: 1 }
{ nombre: 'Salma', salario: 2000, id: 2 }
No se encontró un salario para el empleado Nikole //Ya sale bien
No existe empleado con el id 4
```

Lo podríamos simplificar aun más:

```
getEmpleado(5)
    .then( getSalario )
    .then( console.log )
    .catch( err => console.log(err) );

//output
{ nombre: 'Ronaldo', salario: 1000, id: 1 }
{ nombre: 'Salma', salario: 2000, id: 2 }
No se encontró un salario para el empleado Nikole
No existe empleado con el id 4
```

Se llama la función `getEmpleado()` el producto de esta función `getEmpleado()` va a pasar como argumente de la función `getSalario` (no es necesario ponerlo), al mismo tiempo el resultado de getSalario se pasa la siguiente `then` lo que lo imprime, finalmente cualquier error que sucede en las promesas es manejado por el único `catch` que existe.

La solución es muy sencilla y limpia.

## ES7: Async

El `async` se aplica a una función de esta manera la función retornara una `Promise` como resultado, veamos el siguiente ejemplo de una función normal:

```
const getNombre = () => {
    return 'Salma';
}
console.log( getNombre() );

//output
Salma
```

Ahora vamos a aplicar `async` a la función:

```
const getNombre = async () => {
    return 'Salma';
}
console.log( getNombre() );

//output
Promise { 'Salma' }
```

Por lo que si me regresa una `Promise` puedo manejarla como tal:

```
const getNombre = async () => {
    return 'Salma';
}
getNombre().then( nombre => {
    console.log( nombre );
});

//output
Salma
```

Vamos a hacer que nuestra función `async` tenga en el `return` una `Promise`:

```
const getNombre = async () => {
    return new Promise ( (resolve, reject) => {
        setTimeout( () => {
            resolve('Salma');
        }, 3000);
    });
}
getNombre().then( nombre => {
    console.log( nombre );
});

//output
Salma
```

Sigue haciendo lo mismo, veamos el siguiente código:

```
const getNombre = async () => {
    return new Promise ( (resolve, reject) => {
        setTimeout( () => {
            resolve('Salma');
        }, 3000);
    });
}

const saludo = async() => {
    const nombre = getNombre();
    return `Hola ${nombre}`;
}

saludo().then( mensaje => {
    console.log(mensaje);
});

//output
Hola [object Promise]
```

Tenemos nuestra función `getNombre` la cual nos regresa una `Promise`, después tenemos la "función saludo" que es `async` y que manda llamar a `getNombre()` la cual se ejecuta inmediatamente retornando `[object Promise]` cuando ejecutamos la función `saludo()`. Aquí lo que esta pasando es que se ejecuta el código inmediatamente sin esperar que la función `getNombre` finalice (después de 3 seg.), tendríamos que esperar a que nuestra función asíncrona concluyera para continuar ejecutando el siguiente código, para eso tenemos la palabra reservada `await`;

Veamos:

```
const getNombre = async () => {
    return new Promise ( (resolve, reject) => {
        setTimeout( () => {
            resolve('Salma');
        }, 3000);
    });
}

const saludo = async() => {
    const nombre = getNombre();
    return `Hola ${nombre}`;
}

saludo().then( mensaje => {
    console.log(mensaje);
});

//output (Después de 3 seg.)
Hola Salma
```

## Ejercicios Async Await

Vamos a transformar el ejercicio final de las promesas usando async:

```
const empleados = [{
    id: 1,
    nombre: 'Ronaldo'
},{
    id: 2,
    nombre: 'Salma'
},{
    id: 3,
    nombre: 'Nikole'
}];

const salarios = [{
    id: 1,
    salario: 1000
},{
    id: 2,
    salario: 2000
}];

const getEmpleado = async( id ) => {
    const empleadoDB = empleados.find( empleado => empleado.id === id);
    if ( !empleadoDB ){
        throw new Error(`No existe empleado con el id ${id}`);
    } else {
        return empleadoDB;
    }
};

const getSalario = async( empleado ) => {    
    const salarioDB = salarios.find( salario => salario.id === empleado.id);
    if ( !salarioDB ){        
        throw new Error(`No se encontró un salario para el empleado ${empleado.nombre}`);
    } else {        
        return {
            nombre: empleado.nombre,
            salario: salarioDB.salario,
            id: empleado.id
        };
    }   
};

const getInformacion = async(id) => {
    try {
        const empleado = await getEmpleado(id);
        const datosSalario = await getSalario( empleado );
        return `${ empleado.nombre } tiene un salario de ${ datosSalario.salario}`; 
    } catch (error) {
        return error.message;
    }  
}

getInformacion(1).then(console.log);
getInformacion(2).then(console.log);
getInformacion(3).then(console.log);
getInformacion(4).then(console.log);

//Output
Ronaldo tiene un salario de 1000
Salma tiene un salario de 2000
No se encontró un salario para el empleado Nikole
No existe empleado con el id 4
```

La ventaja de tenerlo así es que todo esta concentrado en una misma función es decir para obtener el resultado solo llamamos a `getInformacion()`.
