Ejercicio 7
======
Existe una casa de comida rápida que es atendida por 1 empleado. Cuando una persona
llega, se pone en la cola y espera a lo sumo 10 minutos a que el empleado lo atienda. Pasado
ese tiempo se retira sin realizar la compra.

Solución
------
```c++
sem mutex_cola = 1;
sem mutex_estado = 1;
sem barrier_timer_set[P] = ([P], 0);
sem barrier_atencion[P] = ([P], 0);
sem barrier_empleado = 0;
queue cola;
String estado[P] = ([P], "Esperando");

Process cliente[i=1..P](){
  P(mutex_cola);
  cola.push(i);
  V(mutex_cola);
  V(barrier_empleado);
  V(barrier_timer_set[i]);
  P(barrier_atencion[i]);
}

Process timer[i=1..P](){
  P(barrier_timer_set[i]);
  delay(10*60);
  P(mutex_estado);
  if(estado[i] == "Esperando"){
    estado[i] = "Me fui"
    V(mutex_estado);
    V(barrier_atencion[i]);
    P(mutex_estado);
  }
  V(mutex_estado);
}

Process empleado(){
  int id;
  while(true){
    P(barrier_empleado);
    P(mutex_cola);
    id = cola.pop();
    V(mutex_cola);
    P(mutex_estado);
    if(estado[id] == "Esperando"){
      estado[id] = "Atendido"
      V(mutex_estado);
      //Atender cliente
      V(barrier_atencion[id]);
      P(mutex_estado);
    }
    V(mutex_estado);
  }
}
```
