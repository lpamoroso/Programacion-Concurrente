# Ejercicio 1

Supongamos que tenemos una abuela que tiene dos tipos de lápices para dibujar: 10 de colores y 15 negros. Además tenemos tres clases de niños que quieren dibujar con los lápices: los que quieren usar sólo los lápices de colores (tipo C), los que usan sólo los lápices negros (tipo N), y los niños que usan cualquier tipo de lápiz (tipo A).
1. Implemente un código para cada clase de niño de manera que ejecute pedido de lápiz, lo use por 10 minutos y luego lo devuelva y además el proceso abuela encargada de asignar los lápices.
2. Modificar el ejercicio para que a los niños de tipo A se les puede asignar un lápiz sólo cuando: hay lápiz negro disponible y ningún pedido pendiente de tipo N, o si hay lápiz de color disponible y ningún pedido pendiente de tipo C.
NOTA: se deben modelar los procesos niño y el proceso abuela.

## Solucion

```c
process abuela(){
  int color = 10, negro = 15, id; char pencil;
  while(true){
    if(!empty(nieto_color) && color--)-->{
      receive request_nieto_color(id);
      send response_nieto_color[id]('C');
    }
    [](!empty(nieto_negro) && negro--)-->{
      receive nieto_negro(id);
      send response_nieto_negro[id]('N');
    }
    [](!empty(nieto_cualquiera) && color--)-->{
      receive nieto_cualquiera(id);
      pencil = choose_pencil(pencil);
      send response_nieto_cualquiera[id](pencil);
    }
    [](!empty(devolver_lapiz))-->{
      receive devolver_lapiz(pencil);
      if (pencil == 'C'){
        color++;
      }else{
        negro--;
      }
    }
  }
}

process negro[id=1..N](){
  char pencil;
  send request_nieto_negro(id);
  receive response_nieto_negro[id](pencil);
  delay(15*60);
  send devolver_lapiz(pencil);  
}

proceso color[id=1..N](){
  char pencil;
  send request_nieto_color(id);
  receive response_nieto_color[id](pencil);
  delay(15*60);
  send devolver_lapiz(pencil);  
}

proceso cualquiera[id=1..N](){
  char pencil;
  send request_nieto_cualquiera(id);
  receive response_nieto_cualquiera[id](pencil);
  delay(15*60);
  send devolver_lapiz(pencil);
}
```
