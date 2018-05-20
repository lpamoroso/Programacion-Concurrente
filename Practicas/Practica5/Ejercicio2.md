# Ejercicio 2

Se quiere modelar la cola de un banco que atiende un solo empleado, los clientes llegan y si esperan más de 10 minutos se retiran.

```ada
TASK TYPE CLIENTE;

TASK EMPLEADO IS
  ENTRY ATENDER;
END EMPLEADO;

TASK BODY CLIENTE IS
  SELECT
    EMPLEADO.ATENDER();
  OR DELAY 10
  END SELECT;
END CLIENTE;

TASK BODY EMPLEADO IS
  WHILE(TRUE)LOOP
    ACCEPT ATENDER() DO
      --ATENDER
    END ATENDER;
  END LOOP;
END EMPLEADO;
```
