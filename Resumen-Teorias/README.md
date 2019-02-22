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

- 3. ¿Por qué semáforos es mejor que busy waiting?

Porque hace que no se consuma tiempo de procesamiento hasta que no se tenga posibilidad de ejecución. Busy waiting es ineficiente cuando los procesos son implementados por multiprogramación ya que mantendrá ocupado un procesador realizando spinning cuando este podría ser usado por otro proceso. Otro tema que también resuelve es la complejidad que conlleva diseñar o prbar un algoritmo de tipo busy waiting: la mayoría de los protocolos son complejos y no hay una clara separación entre las variables usadas para sincronización y aquellas para comparar resultados. //ESTA BIEN??

- Rendezvous propone un encuentro e intercambio entre dos procesos(algo así como una cita). La idea es que un proceso que espere su invocación(espere a que un proceso venga al encuentro). Cuando otro proceso lo invoque(acuda al encuentro), es ahí cuando el proceso se bloquea y garantiza la simetría entre dos procesos. //ESTA BIEN??

problema de los baños
criba de erastotenes
9)Sea el problema en el cual N procesos poseen inicialmente cada uno un valor V, y el objetivo es que todos conozcan cuál es el máximo y cuál es el mínimo de todos los valores. a) Plantee conceptualmente posibles soluciones con las siguientes arquitecturas de red: centralizada, simétrica (o totalmente conectada) y anillo circular (NO IMPLEMENTE). b) Analice las soluciones desde el punto de vista del número de mensajes y la perfomance global del sistema.
10) Defina el concepto de granularidad. ¿Qué relación existe entre la granularidad de programas y de procesadores?

11) Dado el siguiente programa concurrente con memoria compartida, y suponiendo que todas las variables están inicializadas en 0 al empezar el programa y las instrucciones NO son atómicas. Para cada una de las opciónes indique verdadero o falso. En caso de ser verdadero indique el camino de ejecución para llegar a ese valor, y en caso de ser falso justifique claramente su respuesta. 
P1:: 
if(x = 0) then    
    y:= 4*x + 2;   
    x:= y + 2 + x; 
P2:: 
if (x > 0 ) then    
    x:= x + 1;
P3:: 
x:= x*8 + x*2 + 1;

a) El valor de x al terminar el programa es 9.
b) El valor de x al terminar el programa es 6.
c) El valor de x al terminar el programa es 11.
d) Y siempre termina con alguno de los siguientes valores: 10 o 6 o 2 o 0.

 Describa brevemente los mecanismos de comunicación y de sincronización provistos por MPI, Linda y Java.

16) Suponga que quiere ordenar n números enteros utilizando mensajes con el siguiente algoritmos (odd/even exchange sort). Hay n procesos P[1:n], con n par. Cada proceso ejecuta una serie de rondas. En las rondas “impares”, los procesos con número impar  P[impar] intercambian valores con P[impar+1] si los números están desordenados. En las rondas “pares”, los procesos con número par P[par] intercambian valores con P[par+1] si los números están desordenados (P[1] y P[n] no hacen nada en las rondas “pares”). a) Determine cuántas rondas deben ejecutarse en el peor caso para ordenar n números. N
b) ¿Considere que para este caso es más adecuado utilizar mensajes sincrónicos o asincrónicos? Justifique. Mejor PMS porque asincrónico es innecesario mantener las colas de PMA c) Escriba un algoritmo paralelo para odenar un arreglo a[1:n] en forma ascendente. ¿Cuántos mensajes se utilizan?
d) ¿Cómo modificaría el algoritmo del punto c) para que termine tan rápido como el arreglo esté ordenado (por ej, podría estar ordenado inicialmente)? ¿Esto agrega overhead de mensajes? ¿Cuanto?
e) Modifique la respuesta dada en c) para usar k procesos (asuma que n es múltiplo de k).


 Explica los problemas que pueden surgir en el anidamiento de llamadas a monitores
25.	¿Qué elementos de la forma general de Rendezvous no se encuentran en el lenguaje ADA?

* La expresion de scheduler.
* En Ada no se pueden ver los parametros hasta no haber aceptado

34.	Defina el concepto de “continuidad conversacional” entre procesos.

26.	Describa los mecanismos de comunicación y sincronización provistos por MPI, Ada, Java y Linda

51.	Analice las arquitecturas multiprocesador, vector processor, array processor, data flow y transputer. Relaciónelos con los tipos de problemas que son más adecuados para resolver en cada una de ellas.

52.	¿Qué es una lógica de programación y cuáles son sus componentes? ¿Qué es un invariante?