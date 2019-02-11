# Programación paralela

1. ¿Qué es un programa paralelo?

Es un programa concurrente escrito para resolver un problema en menos tiempo que el secuencial. El objetivo principal es reducir el tiempo de ejecución, o resolver problemas más grandes o con mayor precisión en el mismo tiempo. Un programa paralelo puede escribirse usando memeoria compartida o pasaje de mensajes.

2. ¿Qué extras trae consigo el paralelismo?

Existen dos modos tradicionales de descubrimiento científico: la teoría y la experimentación. Con el paralelismo surge un tercer modo que es la modelización computacional, es decir, utilizar computadoras para simular fenómenos del tipo "what if". Dentro de la modelización computacional existen tres técnicas:
* Computación de grillas: dividen una región espacial en un conjunto de puntos.
* Computación de partículas: modelos que simulan interaccciones de partículas individuales como moléculas u objetos estelares.
* Computación de matrices: sistemas de ecuaciones simultáneas.

3. ¿Cómo diseñar algoritmos paralelos?

No hay una receta, sino que es cuestión de creatividad. La mejor solución paralela puede diferir mucho de la sugerida por los algoritmos secuenciales. Hay dos etapas: descomposición y mapeo.

4. ¿Con qué métricas se mide el paralelismo?

Con la ganancia en performance. Hay otras medidas, pero deben tenerse en cuenta solo si favorecen a sistemas con mejor tiempo de ejecución. El tiempo de ejecución depende del tamaño de la entrada y de la arquitectura y número de procesadores. Esto hace que medir la performance sea complicado: ¿Qué interesa medir? ¿Qué indica que us sistema paralelo es mejor que otro? ¿Hasta que punto es bueno agregar más procesadores?  
Cuando se elige un problema y se mide la performance variando el número de procesadores entra en juego la ley de Amdahl, el speedup y la eficiencia. Otro tema es también la escalabilidad que determina hasta qué punto es eficiente incrementar el número de procesadores.

5. ¿A que llamamos tamaño de la entrada?

Está dada por el número de operaciones básicas necesarias para resolver un problema con el algoritmo secuencial más rápido. Si, por ejemplo, estamos resolviendo un problema con matrices y duplicamos la entrada, es incorrecto suponer que el tiempo se duplica, ya que está comprobado que éste no se incrementa 2, sino 8 veces para la multiplicación y 4 para la suma.

6. ¿Qué es el speedup?

El speedup es el cociente entre el tiempo de ejecución del algoritmo serial conocido más rápido y el tiempo de ejecución paralelo del algoritmo elegido. El speedup óptimo depende de la arquitectura.  
El speedup se ve afectado por varias causas:
* Alto porcentaje de código secuencial.
* Alto porcentaje de entrada/salida respecto de la computación.
* Algoritmo no adecuado.
* Excesiva contención de memoria.
* Tamaño del problema, es decir, si es chico, si es grande, si no crece, etc.
* Desbalanceo de carga, lo cual produce esperas ociosas en algunos procesadores.
* Overhead, es decir, ciclos adicionales en crear procesos, sincronizar, etc.

7. ¿Qué es la eficiencia?

Es el cociente entre el speedup y el speedup óptimo. Mide la fracción de tiempo en que los procesadores son útiles para el cómputo. El valor va entre 0 y 1, dependiendo de la efectividad de uso de los procesadores. 1 corresponde al speedup perfecto.

8. ¿Qué es el costo?

Es el producto entre el tiempo de ejecución paralelo y los procesadores. Refleja la suma del tiempo que cada procesador utiliza en la resolución del problema. Puede expresarse la eficiencia como el cociente entre el tiempo de ejecución del algoritmo secuencial conocido más rápido y el costo de resolver el problema en p procesadores.

9. ¿Qué es el grado de concurrencia?

El grado de concurrencia(o paralelismo) es el número máximo de tareas que pueden ejecutarse simultáneamente en cualquier momento del algoritmo paralelo. Para un tamaño de problema dado, el algoritmo paralelo no puede usar tantos procesadores como su paralelismo lo indique. El grado de concurrencia depende solo del algoritmo, no de la arquitectura.

10. ¿Qué es la granularidad?

Es la relación que existe entre la cantidad mínima/promedio de procesamiento con respecto a la cantidad mínima/promedio de comunicaciones. Esta noción impacta directamente en la complejidad del procesador: cuantas más operaciones entre comunicaciones realice y más independiente sea el procesador, más complejo será. Si la granularidad del algortmo difiere de la de la arquitectura, por lo general se tendrá pérdida de rendimiento.