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

//cOMPLETAR COSO HARDWARE

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
