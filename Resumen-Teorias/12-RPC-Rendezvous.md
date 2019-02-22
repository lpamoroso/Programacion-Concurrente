## RPC y rendezvous

1. ¿Por qué RPC o rendezvous?

Si bien es cierto que PMS se ajusta bien a problemas de filtros y pares que interactúan dado que plantea la comunicación bidireccional, lo cierto es que para resolver cliente-servidor la comunicación bidireccional obliga a especificar dos tipos de canales: requerimientos y respuestas. Además cada cliente necesita un canal de respuesta distinto.  
RPC y Rendezvous aparecen como técnicas de comunicación y sincronización entre procesos que suponen un canal bidireccional, ideal para la arquitectura cliente-servidor.

2. ¿Cuál es su estructura?

La idea es combinar una interfaz tipo monitor con operaciones exportadas a través de *calls* externas con mensajes sincrónicos; en otras palabras, demoran al *caller* hasta que la operación llamada se termina de ejecutar y se devuelven los resultados.

3. ¿Cómo funciona RPC?

La idea es descomponer los programas en módulos, con procesos y procedures, que podrían residir en espacios de direcciones distintos. Los procesos de un módulo pueden compartir variables y llamar solo a procedures de ese módulo. Los procesos de un móduloA puede comunicarse con procesos de otro móduloB sólo invocando procedimientos exportados por el móduloB. Cada módulo tiene una especificación e implementación de procedures. Se denomina background a los procesos locales de un módulo, para distinguirlos de las operaciones exportadas. Mediante un call module_name.operation_name, un proceso en un módulo llama a un procedure en otro. Si el llamado es local, el nombre del módulo puede omitirse.  
Dada la diferencia entre un llamado intermódulo y un llamado local, es que sus implmentaciones, naturalmente, difieren. Para el primero hay que tener en cuenta que el módulo al que se llama puede estar en una máquina distinta. En tal caso, se crea un nuevo proceso que sirve al llamado, y los argumentos son pasados como mensajes entre el llamador y el proceso server. El llamador, naturalmente, se demora tanto como el proceso servidor tarde en ejecutar el cuerpo del procedure que implementa a operation_name. Cuando el server vuelve de operation_name, envía los resultados al llamador y termina. Luego de recibir los resultados, el llamador sigue. Si el proceso llamador y el procedure están en un mismo espacio de direcciones, puede evitarse crear un nuevo proceso; lo cierto es que la mayoría de las veces el llamado será remoto y tendrá que crearse un proceso server.

4. ¿Cómo funciona rendezvous?

A diferencia de RPC, rendezvous combina comunicación y sincronización. Un cliente invoca una operación por medio de un call, pero esta operación es servida por un proceso existente en lugar de por uno nuevo. La idea es que se tiene un proceso servidor con una sentencia de entrada esperando por un call para actuar. Las operaciones se atienden una por vez, por lo que queda garantizada la sincronización.  
La especificación de un módulo contiene declaraciones de los headers de las operaciones exportadas, pero el cuerpo consta de un único proceso que sirve operaciones. Si un módulo exporta operation_name, el proceso server en el módulo realiza rendezvous con un llamador de operation_name ejecutando una sentencia de entrada. Una sentencia de entrada demora al proceso server hasta que haya, al menos, un llamado pendiente de operation_name; luego elige el llamado pendiente más viejo, copia los argumentos en los parámetros formales, ejecuta la sentencia de accept por parte del servidor y finalmente retorna los parámetros de resultado al llamador.

5. ¿En qué se diferencian?

En la manera de servir la invocación de operaciones. RPC propone declarar un procedure por cada operación y crear un nuevo proceso para manejar cada llamado, porque el llamador y el cuerpo del procedure pueden no estar en la misma máquina. Sin embargo, para el cliente, es como si todo estuviera en la misma máquina. Rendezvous propone un encuentro e intercambio entre dos procesos(algo así como una cita). La idea es que un proceso que espere su invocación(espere a que un proceso venga al encuentro). Cuando otro proceso lo invoque(acuda al encuentro), es ahí cuando el proceso se bloquea y garantiza la simetría entre dos procesos.  
De ahí, es que otra diferencia importante es que RPC puede servir varios pedidos al mismo tiempo, mientras que rendezvous solo uno por vez.  
Además, RPC, por sí mismo, es solo un mecanismo de comunicación; rendezvous es de comunicación y sincronización.  

6. ¿Por qué es necesario proveer sincronización dentro de los módulos en RPC? ¿Cómo puede realizarse tal sincronización?

RPC, por sí mismo, es solo un mecanismo de comunicación: si bien el proceso llamador y su server sincronizan, el único rol del server es actuar en nombre del llamador(como si éste estuviera ejecutando el llamado), por lo que la sincronización es solo implícita.  
También necesitaremos alguna manera para que los procesos en un módulo sincronicen con cada uno de los otros. Esto incluye tanto a los procesos server que están ejecutando llamados remotos como otros procesos declarados en el módulo. Como es habitual, esto comprende dos clases de sincronización: exclusión mutua y sincronización por condición. Existen dos enfoques para proveer sincronización, dependiendo de si los procesos en un módulo ejecutan con exclusión mutua o concurrentemente. Si ejecutan con exclusión mutua, las variables compartidas son protegidas automáticamente contra acceso concurrente, pero es necesario programar sincronización por condición. Para esto podríamos usar variables condición.  
Si ejecutan concurrentemente, necesitamos mecanismos para programar exclusión mutua y sincronización por condición ya que cada módulo es un programa concurrente. Para esto se puede utilizar desde semáforos hasta rendezvous. Por lo general, se asume que los procesos pueden ejecutar concurrentemente ya que es mas eficiente la ejecución en un multiprocesador que en memoria compartida. Se asume, además, que los procesos ejecutan concurrentemente utilizando, por ejemplo, time-slicing.