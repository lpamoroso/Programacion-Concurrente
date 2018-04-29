Ejercicio 1
======
Existen N personas que deben ser chequeadas por un detector de metales antes de poder ingresar al avión.
1. Implemente una solución que modele el acceso de las personas a un detector (es decir si el detector está libre, la persona lo puede utilizar; caso contrario, debe esperar).
2. Modifique su solución para el caso que haya tres detectores.

Solución
======
Parte 1
------
```c
sem mutex = 1;
Process Persona[i=1..N](){
  P(mutex);
  //La persona pasa por el detector
  V(mutex);
 }
```
Parte 2
------
```c
sem mutex = 3;
Process Persona[i=1..N](){
  P(mutex);
  //La persona pasa por el detector
  V(mutex);
 }
```
