# Paradigmas de resolución de programas concurrentes

1. ¿Qué paradigmas existen?

Si bien el número de aplicaciones de la programación concurrente es muy grande, los patrones se reducen a:
* Paralelismo iterativo.
* Paralelismo recursivo.
* Productores y consumidores.
* Cliente y servidores
* Pares que interactúan.

En el ***paralelismo iterativo***, un programa consta de un conjunto de procesos(posiblemente idénticos) cada uno de los cuales tiene uno o más *loops*. Cada proceso es un programa idéntico. Este paradigma es utilizado cuando se requiere cooperación entre los procesos(que bien pueden trabajar el uno independiente del otro) y comunicación y sincronización entre ellos para resolver un único problema. Un ejemplo es la multiplicación de matrices.  
En el ***paralelismo recursivo***, el problema general(programa) puede descomponerse en procesos recursivos que trabajan sobre partes del conjunto total de datos(*divide & conquer*). Un ejemplo es el *sorting by merging*.  
El esquema ***productor-consumidor***, por su parte, presenta un problema en el que muchos procesos se comunican a fin de formar pipes a través de los cuales fluye la información. Cada proceso en el pipe es un filtro que consume la salida de su proceso predecesor y produce una salida para el proceso siguiente. Un ejemplo es son las secuencias de filtros sobre imágenes.  
Si hablamos de un esquema ***cliente-servidor***, en cambio, nos referimos a aquellos esquemas que muestran un ***servidor*** que espera pedidos de servicios de multiples ***clientes***. Naturalmente, unos y otros pueden ejecutarse en procesadores diferentes. La comunicación es bidireccional. Suele atenderse de a un cliente a la vez, varios si contamos con *multithreading*. El ejemplo por excelencia son los sitios de *internet*.  
Finalmente, en los esquemas de ***pares que interactúan***, los procesos(que forman parte de un programam distribuído)resuelven partes del problema(normalmente mediante código idéntico) e intercambian mensajes para avanzar en la tarea y completar el objetivo. El esquema permite mayor grado de asincronismo que cliente-servidor. 
