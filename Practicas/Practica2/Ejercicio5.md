Ejercicio 5
======
Suponga que se tiene un curso con 50 alumnos. Cada alumno elije una de las 10 tareas para realizar entre todos. Una vez que todos los alumnos eligieron su tarea comienzan a realizarla. Cada vez que un alumno termina su tarea le avisa al profesor y se queda esperando el puntaje del grupo. Cuando todos los alumnos que tenían la misma tarea terminaron el profesor les otorga un puntaje que representa el orden en que se terminó esa tarea.
<br><b>Nota:</b> <em>Para elegir la tarea suponga que existe una función elegir que le asigna una tarea a un alumno (esta función asignará 10 tareas diferentes entre 50 alumnos, es decir, que 5 alumnos tendrán la tarea 1, otros 5 la tarea 2 y así sucesivamente para las 10 tareas).</em>

Solución
------
```c++
sem mutex_alumno = 1;
sem mutex_cantidad = 1;
sem barrier_maestra = 0;
sem barrier_correccion[10] =([10], 0);
sem barrier_alumno[50] = ([50], 0);
int contador[10] = ([10], 0);
int notas[10];
int tareas[50];
int id;
int cant = 0;

Process Alumno[i=1..50](){
  tareas[i] = elegir_tarea();
  P(mutex_cantidad);
  cant++;
  if(cant == 50){
    for( = 1; j <= 49; j++){
      V(barrier_alumno[j]);
    }
  }
  else{
    V(mutex_cantidad);
    P(barrier_alumno[i]);
  }
  V(mutex_cantidad);
  //Hacer tarea
  P(mutex_alumno);
  id = i;
  V(barrier_maestra);
  P(barrier_correccion[tareas[i]]);
}

Process Maestra(){
  int orden = 1;
  while(orden <= 10){
    P(barrier_maestra);
    contador[tareas[id]]++;
    if(contador[tareas[id]] == 5){
      notas[tareas[id]] = orden;
      orden++;
      for(i = 0; i < 5; i++){
        V(barrier_correccion[tareas[id]]);
      }
    }
    V(mutex_alumno);
  }
}
```
