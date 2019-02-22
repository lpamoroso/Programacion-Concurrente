# Variables compartidas

1. ¿Qué son las variables compartidas?

Una variable compartida es aquella **variable** que es **compartida** entre varios procesos.  
Esta implementación tiene un problema muy importante que se da a causa de la naturaleza promiscua de estas variables. El hecho de que más de un proceso pueda **escribirlas** hace que se generen inconsistencias en el valor de estas. Este problema es el que se conoce como ***Problema de la sección crítica***.  
Para evitar caer en este problema, existen cuatro propiedades a cumplir:
1. Exclusión mutua: a lo sumo un proceso dentro de una sección crítica. Es una propiedad de seguridad.
2. Ausencia de *deadlock*: si dos o más procesos tratan de entrar a su sección crítica(y ésta está libre), al menos uno tendrá éxito. Es una propiedad de seguridad.
3. Ausencia de demora innecesaria: si un proceso trata de entrar a su sección crítica y los otros están en sus secciones no críticas o terminaron de ejecutarse, el primero no debe estar impedido de entrar a su sección crítica. Es una propiedad de seguridad.
4. Eventual entrada: un proceso que intenta entrar a su sección crítica eventualmente lo hará. Es una propiedad de vida.

Las tres primeras son de seguridad; la cuarta, de vida.

2. ¿Qué implementaciones de variables compartidas existen?

Hay cinco:
1. *Busy-waiting*.
2. *Spin locks*.
3. Algoritmo *tie-breaker*.
4. Algoritmo *bakery*.
5. Sincronización *barrier*.

En la implementación por ***busy-waiting***, un proceso chequea repetidamente una condición hasta que sea verdadera. Si bien puede implementarse con cualquier procesador y es aceptable siempre y cuando cada proceso se ejecute en su procesador(para que no interfiera con otros procesos), es ineficiente en multiprogramación, es decir, cuando varios procesos compiten por el procesador y la ejecución es intercalada.
Si, en cambio, se utiliza una solución de tipo ***spin locks*** entonces los procesos se quedarán iterando mientras comprueban repetidamente(*spin*) si el bloqueo(*lock*) está disponible. Si el *scheduling* es fuertemente *fair*, cumple las cuatro propiedades. Lo cierto es que una política débilmente fair es aceptable dado que rara vez todos los procesos están simultánemente tratando de acceder a su sección crítica. Por el contrario, su *performance* en multiprocesadores en los que varios procesos compiten por el acceso. Además, dado que la variable booleana que se utiliza como bloqueo es compartida, su acceso termina siendo costoso. Otra desventaja es que podía producirse un alto *overhead* por cache inválida.  
Uno de los problemas(el más significativo) de *spin-locks* era que no controlaba el orden de los procesos demorados, por lo que era posible que algún proceso no entre nunca si el *scheduling* no era lo suficientemente *fair*: esto es lo que soluciona el ***algoritmo tie-breaker***. La idea es usar una variable por cada proceso para indicar que el proceso comenzó a ejecutar su protocolo de entrada a la sección crítica, y una variable adicional para romper empates(*tie-breaker*), indicando qué proceso fue el último en comenzar dicha entrada. Esta última variable es compartida y de acceso protegido. Este algoritmo resuelve la problemática de *spin-locks*, ya que solo requiere *scheduling* débilmente *fair* y no utiliza instrucciones especiales. Sin embargo, el problema está en que es mucho más complejo que el anterior.  
Si el algoritmo *tie-breaker* es llevado a un escenario con *n*-procesos, siendo que ya de por si es complejo, se vuelve aun más y, además, se vuelve costoso en tiempo. Bajo estas condiciones, aparece el ***algoritmo ticket***. La idea, esta vez, es repartir números a cada proceso y que cada uno espere su turno. Los procesos toman un número mayor que el de cualquier otro que espera ser atendido; luego esperan hasta que todos los procesos con número más chico hayan sido atendidos. *A priori*, este algoritmo tiene un problema potencial que es que los valores de **próximo** y **turno** son ilimitados: esto se soluciona reseteandolos a un valor chico en cada corrida(1, por ejemplo). El algoritmo *ticket* cumple todas las propiedades:
1. Exclusión mutua: **número** es leído e incrementado en una acción atómica y **próximo** es incrementado en una acción atómica: siempre a lo sumo hay un proceso en la sección crítica.
2. Ausencia de *deadlock*: los valores de **turno** son únicos.
3. Ausencia de demora innecesaria: los valores de **turno** son únicos.
4. Eventual entrada: si el *scheduling* es débilmente *fair*, queda asegurada.

El algoritmo *ticket* es muy completo, pero tiene un detalle: si no existe una operación que venga implementada en la mayoría de los procesadores(como es el *fetch & add*) que se encargue de los turnos, esto debe simularse con otra sección crítica y la solución puede no ser *fair*. Para esto está el ***algoritmo bakery***. Esta vez, la idea es que cada proceso que trata de ingresar recorre los números de los demás y se auto-asigna uno mayor. Luego espera a que su número sea el menor de los que esperan. La característica importante es que los procesos se chequean entre ellos y no contra un global. Si bien este algoritmo es mucho más complejo, lo cierto es que es *fair* y no requiere instrucciones especiales. Tampoco hace uso de un contador global **próximo** como en el algoritmo *ticket*. Sin embargo, esta solución no es implementable directamente por dos razones:
1. La asignación de turno exige calcular el máximo de *n* valores.
2. Para calcular el máximo(el turno que le corresponde a un proceso *n*) es necesario referenciar la variable compartida(hay que calcular el máximo de entre todos los turnos), por lo que en una misma sentencia, que debe ejecutarse de forma atómica, se referencia dos veces la misma variable compartida.

Si estamos usando un algoritmo iterativo(como los ya descritos antes) es porque la idea, de una manera u otra, es que en cada iteración todos los procesos realicen el mismo trabajo sobre diferentes datos y requieran que haya finalizado el paso previo. Esto a la larga termina siendo ineficiente, ya que produce *n* procesos en cada iteración. La ***sincronización barrier*** mejora ese aspecto ya que propone que el punto de demora al final de cada iteración sea una barrera a la que deban llegar todos antes de permitirles pasar. Existen varias implementaciones de este algoritmo:
1. Contador compartido.
2. Flags y coordinadores.
3. Árboles.
4. Barreras simétricas.

Si utilizamos la implementación de ***contador compartido***, debemos tener en cuenta tres aspectos importantes:
1. Cada proceso incrementa una variable **cantidad** al llegar. Esto es implementable mediante la instrucción por hardware *fetch & add*.
2. Solo cuando *cantidad = n* los procesos pueden pasar.
3. **cantidad** debe ser reseteado a 0 con cada corrida. Esta implementación puede acarrear contención de memoria e incoherencias en la cache.

Si, en cambio, se opta por una implementación **flags y coordinadores**, entonces, en lugar de un contador compartido, cada proceso tendrá su **flag** que marcará en el momento que arribe. Para continuar, deberá esperar al resto de *workers*. Cuando todos hayan concluído, el proceso **coordinador** será quien resetee los flags y proceda a desbloquear cada barrera. Esta implementación, de no existir *fetch & add*, reintroduce contención de memoria y es ineficiente ya que cae en la misma problemática que el algoritmo *ticket*.  
Por otro lado, otra implementación posible es la de **árboles**, sin embargo, los problemas en este caso son dos:
1. Requerir un proceso(y, por ende, un procesador) extra.
2. El tiempo de ejecución del coordinador es proporcional a *n*.

Para solucionar ese *issue* podríase combinar las acciones de *workers* y coordinador haciendo que cada *worker* también sea coordinador(*workers* en forma de árbol: las señales de arribo van hacia arriba en el árbol; y las de continuar, hacia abajo).  
Con la implementación de sincronización *barrier* utilizando árboles, los procesos juegan diferentes roles. Otra implementación sería construir una **barrera simétrica** para *n* procesos, a partir de pares de barreras simples para dos procesos.

1. Cada proceso incrementa una variable **cantidad** al llegar. Esto es implementable mediante la instrucción por hardware *fetch & add*. Si tal instrucción no existiera, caeríamos en la misma situación del algoritmo *ticket*: tener que implementar el *fetch & add* con una sección crítica lo cual reintroduciría las contenciones de memoria y resultaría siendo ineficiente.
2. Solo cuando *cantidad = n* los procesos pueden pasar.
3. Otra forma en que puede implementarse la sincronización *barrier* es mediante **árboles**, los problemas en este caso son dos:
    1. Requerir un proceso(y, por ende, un procesador) extra.
    2. El tiempo de ejecución del coordinador es proporcional a *n*.

Para solucionar ese issue podríase combinar las acciones de *workers* y coordinador haciendo que cada *worker* también sea coordinador(*workers* en forma de árbol: las señales de arribo van hacia arriba en el árbol; y las de continuar, hacia abajo).

Uno de los problemas que tiene este algoritmo es el mismo que tiene el algoritmo *ticket*, ya que el valor **cantidad** debe ser reseteado a 0 con cada iteración. También puede haber inconsistencias en la cache y problemas en la contención de memoria.

3. ¿En qué se relaciona el *await* con las instrucciones *test & set* o *test & set*?

La sentencia await se utiliza para especificar acciones atómicas arbitrarias de grano grueso. Esto la hace conveniente tanto para expresar sincronización por condición como exclusión mutua. Su expresividad hace que sea muy costosa de implementar; sin embargo, puede representarse eficientemente con *test & set* o *fetch & add* que están presentes en casi todos los procesadores.