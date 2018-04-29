Ejercicio 4
======
Se tiene un curso con 40 alumnos. La maestra entrega una tarea distinta a cada alumno; luego cada alumno realiza su tarea y se la entrega a la maestra para que la corrija; ésta revisa la tarea y si está bien le avisa al alumno que puede irse; si la tarea está mal, le indica los errores al alumno y éste corregirá esos errores y volverá a entregarle la tarea a la maestra para que realice la corrección nuevamente hasta que la tarea no tenga errores.

Solución
------
```c++
sem barrier_alumno[40] = ([40], 0);
sem barrier_maestra = 0;
sem mutex_alumno = 1;
sem barrier_correccion[40] = ([40], 0);
bool solved[40] = ([40], false);
String resolucion[40] = ([40], "");
String tarea[40] = ([40], "");
int id;

Process alumno[i=1..40]{
  P(barrier_alumno[i]);
  while(!solved[i]){
    resolucion[i] = hacer_tarea(tarea[i]);
    P(mutex_alumno);
    id = i;
    V(barrier_maestra);
    P(barrier_correccion[i]);
  }
}

Process maestra(){
  int aprobados = 0;
  bool esta_bien;
  int i;
  for(i = 1; i <= 40; i++){
    tarea[i] = dar_tarea();
    V(barrier_alumno[i]);
  }
  while (aprobados < 40){
    P(barrier_maestra);
    esta_bien = chequear_tarea(resolucion[id]);
    if(esta_bien){
      solved[i] = true; aprobados++;
    }
    V(barrier_correccion[id]);
    V(mutex_alumno);
  }
}
```
