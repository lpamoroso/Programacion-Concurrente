# Ejercicio 7(Corregido)

Hay una empresa de servicios que tiene un Recepcionista, N Clientes que hacen reclamos y E Empleados que los resuelven. Cada vez que un cliente debe hacer un reclamo, espera a que el Recepcionista lo atienda y le dé un Número de Reclamo, y luego continúa trabajando. Los empleados cuando están libres le piden un reclamo para resolver al Recepcionista y lo resuelven. El Recepcionista recibe los reclamos de los clientes y les entrega el Número de Reclamo, o bien atiende los pedidos de más trabajo de los Empleados (cuando hay reclamos sin resolver le entrega uno al empleado para que trabaje); siempre le debe dar prioridad a los pedidos de los Empleados.  
***Nota**:* los clientes, empleados y recepcionista trabajan infinitamente.

```ada
TASK TYPE CLIENTE;

TASK RECEPCIONISTA IS
  ENTRY ATENDER_CLIENTE(NUM_RECLAMO: OUT INTEGER; RECLAMO: IN STRING);
  ENTRY PEDIR_RECLAMO(RECLAMO: OUT STRING);
END RECEPCIONISTA;

TASK TYPE EMPLEADO;

TASK BODY CLIENTE IS
  RECLAMO: STRING;
  NUM_RECLAMO:INTEGER;
  WHILE(TRUE)LOOP
    RECLAMO := GENERAR_RECLAMO();
    RECEPCIONISTA.ATENDER_CLIENTE(NUM_RECLAMO, RECLAMO);
  END LOOP;
END CLIENTE;

TASK BODY EMPLEADO IS
  WHILE(TRUE)LOOP
    RECEPCIONISTA.PEDIR_RECLAMO(RECLAMO);
    RESOLVER_RECLAMO(RECLAMO);
  END LOOP;
END EMPLEADO;

TASK BODY RECEPCIONISTA IS
  CURRENT: INTEGER := 0;
  COLA: QUEUE;
  WHILE(TRUE)LOOP
    SELECT
      WHEN(PEDIR_RECLAMO'COUNT == 0) => ACCEPT ATENDER_CLIENTE(NUM_RECLAMO: OUT INTEGER; RECLAMO: IN STRING) IS
        NUM_RECLAMO := CUR;
        COLA.PUSH(RECLAMO);
      END ATENDER_CLIENTE;
      CUR := CUR + 1;
    OR
      WHEN (!COLA.EMPTY()) => ACCEPT PEDIR_RECLAMO(RECLAMO: OUT STRING) IS
        RECLAMO := COLA.POP();
      END PEDIR_RECLAMO;
    END SELECT;
  END LOOP;
END RECEPCIONISTA;

VAR
  CLIENTES = ARRAY(1..A) OF CLIENTE;
  EMPLEADOS = ARRAY(1..B) OF EMPLEADO;
```
