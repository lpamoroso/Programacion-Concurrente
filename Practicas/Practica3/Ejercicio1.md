Ejercicio 1
======
Se dispone de un puente por el cual puede pasar un solo auto a la vez. Un auto pide
permiso para pasar por el puente, cruza por el mismo y luego sigue su camino.
```c
Monitor Puente
  cond cola;
  int cant = 0;
  Procedure entrarPuente (int au)
    while ( cant > 0) wait (cola);
    cant = cant + 1;
  end;
  Procedure salirPuente (int au)
    cant = cant – 1;
    signal(cola);
  end;
End Monitor;

Process Auto [a:1..M]
  Puente. entrarPuente (a);
  “el auto cruza el puente”
  Puente. salirPuente(a);
End Process;
```
1. ¿El código funciona correctamente? Justifique su respuesta.
2. ¿Se podría simplificar el programa? En caso afirmativo, rescriba el código.
3. ¿La solución original respeta el orden de llegada de los vehículos? Si rescribió el
código en el punto 2, ¿Esa solución respeta el orden de llegada?
4. Modifique el código de la solución original para que permita pasar el puente hasta 5
autos a la vez (sin necesidad de que se respete el orden de llegada al puente).
5. Modifique el inciso 4 para que se respete el orden de llegada al puente.

Solución
-------
1. El codigo podria funcionar mejor: dado que se esta usando un while, el auto que le sigue al primero quedará en el bucle varias veces antes de poder salir.
2. Como dije, podría funcionar mejor: si se reemplaza el while por un if y se mantiene la condición de que solo puede haber un auto en el puente funciona perfectamente.
```c
Monitor Puente
  cond cola;
  int cant = 0;
  Procedure entrarPuente ()
    if ( cant > 0) wait (cola);
    cant = cant + 1;
  end;
  Procedure salirPuente ()
    cant = cant – 1;
    signal(cola);
  end;
End Monitor;

Process Auto [a:1..M]
  Puente. entrarPuente ();
  “el auto cruza el puente”
  Puente. salirPuente();
End Process;
```
3. La solución 1 podría no respetarlo: dado que se usa un while, podría despertarse un proceso dormido, pero si antes entra un proceso de afuera, le robaría el token. La solucion del punto 2 si lo respeta. 
4.
```c
Monitor Puente
  cond cola;
  int cant = 0;
  Procedure entrarPuente ()
    while ( cant >= 5) wait (cola);
    cant = cant + 1;
  end;
  Procedure salirPuente ()
    cant = cant – 1;
    signal(cola);
  end;
End Monitor;

Process Auto [a:1..M]
  Puente. entrarPuente ();
  “el auto cruza el puente”
  Puente. salirPuente();
End Process;
```
5.
```c
Monitor Puente
  cond cola[M];
  int cant = 0;
  queue autos;
  Procedure entrarPuente (int au)
    while ( cant >= 5){
      autos.push(au);
      wait(cola[au]);
    }
    cant = cant + 1;
  end;
  Procedure salirPuente ()
    int au;
    cant = cant – 1;
    autos.pop(au);
    signal(cola[au]);
  end;
End Monitor;

Process Auto [a:1..M]
  Puente. entrarPuente (a);
  “el auto cruza el puente”
  Puente. salirPuente();
End Process;
```
