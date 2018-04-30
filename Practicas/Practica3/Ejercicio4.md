Ejercicio 4
======
En una casa viven una abuela y sus N nietos. La abuela compró caramelos que quiere convidar entre sus nietos. Inicialmente, la abuela deposita en una fuente X caramelos, luego cada nieto intenta comer caramelos de la siguiente manera: si la fuente tiene caramelos, el nieto agarra uno de ellos; en el caso de que la fuente esté vacía, entonces se le avisa a la abuela quien repone nuevamente X caramelos. Luego se debe permitir que el nieto que no pudo comer sea el primero en hacerlo, es decir, el primer nieto que puede comer nuevamente es el primero que encontró la fuente vacía.
<br>**Nota**: *siempre existen caramelos para reponer. Cada nieto tarda t minutos en comer un caramelo (t no es igual para cada nieto). Puede haber varios nietos comiendo al mismo tiempo.*

Solución
------

```c
process nieto[i=1..N](){
  while(true){
    Fuente.agarrar_caramelo(i);
  }
}

process abuela(){
  while(true){
    Fuente.reponer();
  }
}

Monitor Fuente(){
  cond abuela;
  cond cola[N];
  queue nietos;
  int cant_caramelos = 0;
  
  procedure agarrar_caramelo(int id){
    while(cant_caramelos == 0){
      signal(abuela);
      nietos.push(id);
      wait(cola);
    }
    cant_caramelos--;
    signal(cola[nietos.pop()]);
  }
  
  procedure reponer(){
    if(cant_caramelos > 0){
      wait(abuela);
    }
    cant_caramelos = X;
    signal(cola[nietos.pop()]);
  }
}
```
