# Pasaje de mensajes

1. ¿Qué es?

Al igual que memoria compartida o RPC, es un método de sincronización entre los procesos que quieran comunicarse entre ellos.  
Un mensaje es un pedazo de información estructurada que envía un agente a oro a través de un canal de comunicación. El pasaje de mensajes es imprescindible en sistemas distribuídos dado que en este caso no existen recursos directamente compartidos para intercambiar información entre procesos.  
Como los semáforos, monitores o cualquier otra técnica, la idea es aportar sincronización entre procesos y permitir la exclusión mutua. En particular, esta técnica no requiere de memoria compartida, de ahí su importancia en la programación de sistemas distribuídos y la exclusión mutua viene asegurada.  
En el pasaje de mensajes hay tres elementos muy bien definidos: el proceso que envía, aquel que recibe y el mensaje.  

2. ¿Qué tipos existen?

Existen dos tipos:

* Pasaje de mensajes asincrónico(PMA): el proceso que envía no espera a que sea recibido el mensaje enviado. Puede que éstos tengan un buzón para mantener los mensajes que se han enviado previamente y no han sido recibidos aun.
* Pasaje de mensajes sincrónico(PMS): el proceso que envía el mensaje espera a que éste sea recibido antes de generar y enviar otro mensaje.

3. ¿Cuándo es útil?

Es útil cuando:

* La necesidad de comunicación es simple.
* La cantidad de información que se puede trasnmitir, por unidad de tiempo, es crítica.
* El alcance del sistema es limitado, es decir, es preferible una implementación rápida que un diseño flexible y sofisticado.
* Hay que evitar protocolos de redes.
* No están disponibles protocolos de objetos remotos.

## Pasaje de mensajes asincrónico

1. ¿Cómo se estructura?

Utiliza canales, es decir, colas de mensajes enviados y aun no recibidos. Hay dos operaciones:
* Send: un proceso agrega un mensaje al final de la cola(que se supone es ilimitada) de un canal, que no bloquea al emisor.
* Receive: un proceso recibe un mensaje desde un canal, que demora al receptor hasta que en el canal haya, al menos, un mensaje; luego toma el primero y lo almacena en variables locales. Las variables locales deben tener el mismo tipo que la declaración del canal. Cuando no hay mensajes, el receive produce una espera, una demora, un bloqueo por lo que es una operación bloqueante. Es importante tener esto en cuenta para evitar deadlocks. No debe realizarse polling sobre el receive.  

El acceso a los contenidos de cada canal se supone atómico y de tipo FIFO y, aunque en este caso se consideren ilimitados, las implementaciones reales tienen un tamaño de buffer asignado. También se supone que ningún mensaje se pierde o se modifica en el camino y que todo mensaje enviado es eventualmente leído. Los canales entienden empty, es decir, si la cola del canal está vacía. Es una operación que sirve, entre otras cosas, cuando el proceso puede hacer trabjo productivo mientras espera un mensaje, pero debe usarse cuidadosamente dado que podría llegar un mensaje instantes despúes en que se evalúa el empty; del mismo modo, podría ser false y no haber más mensajes cuando siga ejecutando.  
Los canales son globales ya que pueden ser compartidos. Hay varias implementaciones, entre ellas:
* Mailbox: si cualquier proceso puede enviar o recibir por el canal. Se dice, en este caso, que la comunicación es bidireccional.
* Input port: si el canal tiene un solo receptor y muchos emisores. 
* Link: si el canal tiene un único emisor y un único receptor.

## Pasaje de mensajes sincrónico

1. ¿En qué se diferencia con PMA?

Los canales son punto a punto, por lo que la primitiva send es bloqueante, es decir, quédase esperando hasta que el mensaje llegare al receptor. Esto hace que las colas no sean ilimitadas, sino de un mensaje, por lo que se ahorra memoria.
Una de las diferencias(y algunas veces inconvenientes) del PMS con respecto al PMA es que, naturalmente, el grado de concurrencia se reduce bastante dado que el flujo es más estricto, a diferencia del PMA. Esto hace que en algunos casos, por ejemplo el de productor-consumidor, si hay operaciones que llevan más tiempo que otras, los pares send/receive se completarán asumiendo la demora del proceso que tarde más. Con PMA, la mayor concurrencia lograda a partir de la independencia de los pares hace que tarde mucho menos. Este efecto puede lograrse en PMS agregando un buffer en medio, a costa de la complejidad adicional.
Otra interacción afectada por la pérdida de la concurrencia es la de cliente-servidor. En este caso es más evidente ya que cuando un cliente está liberando un recurso, no habría motivo para demorarlo hasta que el servidor reciba el mensaje, pero ésta es la forma que propone PMS.  
Otra desventaja del PMS viene dada también por la naturaleza del send sincrónico, el cual es más proclive a deadlock.