Ejercicio 6
======
Suponga una comisión con 50 alumnos. Cuando los alumnos llegan forman una fila, una vez que están los 50 en la fila el jefe de trabajos prácticos les entrega el número de grupo(número aleatorio del 1 al 25) de tal manera que dos alumnos tendrán el mismo número de grupo (suponga que el jefe posee una función dar_numero() que devuelve en forma aleatoria un número del 1 al 25, el jefe de trabajos prácticos no guarda el número que le asigna a cada alumno). Cuando un alumno ha recibido su número de grupo comienza a realizar la práctica. Al terminar de trabajar, el alumno le avisa al jefe de trabajos prácticos y espera la nota. El jefe de trabajos prácticos, cuando han llegado los dos alumnos de un grupo les devuelve a ambos la nota del GRUPO (el primer grupo en terminar tendrá como nota 25, el segundo 24, y así sucesivamente hasta el último que tendrá nota 1).

Solución
------
```c
process alumno[i=1..50](){
  int nro_grupo;
  JTP.await_partner(nro_grupo);
  //Hacer practica
  JTP.corregir(nro_grupo);
}

Monitor JTP(){
  cond cola;
  cond cant_miembros[25];
  int grupos[25] = ([25], 0);
  int notas[25];
  int nota = 25;
  int cant = 0;

  procedure await_partner(var int nro_grupo){
    cant++;
    if(cant < 50){
      wait(cola)
    }
    nro_grupo = dar_numero();
    signal(cola);
  }
  
  procedure corregir(int nro_grupo){
    grupos[nro_grupo]++;
    if(grupos[nro_grupo] == 2){
      notas[nro_grupo] = nota;
      nota--;
      signal(cant_miembros[nro_grupo]);
    }else{
      wait(cant_miembros[nro_grupo]);
    }
  }
}
```
