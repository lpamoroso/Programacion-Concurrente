Ejercicio7
======
Resolver el uso de un equipo de videoconferencia que puede ser usado por una única persona a la vez. Hay P Personas que utilizan este equipo (una única vez cada uno) para su trabajo de acuerdo a su prioridad. La prioridad de cada persona está dada por un número entero positivo. Además existe un Administrador que cada 3 hs. incrementa en 1 la prioridad de todas las personas que están esperando por usar el equipo.

Solución
------
```c
process personas[i=1..P](){
  int prioridad;
  prioridad = dar_prioridad();
  Equipo.usar(i, prioridad);
  //Usar equipo
  Equipo.dejar_equipo();
}

process Administrador(){
  while(true){
    delay(3*60*60);
    Equipo.incrementar_prioridades();
  }
}

Monitor Equipo(){
  cond cola[P];
  priority_queue cola_prioridad();
  bool busy = false;
  
  procedure usar(int id, int prioridad){
    while(busy){
      cola_prioridad.push(prioridad, id);
      wait(cola[id]);
    }
    busy = true;
  }
  
  procedure dejar_equipo(){
    int id, prioridad;
    busy = false;
    priority_queue.pop(prioridad, id);
    signal(cola[id]);
  }
  
  procedure incrementar_prioridades(){
    //Incrementar en uno las prioridades a procesos en priority_queue
  }  
}
```
