# Ejercicio 9

Se debe modelar la atención en una panadería por parte de 3 empleados. Hay C clientes que ingresan al negocio para ser atendidos por cualquiera de los empleados, los cuales deben atenderse de acuerdo al orden de llegada.

```c++
process cliente[id_cliente=1..C](){
  Pedido pedido;
  ticket_dispenser!encolar(id_cliente);
  empleado?atendido(pedido);
}

process ticket_dispenser(){
  queue cola; int id_cliente; Pedido pedido;
  while(true){
		if();cliente[*]?encolar(pedido, id_cliente) --> cola.push(pedido, id_cliente);
		[](!cola.empty());empleado[*]?quiero_cliente(id_empleado) --> empleado[id_empleado]!toma_cliente(pedido, id_cliente);
		end if
  }
}

process empleado[id_empleado=1..3](){
  int id_cliente, Pedido pedido;
  while(true){
    ticket_dispenser!quiero_cliente(id_empleado);
    ticket_dispenser?toma_cliente(pedido, id_cliente);
    pedido = armar_pedido();
    cliente[id_cliente]!atendido(pedido);
  }
}
```
