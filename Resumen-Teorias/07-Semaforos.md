# Semáforos

1. ¿Cómo surge el concepto del semáforo?

Dado que cualquier algoritmo del protocolo busy-waiting resulta complejo y sin clara separación entre variables de sincronización y variables para computar resultados, es que resulta difícil diseñar un algoritmo de éste tipo que sesuelva el problema. Incluso la verificación se torna densa conforme se incrementa el número de procesos. Como si fuera poco, es una técnica ineficiente si se la utiliza en multiprogramación: un procesador ejecutando un proceso *spinning* puede ser usado de manera más productiva por otro proceso.

2. ¿Qué es un semáforo?

Es una instancia de un *TAD*(**Tipo abstracto de dato**) con solo dos operaciones atómicas:
* P: señala la ocurrencia de un evento(incrementa).
* V: se usa para demorar un proceso hasta que ocurra un evento(decrementa).

3. Problemas básicos y técnicas con semáforos

### Barreras como señalizadoras de eventos

Si lo que se necesita es una *butterfly barrier* para *n* o sincronización con un coordinador central, entonces podría pensarse un semáforo para cada *flag* de sincronización. Un proceso setea el *flag* ejecutando **V**, y espera a que un *flag* sea seteado y luego lo limpia ejecutando **P**. Para armar la barrera de los dos procesos, es importante saber cuándo un proceso llega o parte de la barrera, por lo que los estados de los dos procesos deben relacionarse.

### Productores y consumidores como semáforos binarios divididos

La estructura del ***semáforo binario dividido*** puede verse como *n* semáforos binarios
///COMPLETAR

### Contadores de recursos como *buffers* limitados

Si los procesos compiten por recursos de multiples unidades, entonces es mejor usar ***contadores de recursos***. La idea es que cada semáforo cuente el número de unidades libres de un recurso determinado. Si lo vemos como un *buffer*(una cola de mensajes depositados y aun no buscados), existe un productor y un consumidor que depositan y retiran elementos del *buffer*. Dado que solo hay un productor y un consumidor, las operaciones de depósito y retirado pueden asumirse como atómicas. Si hubiera más de un productor y/o consumidor, entonces debe protegerse cada *slot* del *buffer* ya que podríase retirar dos veces el mismo dato o perderse otros datos por sobreescritura.

### Exclusión mutua selectiva aplicada al problema de los filósofos

Si, en cambio, estamos ante un problema de varios procesos y varios recursos, cada uno protegido por un *lock*, estamos ante una situación de ***exclusión mutua selectiva***. Para que un proceso se ejecute, antes debe adquirir los *locks* de los recursos que requiera. Cuando varios procesos compiten por conjuntos superpuestos de recursos, puede caerse en *deadlock* por lo que este problema debe manejarse con cautela.  
Desmenuzando el problema de los filósofos podemos plantear que:
* Cada tenedor es una sección crítica, y como tal, puede ser tomada por un único filósofo a la vez.
* Levantar un tenedor representa a *P*; dejarlo en la mesa, a *V*.
* Cada filósofo necesita el tenedor izquierdo y derecho.

// NECESITO EXPLICACIÓN DEL PROBLEMA DE LOS FILÓSOFOS.

### Lectores y escritores usando exclusión mutua selectiva

El problema consiste en una base de datos que es usada por lectores y escritores. Los lectores pueden ejecutar concurrentemente entre ellos si no hay escritores actualizando. El acceso de los escritores, por otro lado, debe ser exclusivo para evitar interferencia entre transacciones.
*A priori*, hay dos cuestiones a plantear:
* Los escritores necesitan acceso mutuamente exclusivo.
* Los lectores(como grupo) necesitan acceso exclusivo con respecto a cualquier escritor.

Una solución podría ser aquella en la que haya dos semáforos: uno que controle el acceso a la base de datos y otro que controle la cantidad de lectores. El proceso escritor basta que sea un *P* y *V* del semáforo de la base de datos. El proceso lector es un poco más complejo: la idea es que haya un semáforo que controle la cantidad de lectores para que todos puedan acceder a la base de datos concurrentemente usando el semáforo que controla esta y haya una manera de que se vaya liberando esta usando el semáforo que la controla decrementando una variable que controle la cantidad de lectores.

### Lectores y escritores usando sincronización por condición

Otro enfoque del problema anterior es que un lector puede acceder a la base de datos siempre y cuando un escritor no esté presente. Esto lo podemos representar fácilmente con un semáforo. El problema lo tenemos en el escritor: la idea es representar que el escritor puede entrar cuando no haya otro escritor y lectores. Esto no lo podemos implementar con un semáforo. Lo que vamos a usar es la técnica ***passing the baton*** para brindar exclusión y despertar procesos demorados usando semáforos binarios divididos. La idea es que cuando un procesos esté dentro de un sección crítica mantenga el batón, es decir, tenga permiso para ejecutarse. Cuando un proceso sale de la sección crítica, pasa el batón a otro proceso. Si se da que nadie está esperando el batón, entonces éste se libera para que lo tome el próximo proceso que trate de entrar.  
La solución, entonces consistiría en que, siempre que los lectores intentasen ejecutarse, si no hubieran escritores, pudieran hacerlo. Al final, cuando terminasen de ejecutarse, los lectores pasarían el batón, de ser posible, a los escritores, siempre y cuando no hubieren lectores dentro y haya escritores que quisieran escribir. Si hay un escritor ya escribiendo, los lectores son demorados hasta que éste les pase el batón.  
Por su parte, los escritores solo podrán escribir cuando no hubieren lectores ni otros escritores dentro de la base de datos. Si no pudieran escribir, entonces éstos son demorados hasta que se cumplan las condiciones ya mensionadas. Luego de escribir, se intentará pasar el batón a algún lector. De no ser esto posible, el batón será pasado a algún escritor. De ser esto tampoco posible, entonces solo se libera el batón.

3. ¿En qué consiste la alocación de recursos y *scheduling*?

Dado que debemos decidir cuándo se le puede dar a un proceso determinado acceso a un recurso, es que es que entonces debemos encontrar alguna forma de implementar ***políticas de alocación de recursos generales*** controlando explícitamente cuál proceso toma un recurso si hay más de uno esperando. Consideraremos **recurso** todo aquel elemento por el que un proceso puede ser demorado esperando adquirirlo. Para resolver este problema, implementaremos dos funciones: *request* y *release*. *Request* alocará inmediatamente un recurso a un proceso por algún tiempo, siempre y cuando el recurso esté libre. *Release* asignará el recurso, una vez liberado, al proceso demorado con el mínimo valor de tiempo. Si dos procesos tienen el mismo valor de tiempo, el recurso es alocado al que esperó más.  
Esta forma de decidir que recurso se da a qué proceso en un determinado momento es lo que llamamos *scheduling*. El *scheduling* mensionado arriba es el que denominamos *Shortest-job-next*(SJF) y no es *fair* dado que un proceso que llegó antes no será atendido si llega otro más corto. Esto puede mejorarse con la técnica de ***aging***, es decir, darle preferencia a un procesos que esperó más tiempo.