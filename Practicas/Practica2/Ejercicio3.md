Ejercicio 3
======
A una cerealera van T camiones a descargarse trigo y M camiones a descargar maíz. Sólo hay lugar para que 7 camiones, a la vez, descarguen, pero no pueden ser más de 5 del mismo tipo de cereal.
<br><strong>Nota:</strong> <em>no usar un proceso extra que actúe como coordinador, resolverlo entre los camiones.</em>

Solución
------
```c
sem mutex_lugar = 7;
sem mutex_lugar_trigo = 5;
sem mutex_lugar_maiz = 5;

process camion_trigo[i=1..T](){
  P(mutex_lugar_trigo);
  P(mutex_lugar);
  //Descargar trigo.
  V(mutex_lugar_trigo);
  V(mutex_lugar);
}

process camion_maiz[i=1..M](){
  P(mutex_lugar_maiz);
  P(mutex_lugar);
  //Descargar maiz.
  V(mutex_lugar_maiz);
  V(mutex_lugar);
}
```
