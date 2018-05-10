# Ejercicio 6

Suponga que N personas llegan a la cola de un banco. Una vez que la persona se agrega en la cola no espera más de 15 minutos para su atención, si pasado ese tiempo no fue atendida se retira. Para atender a las personas existen 2 empleados que van atendiendo de a una y por orden de llegada a las personas.

```c++
process persona[id_persona=1..N](){
  send set_timer[id_persona]();
  send request_atencion();
}

process timer[id_timer=1..N](){
  receive set_timer[id_timer]();
  delay(15*60);
  send time_over();//check
}

process

process empleado[id_empleado=1..2](){
  while(true){
    receive request_atencion();
  }
}
```
