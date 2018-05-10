# Ejercicio 5

Se debe modelar una casa de comida rápida, en la cual trabajan 2 cocineros y 3 vendedores. Además hay C clientes que dejan un pedido y quedan esperando a que se lo alcancen.  
Los pedidos que hacen los clientes son tomados por cualquiera de los vendedores y se lo pasan a los cocineros para que realicen el plato. Cuando no hay pedidos para atender, los vendedores aprovechan para reponer un pack de bebidas de la heladera (tardan entre 1 y 3 minutos para hacer esto). Repetidamente cada cocinero toma un pedido pendiente dejado por los vendedores, lo cocina y se lo entrega directamente al cliente correspondiente.  
Nota: maximizar la concurrencia.

```c++
process cliente[id_cliente=1..C](){
  Pedido pedido;
  pedido = generar_pedido();
  send request_dejar_pedido(id_cliente, pedido);
  receive response_cocinar_pedido[id_cliente](pedido);
  comer(pedido);
}

process vendedor[id_vendedor=1..3](){
  int id_cliente, minutes;
  Pedido pedido;
  while(true){
    if(!empty(request_dejar_pedido))-->
      receive request_dejar_pedido(id_cliente, pedido);
      send request_cocinar_pedido(id_cliente, pedido);
    [](empty(request_dejar_pedido))-->
      minutes = generate_number(1..3);
      delay(minutes);
    end if
  }
}

process cocinero[id_cocinero=1..2](){
  int id_cliente;
  Pedido pedido;
  while(true){
    if(!empty(request_cocinar_pedido))-->
      receive request_cocinar_pedido(id_cliente, pedido);
      pedido = cocinar_pedido();
      send response_cocinar_pedido[id_cliente](pedido);
    end if
  }
}
```
