# Clasificacion de arquitecturas paralelas

1. ¿Qué enfoques pueden tenerse en cuenta para clasificar las arquitecturas?

Hay cuatro:

* Por la organización del espacio de direcciones.
* Por la granularidad.
* Por el mecanismo de control.
* Por la red de interconexión.

Según el ***mecanismo de control***, nos basaremos en la manera en que las instrucciones son ejecutadas sobre los datos. ESto nos deja con cuatro opciones posibles:

* *Single instruction, single data*(**SISD**)
* *Single instruction, multiple data*(**SIMD**)
* *Multiple instruction, single data*(**MISD**)
* *Multiple instruction, multiple data*(**MIMD**)

Si se utiliza una arquitectura ***SISD***, las instrucciones son ejecutadas en secuencia, una por ciclo de instrucción. La memoria afectada es usada solo por ésta instrucción. La *CPU* ejecuta instrucciones(decodificadas por la *UC*) sobre los datos. La memoria recibe y almacena datos en las escrituras y brinda datos en las lecturas. La mayoría de los uni procesadores utiliza ésta arquitectura. Ejecución determinística.  
Por otra parte, si la arquitectura es ***SIMD***, tenemos un conjunto de procesadores idénticos, con sus memorias respectivas, que ejecutan la misma instrucción sobre distintos datos. Por lo general, los procesadores suelen ser muy simples. El *host* hace *broadcast* de la instrucción. La ejecución es sincrónica y determinística. Pueden deshabilitarse y habilitarse selectivamente procesadores para que ejecuten o no instrucciones. Esta arquitectura es muy útil para aplicaciones con alto grado de regularidad(procesamiento de imágenes, por ejemplo).  
Si, en cambio, se sigue una arquitectura ***MISD***, entonces los procesadores ejecutan un flujo de instrucciones distinto pero comparten datos comunes. Opera en forma sincrónica, en forma de cerradura(*lockstep*). No son máquinas de propósito general, son hipotéticas.  
Finalmente, si se utiliza una arquitectura ***MIMD*** cada procesador tiene su propio flujo de instrucciones y de datos, es decir, cada uno ejecuta su propio programa. La memoria, en este caso, puede ser compartida o distribuída. Dentro de MIMD podemos distinguir dos casos:
    + *Multiple program, multiple data*(***MPMD***): cada procesador ejecuta su propio programa.
    + *Single program, multiple data*(***SPMD***): hay unúnico programa fuente y cada procesador ejecuta su copia independientemente.

Según las ***redes de interconexión***, que conectan las máquinas construídas a partir de diversos procesadores, que bien pueden ser tanto de memoria compartida como las de pasaje de mensajes, hay dos tipos:
* Las redes estáticas: aquellas que constande links punto-a-punto. Suelen usarse para máquinas de pasaje de mensajes.
* Las redes dinámicas: construídas usando *switches* y enlaces de comunicación. Suelen usarse para máquinas de memoria compartida.