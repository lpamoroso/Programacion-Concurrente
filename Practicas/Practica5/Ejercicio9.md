# Ejercicio 9

Una empresa de limpieza se encarga de recolectar residuos en una ciudad. Hay P personas que continuamente hacen reclamos para que pasen por su casa. La empresa tiene 2 camiones, que cuando alguno de ellos está libre lo envía a recolectar los residuos de la persona que más reclamos ha hecho.

```ada
TASK TYPE PERSONA;

TASK EMPRESA IS
  ENTRY RECLAMAR(RECLAMO: IN STRING);
  ENTRY TOMAR_RECLAMO(RECLAMO: OUT STRING);
END EMPRESA;

TASK TYPE CAMION;

TASK BODY PERSONA IS
  RECLAMO: STRING;
  WHILE(TRUE)LOOP
    RECLAMO := GENERAR_RECLAMO();
    EMPRESA.RECLAMAR(RECLAMO);
  END LOOP;
END PERSONA;

TASK BODY CAMION IS
  RECLAMO: STRING;
  WHILE(TRUE)LOOP
    EMPRESA.TOMAR_RECLAMO(RECLAMO);
    --RESOLVER RECLAMO
  END LOOP;
END CAMION;

TASK BODY EMPRESA
  COLA: QUEUE;
  WHILE(TRUE)LOOP
    SELECT
      ACCEPT RECLAMAR(RECLAMO: IN STRING) IS
        COLA.PUSH(RECLAMO);
      END RECLAMAR;
    OR
      WHEN(!COLA.EMPTY()) => ACCEPT TOMAR_RECLAMO(RECLAMO: OUT STRING) IS
        RECLAMO := COLA.POP();
      END TOMAR_RECLAMO;
    END SELECT;
  END LOOP;
END EMPRESA;

VAR
  PERSONAS := ARRAY(1..P) OF PERSONA;
  CAMIONES := ARRAY(1..2) OF CAMION;
```
