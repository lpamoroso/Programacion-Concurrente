# Ejercicio 8

Supongamos que tenemos una abuela que tiene dos tipos de lápices para dibujar: 10 de colores y 15 negros. Además tenemos tres clases de niños que quieren dibujar con los lápices: los que quieren usar sólo los lápices de colores (tipo C), los que usan sólo los lápices negros (tipo N), y los niños que usan cualquier tipo de lápiz (tipo A).
1. Implemente un código para cada clase de niño de manera que ejecute pedido de lápiz, lo use por 10 minutos y luego lo devuelva y además el proceso abuela encargada de asignar los lápices.
2. Modificar el ejercicio para que a los niños de tipo A se les puede asignar un lápiz sólo cuando: hay lápiz negro disponible y ningún pedido pendiente de tipo N, o si hay lápiz de color disponible y ningún pedido pendiente de tipo C.  

NOTA: se deben modelar los procesos niño y el proceso abuela.

1.
```c++
process abuela(){
  int color = 10, negro = 15, id; queue cola_colores, cola_negros, cola_cualquiera; char lapiz;
  
  if(color > 0)nieto_color[*]?request_color(id);-->
    color--;
    nieto_color!response_color[id]('C');
  [](negro > 0)nieto_negro[*]?request_negro(id);-->
    negro--;
    nieto_negro!response_negro[id]('N');
  [](color > 0)nieto_cualquiera[*]?request_lapiz(id);-->
    color--;
    nieto_lapiz!response_lapiz[id]('C');
  [](negro > 0)nieto_cualquiera[*]?request_lapiz(id);-->
    negro--;
    nieto_lapiz!response_lapiz[id]('N');
  [](color == 0)nieto_color[*]?request_color(id);-->
    cola_colores.push(id);
  [](negro == 0)nieto_negro[*]?request_negro(id);-->
    cola_negros.push(id);
  []((negro == 0)||(color == 0))nieto_cualquiera[*]?request_cualquiera(id);-->
    cola_cualquiera.push(id);
  []()nieto_color[*]?devolver_lapiz(lapiz)-->
    if(!empty(cola_colores))-->
      nieto_color!response_color[cola_colores.pop()](lapiz);
    [](!empty(cola_cualquiera))-->
      nieto_cualquiera!response_cualquiera[cola_cualquiera.pop()](lapiz);
    []()-->
      color++;
    end if
  []()nieto_negro[*]?devolver_lapiz(lapiz)-->
    if(!empty(cola_negros))-->
      nieto_negro!response_negro[cola_negros.pop()](lapiz);
    [](!empty(cola_cualquiera))-->
      nieto_cualquiera!response_cualquiera[cola_cualquiera.pop()](lapiz);
    []()-->
      negro++;
    end if
  end if
}

process nieto_color[id_color=1..C](){
  char color;
  abuela!request_color();
  abuela?response_color(color);
  //Pintar
  abuela!devolver_lapiz(color);
}

process nieto_negro[id_negro=1..N](){
  char negro;
  abuela!request_negro();
  abuela?response_negro(negro);
  //Pintar
  abuela!devolver_lapiz(negro);
}

process nieto_cualquiera(id_cualquiera=1..A){
  char lapiz;
  abuela!request_lapiz();
  abuela?response_lapiz(lapiz);
  //Pintar
  abuela!devolver_lapiz(lapiz);
}
```

2.
```c++
process abuela(){
  int color = 10, negro = 15, id; queue cola_colores, cola_negros, cola_cualquiera; char lapiz;
  
  if(color > 0)nieto_color[*]?request_color(id);-->
    color--;
    nieto_color!response_color[id]('C');
  [](negro > 0)nieto_negro[*]?request_negro(id);-->
    negro--;
    nieto_negro!response_negro[id]('N');
  []((color > 0) && (empty(cola_colores)))nieto_cualquiera[*]?request_lapiz(id);-->
    color--;
    nieto_lapiz!response_lapiz[id]('C');
  []((negro > 0) && (empty(cola_negros)))nieto_cualquiera[*]?request_lapiz(id);-->
    negro--;
    nieto_lapiz!response_lapiz[id]('N');
  [](color == 0)nieto_color[*]?request_color(id);-->
    cola_colores.push(id);
  [](negro == 0)nieto_negro[*]?request_negro(id);-->
    cola_negros.push(id);
  []((negro == 0)||(color == 0))nieto_cualquiera[*]?request_cualquiera(id);-->
    cola_cualquiera.push(id);
  []()nieto_color[*]?devolver_lapiz(lapiz)-->
    if(!empty(cola_colores))-->
      nieto_color!response_color[cola_colores.pop()](lapiz);
    [](!empty(cola_cualquiera) && empty(cola_colores))-->
      nieto_cualquiera!response_cualquiera[cola_cualquiera.pop()](lapiz);
    []()-->
      color++;
    end if
  []()nieto_negro[*]?devolver_lapiz(lapiz)-->
    if(!empty(cola_negros))-->
      nieto_negro!response_negro[cola_negros.pop()](lapiz);
    [](!empty(cola_cualquiera) && empty(cola_negros))-->
      nieto_cualquiera!response_cualquiera[cola_cualquiera.pop()](lapiz);
    []()-->
      negro++;
    end if
  end if
}

process nieto_color[id_color=1..C](){
  char color;
  abuela!request_color();
  abuela?response_color(color);
  //Pintar
  abuela!devolver_lapiz(color);
}

process nieto_negro[id_negro=1..N](){
  char negro;
  abuela!request_negro();
  abuela?response_negro(negro);
  //Pintar
  abuela!devolver_lapiz(negro);
}

process nieto_cualquiera(id_cualquiera=1..A){
  char lapiz;
  abuela!request_lapiz();
  abuela?response_lapiz(lapiz);
  //Pintar
  abuela!devolver_lapiz(lapiz);
}
```
