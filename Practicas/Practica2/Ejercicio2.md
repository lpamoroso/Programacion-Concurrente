Ejercicio 2
======
Un sistema operativo mantiene 5 instancias de un recurso almacenadas en una cola. Cuando un proceso necesita usar una instancia del recurso, la saca de la cola, la usa y cuando termina de usarla la vuelve a depositar.

Solución
------
```c++
queue recursos;
sem mutex_queue_instances = 1;
int queue_instances = 5;

Process process[i=1..N](){
  Recurso recurso;
  P(mutex_queue_instances);
  while(queue_instances > 0){
    queue.pop(recurso);
    queue_instances--;
    V(mutex_queue_instances);
    //Usar el recurso.
    P(mutex_queue_instances);
    queue.push(recurso);
    queue_instances++;
  }
  V(mutex_queue_instances);
}
```
