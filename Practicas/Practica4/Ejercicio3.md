# Ejercicio 3

En un edificio existen 3 porteros y P personas. Las personas dejan reclamos en la oficina de los porteros que deben ser atendidos por cualquiera de ellos. Cada portero está continuamente trabajando, si hay algún reclamo pendiente lo atiende, sino realiza un recorrido por los pisos durante 10 minutos.

```c++
chan request_reclamo(String reclamo);
chan pedir_reclamo(int id);
chan response_reclamo[3](String reclamo);

process persona[id_persona=1..P](){
  String reclamo;
  reclamo = generar_reclamo();
  send request_reclamo(reclamo);
}

process buzon(){
  String reclamo; queue cola; int id;
  while(true){
    receive pedir_reclamo(id);
    if(!empty(request_reclamo))-->
      receive request_reclamo(reclamo); 
      send response_reclamo[id](reclamo);
    [](empty(request_reclamo))-->
      send response_reclamo[id](-1);
    end if
  }
}

process portero[id_portero=1..3](){
  String reclamo;
  while(true){
    send pedir_reclamo(id_portero);
    receive response_reclamo[id_portero](reclamo);
    if(reclamo == -1){
      delay(10);
    }
    else{
      atender_reclamo(reclamo);
    }
  }
}
```
