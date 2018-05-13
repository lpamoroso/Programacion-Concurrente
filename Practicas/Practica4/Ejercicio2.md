# Ejercicio 2

Se desea modelar el funcionamiento de un banco en el cual existen 5 cajas para realizar pagos. Existen P personas que desean pagar. Para esto cada una selecciona la caja donde hay menos personas esperando, una vez seleccionada espera a ser atendido.
NOTA: maximizando la concurrencia.

```c++
chan request_atencion[5](int id);
chan response_atencion[P]();
chan request_elegir_caja[P](int id);
chan finish_atencion(int chosen);
chan response_caja[P](int chosen);

process caja[id_caja=1..5](){
  int id_persona;
  while(true){
    if(!empty(request_atencion))-->{
      receive request_atencion[id_caja](id_persona);
      //atender person
      send response_atencion[id_persona]();
    }
  }
}
process coordinador(){
  int id_persona, chosen;
  int caja[5] = ([5], 0);
  while(true){
    if(!empty(request_elegir_caja) && empty(finish_atencion))-->{
      receive request_elegir_caja(id_persona);
      chosen = get_position_that_has_the_lesser_value(caja);
      caja[chosen]++;
      send response_caja[id_persona](chosen);
    }
    [](!empty(finish_atencion))-->{
      receive finish_atencion(chosen);
      caja[chosen]--;
    }
  }
}
process persona[id_persona=1..N](){
  int chosen;
  send request_elegir_caja(id_persona);
  receive response_caja[id_persona](chosen);
  send request_atencion[chosen](id_persona);
  receive response_atencion[id_persona]();
  send finish_atencion(chosen);
}
```
