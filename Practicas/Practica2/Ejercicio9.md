Ejercicio 9
======
Resolver el funcionamiento en una empresa de genética. Hay N clientes que
sucesivamente envían secuencias de ADN a la empresa para que sean analizadas y esperan
los resultados para poder envíar otra secuencia a analizar. Para resolver estos análisis, la
empresa cuenta con 2 servidores que van alternando su uso para no exigirlos de más (en
todo momento uno está trabajando y el otro descansando); cada 5 horas cambia el servidor
con el que se trabaja. El servidor que está trabajando toma un pedido(de a uno de acuerdo
al orden de llegada de los mismos), lo resuelve y devuelve el resultado al cliente
correspondiente; si al terminar ya han pasado las 5 horas, despierta al otro servidor y él
descansa; sino continúa con el siguiente pedido.

Solución
------
```c++
sem mutex_cola = 1;
sem mutex_current = 1;
sem mutex_empezar = 1;
sem barrier_pedido = 0;
sem barrier_timer = 0;
sem barrier_continuar[2] = ([1], 1; [2], 0);
sem barrier_resultado = 0;
int current = 1;
queue cola;

Process cliente[i=1..N](){
  while(true){
    P(mutex_cola);
    cola.push(pedido);
    V(mutex_cola);
    V(barrier_pedido);
    P(barrier_resultado);
  }
}

Process servidor[i=1..2](){
  Pedido pedido;
  while(true){
    P(barrier_continuar[i]);
    P(mutex_empezar);
    V(barrier_timer);
    P(mutex_current);
    while(i == current){
      V(mutex_current);
      P(barrier_pedido);
      P(mutex_cola);
      pedido = cola.pop();
      P(mutex_cola);
      //Analizar pedido
      V(barrier_resultado);
      P(mutex_current);
    }
    V(mutex_current);
    V(mutex_empezar);
  }
}

Process timer(){
  P(barrier_timer);
  delay(5*60*60);
  P(mutex_current);
  if(current == 1){
    current = 2;
  }else{
    current = 1;
  }
  V(barrier_continuar[current]);
  V(mutex_current);
}
```
