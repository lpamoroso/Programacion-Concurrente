# Ejercicio 6

Suponga que N personas llegan a la cola de un banco. Una vez que la persona se agrega en la cola no espera más de 15 minutos para su atención, si pasado ese tiempo no fue atendida se retira. Para atender a las personas existen 2 empleados que van atendiendo de a una y por orden de llegada a las personas.

```c++
chan set_timer[N]();
chan sync_timer[N]();
chan request_estado[N](String estado);
chan request_atencion(int id);
chan response_atencion[N]();

process persona[id_persona=1..N](){
  String estado = "Esperando";
  send set_timer[id_persona]();
  receive sync_timer[id_timer]();
  send request_estado[id_persona](estado);
  send request_atencion(id_persona);
  receive response_atencion[id_persona]();
}

process timer[id_timer=1..N](){
  String estado;
  receive set_timer[id_timer]();
  send sync_timer[id_timer]();
  delay(15*60);
  receive request_estado[id_timer](estado);
  if(estado == "Esperando"){
    send request_estado[id_timer]("Out");
    send response_atencion[id_timer]();
  }
}

process empleado[id_empleado=1..2](){
  String estado; int id_persona;
  while(true){
    receive request_atencion(id_persona);
    receive request_estado[id_persona](estado);
    if(estado == "Esperando"){
      send request_estado[id_persona]("Out");
      //Atender
      send response_atencion[id_persona]();
    }
  }
}
```
