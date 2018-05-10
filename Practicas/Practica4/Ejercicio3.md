# Ejercicio 3

En un edificio existen 3 porteros y P personas. Las personas dejan reclamos en la oficina de los porteros que deben ser atendidos por cualquiera de ellos. Cada portero está continuamente trabajando, si hay algún reclamo pendiente lo atiende, sino realiza un recorrido por los pisos durante 10 minutos.

```c++
process persona[id_persona=1..P](){
  String reclamo;
  reclamo = generar_reclamo();
  send request_reclamo(reclamo);
}

process portero[id_portero=1..3](){
  String reclamo;
  while(true){
    if(!empty(request_reclamo))-->
      receive request_reclamo(reclamo);
      atender_reclamo(reclamo);
    [](empty(request_reclamo))-->
      delay(10);
    end if
  }
}
```