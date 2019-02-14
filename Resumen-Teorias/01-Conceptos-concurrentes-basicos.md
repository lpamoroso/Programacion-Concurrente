# Concurrencia

1. ¿Qué es la concurrencia?

Es la ***capacidad*** de ***ejecutar múltiples actividades en paralelo o simultáneamente***. Por esto y por la ***necesidad de sistemas de cómputo cada vez más poderosos y flexibles***, es que es un ***factor relevante*** para el ***diseño de hardware, sistemas operativos, multiprocesadores, computación distribuida, etc***.

2. ¿Donde encontramos la concurrencia?

La concurrencia es un factor que se encuentra masivamente en la naturaleza. Desde varios navegadores accediendo a la misma página, el acceso a disco mientras otras aplicaciones siguen funcionando hasta un gran número de células evolucionando simultáneamente y realizando(independientemente) sus procesos, la concurrencia está ahí presente, en todo momento.

3. ¿Por qué es necesaria la programación concurrente?

Hay tres razones:
* **Estructura más natural**: en otras palabras, ***el mundo real no es secuencial***.
* **Mejora en la respuesta**: esto se da porque, ante una operación de entrada o salida, **NO** se bloquea la aplicación completa y porque ésta usa el hardware más adecuadamente(***ejecución paralela***).
* **Sistemas distribuídos**: permite que una aplicación se desarrolle en ***varias máquinas***. Ej: sistema Peer-2-Peer.

4. ¿Cuáles son los objetivos de los sistemas concurrentes?

Hay dos razones:
* ***Ajustar el modelo de arquitectura de hardware*** al problema del mundo real a resolver.
* ***Incrementar la performance***, mejorando los ***tiempos de respuesta*** de los sistemas de cómputo, a través de un enfoque diferente de la arquitectura física y lógica de las soluciones.

5. ¿Cuáles son las ventajas de la concurrencia?

* La ***velocidad de ejecución*** que se puede alcanzar.
* ***Mejor utilización de la CPU*** de cada procesador.
* Explotación de la ***concurrencia inherente*** a la mayoría de ***problemas reales***, con lo que ello implica.

//COMPLETAR COSO HARDWARE

6. ¿Cómo se podría comportar un proceso?

Supongamos que debemos ensamblar un objeto.  
Si ***hay un único flujo de control***, entonces estamos ante un ***proceso secuencial*** que ejecuta ***una instrucción a la vez***. Estamos ante una situación que ***nos fuerza*** a establecer un ***estricto orden temporal***. Además, sabemos que si ejecutamos una y otra vez el programa, los resultados serán siempre los mismos. Esto es lo que llamamos **determinismo**.  
Cuando hay ***muchos flujos de control***, entonces los procesos podrían ser ***independientes*** los unos de los otros, podrían ***competir*** entre ellos o podrían ***cooperar*** en torno a un fin específico. En cualquiera de los casos, la ejecución es **no-determinística**, es decir, pueden dar distintos resultados al ejecutarse sobre los mismos datos de entrada.
Si los procesos fueran ***independientes*** entre ellos, entonces no habría dependencias y podrían ejecutarse ***simultáneamente*** sin problemas. Esto es lo que denominamos ***paralelismo***. Para llevarla a cabo, en principio, requerimos que el paralelismo pueda llevarse a cabo(la independencia que había descrito más arriba) y ésto no siempre es fácil. Sin embargo, las tareas podrán llevarse a cabo en menos tiempo y el esfuerzo individual se reducirá. Por otro lado encontramos varias dificultades:
* Distribución en la carga de trabajo: el problema aparece cuando la carga de trabajo es desigual(¿A qué proceso le asigno más carga? ¿En qué me baso para ello?).
* Necesidad de compartir recursos evitando conflictos.
* Necesidad de esperarse.
* Necesidad de comunicarse.
* //COMPLETAR

El asunto está cuando ***compiten*** entre ellos por algún ***recurso*** en particular o cuando ***cooperan*** en torno a la finalización de una tarea en común. En cualquiera de los dos casos es requerido algún mecanismo de sincronización.
Si los procesos ***cooperan*** o ***compiten*** entre ellos, una máquina dedica una parte del tiempo a cada componente del objeto. Esto es lo que llamamos ***concurrencia sin paralelismo***. En este caso, se utiliza el concepto de ***multiprogramación en un procesador***, el cual consiste en compartir el tiempo de CPU entre los procesos utilizando un *scheduler*. Aparte de las dificultades del paralelismo, hay otras más:
* Menor speedup: en otras palabras, va a llevar más tiempo.
* Necesidad de recuperar el estado de cada proceso al retomarlo.

7. ¿Qué es un programa concurrente?

Es aquel software que permite que dos o más programas secuenciales se ejecuten concurrentemente en el tiempo. **NO** está restringido a una ***arquitectura particular*** ni a un determinado ***número de procesadores***. Tiene dos características importantes:
* Interacción entre los procesos.
* No determinismo, lo que dificulta el *debug* y la interpretación.

8. Si un programa es concurrente ¿Es también paralelo?

No, pero lo que sí ocurre es que ***si un programa es paralelo entonces tambíen es concurrente***, dado que ***los programas paralelos representan un subconjunto de los programas concurrentes***. En este punto es necesario diferenciar dos tipos de concurrencia:
* **Intercalada**: la ejecución es ***intercalada*** en un único procesador(pseudo-paralelismo). El procesamiento en simultáneo es **lógico**.
* **Simultánea**: la ejecución se da en ***diferentes procesadores*** la una independiente de la otra. El procesamiento en simultáneo es **físico**. ***Requiere necesariamente más de un procesador***.

9. ¿Un proceso es lo mismo que un hilo?

Si y no. La diferencia radica en que ***el proceso tiene su propio espacio de direcciones y recursos***; un ***thread***(hilo) es un tipo de proceso "liviano" que tiene *program counter* y pila de ejecución, ***pero no maneja el "contexto pesado"***. Además, los *threads* ***comparten el mismo espacio de direcciones y recursos***, por lo que es menester ***mecanismos para evitar interferencias***.

10. ¿Cómo se mide el cómputo de un programa concurrente?

En términos generales, ***mientras mayor sea el grado de concurrencia, menor es el tiempo de ejecución***. Sin embargo, existen dos métricas que contemplan el incremento de performance:
* ***Speedup***: se calcula como mejor tiempo secuencial dividido tiempo paralelo.
* **Eficiencia**: se calcula como *speedup* dividido tiempo paralelo.
* //QUIERO UNA MEJOR DEFINICION

11. ¿Cómo se comunican los procesos concurrentes?

Definimos ***comunicación*** como ***el modo en que se organizan y transmiten datos entre los procesos***. Hay dos formas de comunicación:
* **Memoria compartida**: los procesos ***intercambian información sobre una memoria compartida*** o ***actúan coordinadamente sobre datos residentes en ella***. Dado que ***no pueden operar todos juntos simultáneamente sobre la memoria compartida***, es que ***son necesarios mecanismos de bloqueo y liberación de acceso*** a ésta. Una ***solución*** simple a tal problemática podría ser un ***semáforo*** que ***habilitara o no el acceso*** a la memoria compartida.
* **Pasaje de mensajes**: se establece un ***canal lógico o físico*** para ***transmitir información*** entre los procesos. Para que la comunicación funcione, ***los procesos deben "saber" cuándo leer mensajes o cuándo transmitirlos***.

12. ¿Qué partes posee un programa concurrente?

Posee:

* Estado: **FALTA COMPLETAR QUE ESTADOS HAY**
* Acciones atómicas: son acciones que hacen una transformación de estado indivisible(estados intermedios invisibles para otros procesos). Cada programa está compuesto por un conjunto de sentencias que, a su vez, están compuestas por una o varias acciones atómicas. La ejecución de un programa concurrente representa un *interleaving* de las aaciones atómicas ejecutadas por procesos individuales.
* Historia de un programa concurrente(*trace*): ejecución de un programa concurrente con un *interleaving* particular. Es importante destacar que algunas historias son válidas y otras no, dado que unas pueden producir el efecto esperado y el intercalado de éstas puede no producirlo. Para restringir las historias de un programa concurrente a solo las permitidas existe la sincronización.

13. ¿Cómo se da la sincronización entre procesos?

La sincronización es la posesión de información acerca de otro proceso para coordinar actividades. Hay dos formas de sincronización:

* Por exclusión mutua: cuando se garantiza que un solo proceso tiene acceso a un recurso compartido en un instante de tiempo. Si el programa tuviere  **secciones críticas** que pudieren compartir más de un proceso, la exclusión mutua evita que dos o más procesos puedieren encontrarse en la misma sección crítica al mismo tiempo.
procesos.
* Por condición: cuando se bloquea la ejecución de un proceso hasta que se cumpla una condición dada.

Cuando un proceso toma una acción que invalida las supsiciones hechas por otro proceso se produce la **interferencia**.

14. ¿Qué tan importante es el tiempo absoluto en los sistemas concurrentes?

El tiempo absoluto no suele ser un factor clave en la mayoría de los sistemas. De hecho, éstos suelen ser actualizados frecuentemente con componentes más rápidos. Un programa concurrente no depende del tiempo absoluto sino de las secuencias. Es decir, pueden haber distintos *interleavings* en que se ejecutan las instrucciones de los diferentes procesos; los programas deben ser correctos para todos ellos.

15. ¿Qué es la granularidad y prioridad?

* Granularidad: está dada por la relación entre el cómputo y la comunicación. Está relacionada y se adapta a la arquitectura. Hay dos tipos:
    + Grano fino: cuando existen muchas comunicaciones y poco cálculo. Esto es, muchos procesadores pero con poca capacidad de cálculo, por lo que son mejores para programas que requieren más comunicaciones, es decir, aquellos que poseen más *tasks*.
    + Grano grueso: cuando existen pocas comunicaciones y mucho cálculo. Esto significa que se tienen pocos procesadores muy potentes, mu útiles para programas de mucho procesamiento y poca comunicación.

* Prioridad: es decir, cuál proceso se ejecuta primero y cuál después. Ésta puede provocar la suspensión de otro proceso concurrente. Del mismo modo, puede tomar un recurso compartido, obligando a retirarse a otro proceso que lo tenga en un instante dado.

16. ¿Cómo es el manejo de recursos?

Hay cinco tópicos a tocar en lo relativo a la administración de recursos:
* Cómo se asignan; los métodos de acceso, bloqueo y liberación; y la seguridad y consistencia.
* El ***fairness***.
* La ***inanición***.
* El ***overloading***.
* El ***deadlock***.

//FALTA DEFINIR EL PRIMERO

El ***fairness*** es una propiedad deseable que implica el equilibrio en el acceso a recursos compartidos por todos los procesos.  
Hay dos situaciones que no son para nada deseables: la ***inanición*** y el ***overloading***. La ***inanición*** se da cuando El ***overloading*** se da cuando la carga asignada a un proceso excede la capasidad de procesamiento.
Hay un problema muy importante que se debe evitar a toda costa: el ***deadlock***. El ***deadlock*** se produce cuando dos o más procesos, por error de programación, se queden esperando que el otro libere un recurso compartido. La propiedad de ***ausencia de deadlock*** es necesaria en los procesos concurrentes.  
Si existe deadlock es porque una(o varias) de éstas propiedades se dan:
* Recursos reusables serialmente: los procesos comparten recursos que pueden usar con exclusión mutua.
* Adquisición incremental: los procesos mantienen los recursos que poseen mientras esperan adquirir recursos adicionales.
* No hay suspensión: una vez que los recursos son adquiridos por un proceso, éstos no pueden quitarse de manera forzada, sino que solo son liberados voluntariamente.
* Espera cíclica: existe una cadena circular(ciclo) de procesos tal que cada uno tiene un recursos que su sucesor en el ciclo está esperando adquirir.

17. ¿Cuáles son los requerimientos de un lenguaje concurrente?

Son tres:

* Indicar las tareas o procesos que pueden ejecutarse concurrentemente.
* Mecanismos de sincronización.
* Mecanismos de comunicación entre los procesos.

18. ¿Qué problemas trae la concurrencia?

A *grosso modo*, hay 3 problemas:

* La complejidad.
* Pérdidad de la propiedad de *liveness*.
* El *overhead*.

El problema de la complejidad está dado por, a priori, la necesidad de utlizar mecanismos de sincronización. Por otro lado también es un tema de complejidad el no determinismo: dado que dos ejecuciones del mismo programa pueden ser(y probablemente sean) completamente diferentes es que la interpretación no suele ser simple y el *debug* aun menos. Otra razón de por qué es compleja la programación concurrente, radica en la adaptación del software concurrente al hardware paralelo para lograr una mejora real en el rendimiento. Por último, el tiempo de desarrollo y la puesta a punto suele ser mayor que en otros paradigmas: esto suele ser porque es muy difícil paralelizar algoritmos secuenciales.  
La pérdida de la propiedad de *liveness* se da en el momento que pudieren existir procesos iniciados que no estuvieren vivos. Esto pudiere llevar a deadlocks o una mala distribución de recursos.  
Por último, el *overhead* se da con los *context switches*, comunicación y sincronización asociados al programa que hacen que la *performance* pudiere reducirse.

19. ¿Qué instrucciones posee un programa concurrente?

Hay tres tipos básicos: la asignación, la alternativa y la iteración.
La asignación es atómica, por lo que es igual que en cualquier otro paradigma, no tiene mayor complejidad.
La iteración es también muy similar con algunas peculiaridades. A cada clausula la llamaremos guarda. Las guardas pueden ser simples, si es un chequeo booleano, o complejas, si hay varias expresiones involucradas. La instrucción de iteración puede componerse de una o varias guardas. Si la iteración posee una guarda, cuando se ejecute, podrán darse dos escenarios:
* Es posible ejecutar la guarda, por lo que es ejecutada.
* No es posible ejecutar la guarda, por lo que culmina la iteración.
La iteración siempre concluye solo cuando todas las guardas no son ejecutables.
Cuando hay más de una guarda, siempre se ejecuta una de forma arbitraria. Si es falsa se procede a ejecutar otra, hasta que sea posible ejecutar alguna o ninguna sea posible.
La alternativa es muy similar a la iteración solo que solo se chequea una vez si es ejecutable o no.