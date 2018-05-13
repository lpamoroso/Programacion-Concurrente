# Ejercicio 5

Se desea modelar una competencia de atletismo. Para eso existen dos tipos de procesos: C corredores y un portero. Los corredores deben esperar que se habilite la entrada a la pista, donde deben esperar que lleguen todos los corredores para comenzar. El portero es el encargado de habilitar la entrada a la pista.  

Notas:

* El proceso portero NO puede contabilizar nada, su única función es habilitar la entrada a la pista.
* NO se puede suponer ningún orden en la llegada de los corredores al punto de partida.

```c++

process corredor[id_corredor= 1..C](){
  int cant;
  receive pass_the_baton(cant);
  if(cant == C){
    send request_gates();
  }else{
    send pass_the_baton(cant + 1);
  }
  receive response_gates[id_corredor]();
}

process portero(){
  int cant = 0, i;
  send pass_the_baton(cant);
  receive request_gates();
  for (i=1; i <= C; i++){
    send response_gates[i]();
  }
}

```
