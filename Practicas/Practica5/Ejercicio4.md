# Ejercicio 4

Se debe controlar el acceso a una base de datos. Existen A procesos de Tipo 1, B procesos de Tipo 2 y C procesos de Tipo 3 que trabajan indefinidamente de la siguiente manera:

* Proceso Tipo 1: intenta escribir, si no lo logro en 2 minutos, espera 5 minutos y vuelve a intentarlo.
* Proceso Tipo 2: intenta escribir, si no lo logra en 5 minutos, intenta leer, si no lo logra en 5 minutos vuelve a comenzar.
* Proceso Tipo 3: intenta leer, si no puede inmediatamente entonces espera hasta poder escribir.

Un proceso que quiera escribir podrá acceder si no hay ningún otro proceso en la base de datos, al acceder escribe y avisa que termino de escribir. Un proceso que quiera leer podrá acceder si no hay procesos que escriban, al acceder lee y avisa que termino de leer. Siempre se le debe dar prioridad al pedido de acceso para escribir sobre el pedido de acceso para leer.

```ada
TASK TYPE TIPO_1;

TASK TYPE TIPO_2;

TASK TYPE TIPO_3;

TASK DATABASE IS
  ENTRY LEER();
  ENTRY ESCRIBIR();
  ENTRY FIN_LEER();
  ENTRY FIN_ESCRIBIR();
END DATABASE;

TASK BODY TIPO_1 IS
  WHILE(TRUE)LOOP
    SELECT
      DATABASE.ESCRIBIR();
      DATABASE.FIN_ESCRIBIR();
    OR DELAY 2
      DELAY 5;
      DATABASE.ESCRIBIR();
      DATABASE.FIN_ESCRIBIR();
    END SELECT;
  END LOOP;
END TIPO_1;

TASK BODY TIPO_2 IS
  WHILE(TRUE)LOOP
    SELECT
      DATABASE.ESCRIBIR();
      DATABASE.FIN_ESCRIBIR();
    OR DELAY 5
      SELECT
        DATABASE.LEER();
        DATABASE.FIN_LEER();
      OR DELAY 5
      END SELECT;
    END SELECT;  
  END LOOP;
END TIPO_2;

TASK BODY TIPO_3 IS
  WHILE(TRUE)LOOP
    SELECT
      DATABASE.LEER();
      DATABASE.FIN_LEER();
    ELSE
      DATABASE.ESCRIBIR();
      DATABASE.FIN_ESCRIBIR();
    END SELECT;
  END LOOP;
END TIPO_3;

TASK BODY DATABASE IS
  CANT_LECTORES : INTEGER := 0;
  SELECT
    WHEN(ESCRIBIR'COUNT == 0) => ACCEPT LEER();
    CANT_LECTORES := CANT_LECTORES + 1;
  OR
    ACCEPT FIN_LEER();
    CANT_LECTORES := CANT_LECTORES - 1;
  OR
    WHEN(CANT_LECTORES == 0)ACCEPT ESCRIBIR();
      ACCEPT FIN_ESCRIBIR();
      FOR 1 TO LEER'COUNT LOOP
        ACCEPT LEER();
        CANT_LECTORES := CANT_LECTORES + 1;
      END LOOP;
    END ESCRIBIR;
  END SELECT;
END DATABASE;

VAR
  TIPOS_1 = ARRAY(1..A) OF TIPO_1;
  TIPOS_2 = ARRAY(1..B) OF TIPO_2;
  TIPOS_3 = ARRAY(1..C) OF TIPO_3;
```
