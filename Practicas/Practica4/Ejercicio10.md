# Ejercicio 10

Existe una casa de comida rápida que es atendida por 1 empleado. Cuando una persona llega se pone en la cola y espera a lo sumo 10 minutos a que el empleado lo atienda. Pasado ese tiempo se retira sin realizar la compra.

```c++
process persona[id_persona=1..P](){
  String estado;
  timer[id_persona]!set();
  timer[id_persona]?ready();
  ticket_dispenser!encolar(id_persona);
  estado[id_persona]?irse();
}

process timer[id_timer=1..P](){
  String estado;
  persona[id_timer]?set();
  persona[id_timer]!ready();
  delay(10*60);
  estado[id_timer]!cambiar_estado();
}

process ticket_dispenser(){
  queue cola; int id_persona;
  if();persona[*].encolar(id_persona) --> cola.push(id_persona);
  [](!cola.empty());empleado?quiero_atender() --> empleado!podes_atender(cola.pop());
}

process empleado(){
 int id_persona; String estado;
 while(true){
  ticket_dispenser!quiero_atender();
  ticket_dispenser?podes_atender(id_persona);
  estado[id_persona]!cambiar_estado();
}

process estado[id_estado=1..P](){
  String estado = "Esperando";
  while(true){
    if(estado == "Esperando"){
      if();timer[id_estado]?cambiar_estado() -->
        estado = "time_out";
        persona[id_estado].irse();
      []();empleado?cambiar_estado() -->
        estado = "Atendido";
        persona[id_estado].irse();
    }
  }
}
```
