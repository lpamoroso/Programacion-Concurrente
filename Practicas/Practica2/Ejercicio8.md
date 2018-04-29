Ejercicio 8
======
Resolver el funcionamiento en una fábrica de ventanas con 7 empleados(4 carpinteros, 1
vidriero y 2 armadores) que trabajan de la siguiente manera:
* Los carpinteros continuamente hacen marcos(cada marco es armando por un único
carpintero) y los deja en un depósito con capacidad de almacenar 30 marcos.
* El vidriero continuamente hace vidrios y los deja en otro depósito con capacidad para
50 vidrios.
* Los armadores continuamente toman un marco y un vidrio(en ese orden) de los
depósitos correspondientes y arman la ventana(cada ventana es armada por un único
armador).

Solución
------
```c++
sem mutex_marco = 1;
sem mutex_vidrio = 1;
sem barrier_marco = 0;
sem barrier_vidrio = 0;
queue deposito_marcos;
queue deposito_vidrios;
int cant_marcos = 0;
int cant_vidrios = 0;
Process carpintero[i=1..4](){
  Marco marco;
  while(true){
    P(mutex_marco);
    if(cant_marcos < 30){
      cant_marcos++;
      V(mutex_marco)
      marco = Marco new;
      P(mutex_marco);
      deposito_marcos.push(marco);
      V(barrier_marco);
    }
    V(mutex_marco);
  }
}

Process carpintero(){
  Vidrio vidrio;
  while(true){
    P(mutex_vidrio);
    if(cant_vidrios < 30){
      cant_vidrios++;
      V(mutex_vidrio)
      vidrio = Vidrio new;
      P(mutex_vidrio);
      deposito_vidrios.push(vidrio);
      V(barrier_vidrio);
    }
    V(mutex_vidrio);
  }
}

Process armador[i=1..2](){
  while(true){
    Marco marco;
    Vidrio vidrio;
    P(barrier_marco);
    P(mutex_marco);
    marco = deposito_marcos.pop(Marco new);
    cant_marcos--;
    V(mutex_marco);
    P(barrier_vidrio);
    P(mutex_vidrio);
    vidrio = deposito_vidrios.pop(Vidrio new);
    cant_vidrios--;
    V(mutex_vidrio);
    //Armar ventana
  }
}
```
