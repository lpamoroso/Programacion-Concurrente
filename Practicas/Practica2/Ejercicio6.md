Ejercicio 6
======
A una empresa llegan E empleados y por día hay T tareas para hacer (T>E), una vez que
todos los empleados llegaron, empezarán a trabajar. Mientras haya tareas para hacer los
empleados tomarán una y la realizarán. Cada empleado puede tardar distinto tiempo en
realizar cada tarea. Al finalizar el día, se le da un premio al empleado que más tareas
realizó.

Solución
------
```c++
sem mutex_cant = 1;
sem mutex_tareas = 1;
sem mutex_contador_tareas = 1;
sem barrier_empleado[E] = ([E], 0);
sem barrier_estoy_listo = 0;
sem barrier_esperar_ganador[E] = ([E], 0);
int contador[E] = ([E], 0);
queue tareas;
int cant = 0;

Process empleado[i=1..E](){
  Tarea tarea;
  int j;
  P(mutex_cant);
  cant++;
  if(cant == E){
    for(j = 1; j < E; j++){
      V(barrier_empleado[j]);
    }
  }
  else{
    V(mutex_cant);
    P(barrier_empleado[i]);
  }
  V(mutex_cant);
  P(mutex_tareas);
  while(!tareas.isEmpty()){
    tarea = tareas.pop();
    V(mutex_tareas);
    //Realizar tarea
    P(mutex_contador_tareas);
    contador[i]++;
    V(mutex_contador_tareas);
    P(mutex_tareas);
  }
  V(mutex_tareas);
  V(barrier_estoy_listo);
  P(barrier_esperar_ganador[i]);
}

Process coordinador(){
  int i;
  for(i = 1; i <= E; i++){
    P(barrier_estoy_listo);
  }
  int max = -1;
  int nro_empleado;
  for(i = 1; i <= E; i++){
    if(contador[i] > max){
      max = contador[i];
      nro_empleado = i;
    }
  }
  println("El empleado ganador es el %d con %d tareas completadas.", nro_empleado, max);
  for(i = 1; i <= E; i++){
    V(barrier_esperar_ganador[i]);
  }
}
```
