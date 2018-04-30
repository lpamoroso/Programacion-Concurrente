Ejercicio 3
======
Suponga que N personas llegan a la cola de un banco. Una vez que la persona se agrega en la cola no espera más de 15 minutos para su atención, si pasado ese tiempo no fue atendida se retira. Para atender a las personas existen 2 empleados que van atendiendo de a una y por orden de llegada a las personas.

Solución
------
```c
process persona[i=1..N](){
  Banco.esperar(i);
  Admin_Persona[i].set(i);
}

process empleado[i=1..2](){
  int id;
  while(true){
    Banco.desencolar(id);
    Admin_Persona[id].atender();
  }
}

process timer[i=1..N](){
  Admin_Persona[i].await();
  delay(15*60);
  Admin_Persona[i].irme();
}

Monitor Banco(){
  queue clientes;
  cond empleados;
  
  procedure encolar(int id){
    clientes.push(id);
    signal(empleados);
  }
  
  procedure desencolar(var int id){
    while(clientes.isEmpty()){
      wait(empleados);
    }
    id = clientes.pop();
  }
}

Monitor Admin_Persona[i=1..N](){
  cond persona;
  cond cola;
  String estado = "Esperando";
  
  procedure atender(){
    if(estado == "Esperando"){
      //Atender cliente
      estado = "Atendido";
    }
  }
  
  procedure irme(){
    if(estado == "esperando"){
      estado = "time_out";
    }
  }
  
  procedure set(){
    signal(cola);
    wait(persona);
  }
  
  procedure await(){
    wait(cola);
  }
}
```
