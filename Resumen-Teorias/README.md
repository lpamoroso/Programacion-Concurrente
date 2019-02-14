# Lo que tengo que preguntar:

- Ventajas de la concurrencia relacionadas al hardware.

Aumentar el rendimiento aprovechando todos los procesadores disponibles porque se aumento de numero de procesadores y no la velocidad(paralelismo).

- *A priori*, este algoritmo tiene un problema potencial que es que los valores de **próximo** y **turno** son ilimitados: esto se soluciona reseteandolos a un valor chico en cada corrida(1, por ejemplo). //NO ENTIENDO POR QUÉ DEBERÍA SER ASÍ. ESTO ESTA PLANTEADO DESDE UN PUNTO DE VISTA EN EL QUE SE DAN SUCESIVAS CORRIDAS EN LAS QUE ES NECESARIO REINICIAR??

Los numeros no son infinitos. En algun momento llega a su limite y se rompe todo.

- Algoritmo Iterativo: computan sucesivas mejores aproximaciones a una respuesta, y terminan al encontrarla o al converger//NO ENTIENDO MUY BIEN ESTO. POR QUÉ UN ALGORITMO ITERATIVO RESULTA SER INEFICIENTE??

No es que sea ineficiente, sino que es inexacto.

- NECESITO ENTENDER POR QUÉ LA SINCRONIZACIÓN BARRIER MEJORA LOS ALGORITMOS ITERATIVOS. LA SINCRONIZACIÓN BARRIER ES MEJOR?? LA FORMA EN QUE SE PROPONEN LOS ALGORITMOS(DESDE EL BUSY WAITING HASTA EL BAKERY) SON DE MEJOR HASTA PEOR?? PODRIA USAR UNA APROXIMACIÓN HISTÓRICA?(SI SURGIERON DE ESA FORMA)?

La sincronizacion barrier es parte del algoritmo iterativo.

- SINCRONIZACION BARRIER ES HACER LOS MISMO QUE ALGORITMOS ITERATIVOS O ES HACERLO MEJOR??

La sincronizacion barrier es parte del algoritmo iterativo.

- SI EN SINCRONIZACION BARRIER NO EXISTE FETCH & ADD, SE CAE EN LA MISMA SITUACION QUE EN EL ALGORITMO TICKET POR LO CUAL ES NECESARIO IMPLEMENTAR ESE FETCH & ADD CON UNA SECCIÓN CRÍTICA.


- 1. Requerir un proceso(y, por ende, un procesador) extra. //CONSULTAR SI ESTA BIEN
- A LO ULTIMO HABLA DE DEFECTOS DE BUSY WAITING... EL BUSY WAITING ES INHERENTE A TODOS LOS ALGORITMOS??

Todos esos algoritmos utilizan busy waiting.

- Semaforo binario dividido es un especie de butterfly barrier?? //NECESITO AYUDA PARA EXPLICARLO BIEN.

No.

- No es un TAD dado que es compartido por procesos que ejecutan concurrentemente //ESTA BIEN ACERCA DE LOS MONITORES??

Esta bien.

- PREGUNTAR SI ENTRA MPI(BIBLIOTECAS PARA MANEJO DE PM) Y CSP(LENGUAJE PARA PMS).

Podria entrar como funciona la comunicacion guardada. De MPI no suele haber nada.

- NO ENTIENDO ESTO: Por sí mismo, RPC es solo un mecanismo de comunicación.

Lo unico que permite es que se comunique es la comunicacion. Las variables entre los modulos pueden pisarse.

- PREGUNTAR SI TENGO QUE RESUMIR ADA.

Si esta biuen algun concepto.

- Un programa paralelo puede escribirse usando VC... QUE ES VC??

Variables compartidas.

- Pedir explicación y ejemplos de speedup y eficiencia

- El grado de concurrencia(o paralelismo) es el número máximo de tareas que pueden ejecutarse simultáneamente en algun momento del algoritmo paralelo. Para un tamaño de problema dado, el algoritmo paralelo no puede usar tantos procesadores como su paralelismo lo indique

----------------------------------------------------------------------------------

- ## Algoritmo broadcast

La idea del algoritmo es transmitir un mensaje de un procesador a todos los otros.
Los mensajes de tipo broadcast se encolan en los canales en el orden de envío; sin embargo, broadcast no es atómico y los mensajes enviados por distintos procesos podrían ser recibidos por otros procesos en distinto orden. Broadcast es útil para diseminar información o para resolver problemas de sincronización distribuida. //ESTA BIEN???

- ¿qué tipo de comunicación por mensajes es más conveniente y qué arquitectura de hardware se ajusta mejor?

- 6. ¿En qué consiste la “ley de Amadhi”?

La ley de Amadhi dice que para un problema dado existe un máximo speedup alcanzable independiente del número de procesadores. Esto significa que es el algoritmo el que decide la mejora de velocidad dependiendo de la cantidad de código no paralelizable y no del número de procesadores,  llegando finalmente a un momento que no se puede paralelizar más el algoritmo. //PREGUNTAR SI ESTO ES ASÍ

- * Computación de grillas: dividen una región espacial en un conjunto de puntos. El procesamiento de imágenes es un ejemplo de esto.

- LEER EL PROBLEMA DE LOS BAÑOS SEMÁFOROS(pregunta 11)

- ## Servidores replicados

Un servidor es replicado cuando maneja, mediante múltiples instancias, recursos compartidos. La idea es incrementar la accesibilidad de datos o servicios donde casa servidor descentralizado interactúa con los demás para dar al cliente la sensación de que existe un único servidor. Un ejemplo es la resolución al problema de los filósofos. Tal problema también puede ser resuelto de forma centralizada, descentralizada y distribuída.  
Si es resuelto de manera centralizada, los filósofos se comunican con un proceso mozo que decide el acceso o no a los recursos.    
Si es resuelto de manera descentralizada, cada filósofo ve un único mozo, pero en realidad existen varios que se comunican entre ellos(cada uno con sus dos vecinos) para decidir el recurso asociado a su filósofo.  
Si se decide resolver el problema de manera distribuída, entonces existen 5 mozos, cada uno manejando un tenedor. El filósofo deberá comunicarse con dos mozos(el izquierdo y el derecho) solicitando y devolviendo el recurso. Los mozos no se comunican entre ellos.
Notar que este problema deja de ser de exclusión mutua selectiva en cualquiera de las variantes ya que desaparece la competencia por el subconjunto de procesos: ahora compite con todos por el acceso a los recursos a través del mozo.// ESTA BIEN??

- REVISAR 05. ASEGURARSE QUE TODO SEA CORRECTO.