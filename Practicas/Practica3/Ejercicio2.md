Ejercicio 2
======
En un laboratorio de genética se debe administrar el uso de una máquina secuenciadora de ADN. Esta máquina se puede utilizar por una única persona a la vez. Existen 100 personas en el laboratorio que utilizan repetidamente esta máquina para sus estudios, para esto cada persona pide permiso para usarla, y cuando termina el análisis avisa que termino. Cuando la máquina está libre se le debe adjudicar a aquella persona cuyo pedido tiene mayor prioridad (valor numérico entre 0 y 100).

Solución
------

```c
process cliente[id=1..100](){
  Maquina.pedir(id);
  Maquina.dejar();
}

Monitor Maquina(int id){
  cond cola[100];
  priority_queue clientes;
  bool busy = false;
  
  procedure pedir(){
    if(busy){
      clientes.push(id);
      wait(cola);
    }
    busy = true;
  }
  
  procedure dejar(){
    int next;
    busy = false;
    if(!clientes.isEmpty()){
      next = clientes.pop();
      signal(cola[next]);
    }
  }
}
```
