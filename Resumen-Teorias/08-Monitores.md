### Monitores

1. ¿Qué es un monitor?

Un ***monitor*** es un módulo de programa con más estructura que un semáforo y que puede ser implementado tan eficientemente como éste. A través de la abstracción de datos encapsula las representaciones de datos y brinda un conjunto de operaciones, siendo éstas los únicos medios para manipular los recursos. También contienen variables que almacenan el estado del recurso y procedimientos que implementan las operaciones sobre el.

2. Propiedades de un monitor

* Exclusión mutua: está implícita ya que los procedures en el mismo monitor no ejecutan concurrentemente.
* Sincronización por condición: explícita con variables condición.

En un programa concurrente, los procesos interactúan con los monitores invocando los *procedures* de éstos. Esto permite que los procesos(y también los programadores) puedan ignorar cómo están implementados.

3. Notación de un monitor

Un monitor agrupa la representación y la implementación de un recurso compartido. No es un TAD dado que es compartido por procesos que ejecutan concurrentemente. Los monitores están compuestos por dos partes: 
* Interfaz: especifica operaciones que brinda el recurso.
* Cuerpo: tiene variables que representan el estado del recurso y *procedures* que implementan las operaciones de la interfaz.

Solo los nombres de los procedures de un monitor son visibles desde afuera. A su vez, los procedures solo pueden acceder a sus variables locales y parámetros que le sean pasados en la invocación.  
El programador no puede conocer, a priori, el orden de llamado.

4. Sincronización en monitores

La sincronización por condición es programada explícitamente con variable condición. El valor asociado a éstas variables es una cola de procesos demorados, no visible directamente al programador. Permiten las siguientes operaciones:
* *wait(cv)*: el proceso es demorado al final de la cola *cv* y deja el acceso exclusivo al monitor.
* *signal(cv)*: despierta al proceso que esté al frente de la cola(si hubiera alguno) y lo saca de ella. El proceso despertado recién podrá ejecutar cuando requiera el acceso exclusivo al monitor.
* *signal_all(cv)*: despierta todos los procesos demorados en *cv*, quedando vacía la cola asociada a *cv*.
* *empty(cv)*: retorna *true* si cv está vacía.
* *wait(cv, rank)*: el proceso se demora en la cola cv en orden ascendente de acuerdo al parámetro *rank* y deja el acceso exclusivo al monitor.
* *minrank(cv)*: retorna el mínimo rango de demora.

Hay dos disciplinas de señalización:
* *Signal and continued*: el proceso que hace el *signal* continúa usando el monitor, y el proceso despertado pasa a competir por acceder nuevamente al monitor para continuar con su ejecución(en la instrucción que le siga al *wait*).
* *Signal and wait*: el proceso que hace el *signal* pasa a competir por acceder nuevamente al monitor, mientras que el proceso despertado pasa a ejecutarse dentro del monitor a partir de la instrucción que le siga al *wait*.

5. *Wait & signal* VS. *P & V*

| *Wait* | *P* |
| :---: | :---: |
| El proceso siempre se duerme | El proceso se duerme si la variable(el semáforo) es 0 |

| *Signal* | *V* |
| :---: | :---: |
| Si hay procesos dormidos, despierta al primero de ellos; caso contrario no hace nada. | Incrementa la variable(el semáforo) en uno para que un proceso dormido o que hará un P continúe. No sigue ningún orden al despertarlos. |

6. Ejemplos

### Alocacion SJN

Podría implementarse usando un *wait* con prioridad para ordenar los procesos demorados por la cantidad de tiempo que usarán el recurso. *Empty* determinaría si hay procesos demorados. Cuando el recurso es liberado, si hay procesos demorados se despierta al que tiene mínimo *rank*. *Wait* no debe ser un *loop*, ya que la decisión de cuándo puede continuar un proceso la hace el proceso(valiere la redundancia) que libera el recurso. Si no se deseara utilizar el wait con prioridad, podríase usar una cola ordenada y variables condición privadas.

### Reloj lógico

Podría implementarse un timer que permita a los procesos dormirse una cantidad de unidades de tiempo. Podríase controlar todo con dos operaciones:
* demorar(*interval*): demora al llamdor durante *interval* ticks de reloj.
* *tick*: incrementa el valor del reloj lógico. La función es llamada por un proceso que es despertado periódicamente por un *timer* de *hardware* y tien alta prioridad de ejecución.

La mejor manera de implementarlo es con un wait con prioridad en el cual se indique la hora a despertar; el proceso tick se encargará de dar la señal al proceso cuando sea el momento(itera repetidamente hasta que llegue la hora de despertar al primero).