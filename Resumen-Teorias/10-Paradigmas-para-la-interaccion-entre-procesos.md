# Paradigmas para la interacción entre procesos

1. ¿Cuáles son?

Dados tres esquemas básicos de interacción entre procesos(productor/consumidor, cliente/servidor e interacción entre pares), si se los combina entre ellos se dan lugar a otros paradigmas o modelos de interacción entre procesos, entre ellos:
* Master/worker: implementación distribuída de del modelo Bag of task.
* Algoritmo heartbeat: cuando los procesos deben intercambiar información con mecanismos tipo send/receive.
* Algoritmo pipeline: cuando la información recorre una serie de procesos utilizando alguna forma de send/receive.
* Probes & echoes: cuando la interacción entre los procesos permite recorrer estructuras dinámicas diseminando y juntando información.
* Algoritmo broadcast: permite alcanzar información global en una arquitectura distribuída. Sirve para las decisiones descentralizadas.
* Token passing: cuando la información global es recibida a partir de tokens de control. Tambíen se utiliza para la toma de decisiones.
* Servidores replicados: cuando los servidores manejan, mediante múltiples instancias, recursos compartidos.

## Master/Worker

Para entender el concepto master/worker hace falta entender antes el de bag of tasks, dado que deriva de éste.  
Un bag of tasks usando variables compartidas supone que un conjunto de workers comparten una bolsa con tareas independientes. Los workers sacan una tarea de la bolsa, la ejecutan y, posiblemente, creen nuevas tareas que ponen en la bolsa.  
La mayor ventaja de este enfoque es la escalabilidad y la facilidad para equilibrar la carga de trabajo de los workers. Este es un esquema cliente/servidor. Este esquema se utiliza, entre otras cosas, para la multiplicación de matrices ralas.

## Algoritmo heartbeat

El algortmo heartbeat resulta útil para soluciones iterativas que se quieran paralelizar. Utiliza un esquema *divide & conquer* en el que se distribuye la carga entre los workers, siendo cada uno responsable de actualizar una parte. Los valores dependen de los mantenidos por los worker o sus vecinos inmediatos. Cada paso, debiera significar un progreso hacia la solución. La idea es expandirse enviando información y contraerse para incorporarla. Este algoritmo tiene un excesivo intercambio de mensajes(los procesos cercanos al centro conocen la topología más pronto y no aprenden nada nuevo en los intercambios) y la terminación no siempre puede determinarse localmente. Este esquema se utiliza, entre otras cosas, para autómatas celulares.

## Algoritmo pipeline

Un pipeline es un arreglo lineal de procesos que reciben datos de un canal de entrada y entregan resultados por un canal de salida. Estos procesos pueden estar en procesadores que operan en paralelo, a lazo abierto o bien en forma de pipeline circular. Estos esquemas sirven en procesos iterativos o bien donde la aplicación no se resuelve en una pasada por el pipe. Por otro lado, otro esquema es aquel que es cerrado, es decir, que exista un proceso coordinador que maneje la retroalimentación. Este algoritmo se suele utilizar para las redes de filtro.

## Probe & echo

Probe-echo se basa en el envío de mensajes(probe) de un nodo al sucesor, y la espera posterior del mensaje de respuesta(echo). Los probes se envían a todos los sucesores. Estos algoritmos son interesantes cuando se trata de recorrer redes donde no hay(o no se conoce) un número fijo de nodos activos. Las redes móviles es un ejemplo de este esquema.

## Algoritmo broadcast

La idea del algoritmo es transmitir un mensaje de un procesador a todos los otros.
Los mensajes de tipo broadcast se encolan en los canales en el orden de envío; sin embargo, broadcast no es atómico y los mensajes enviados por distintos procesos podrían ser recibidos por otros procesos en distinto orden. Broadcast es útil para diseminar información o para resolver problemas de sincronización distribuida.

## Token passing

El token passing consiste en un tipo especial de mensaje que puede usarse para otorgar un permiso o recoger información global de la arquitectura distribuida. El token passing es muy importante para controlar la exclusión mutua, por ejemplo.

## Servidores replicados

Un servidor es replicado cuando maneja, mediante múltiples instancias, recursos compartidos. La idea es incrementar la accesibilidad de datos o servicios donde casa servidor descentralizado interactúa con los demás para dar al cliente la sensación de que existe un único servidor. Un ejemplo es la resolución al problema de los filósofos. Tal problema también puede ser resuelto de forma centralizada, descentralizada y distribuída.  
Si es resuelto de manera centralizada, los filósofos se comunican con un proceso mozo que decide el acceso o no a los recursos.    
Si es resuelto de manera descentralizada, cada filósofo ve un único mozo, pero en realidad existen varios que se comunican entre ellos(cada uno con sus dos vecinos) para decidir el recurso asociado a su filósofo.  
Si se decide resolver el problema de manera distribuída, entonces existen 5 mozos, cada uno manejando un tenedor. El filósofo deberá comunicarse con dos mozos(el izquierdo y el derecho) solicitando y devolviendo el recurso. Los mozos no se comunican entre ellos.
Notar que este problema deja de ser de exclusión mutua selectiva en cualquiera de las variantes ya que desaparece la competencia por el subconjunto de procesos: ahora compite con todos por el acceso a los recursos a través del mozo.