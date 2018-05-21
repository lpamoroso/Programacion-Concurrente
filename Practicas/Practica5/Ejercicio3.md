# Ejercicio 3

Se debe modelar un buscador para contar la cantidad de veces que aparece un número dentro de un vector distribuido entre las N tareas contador. Además existe un administrador que decide el número que se desea buscar y se lo envía a los N contadores para que lo busquen en la parte del vector que poseen.

```ada
TASK ADMINISTRADOR IS
  ENTRY BUSCAR_NUMERO(NUM: OUT INTEGER);
  ENTRY RECIBIR_CANTIDAD(NUM: IN INTEGER);
END ADMINISTRADOR;

TASK TYPE BUSCADOR;

TASK BODY ADMINISTRADOR IS
  X, TOTAL : INTEGER;
  TOTAL := 0;
  X := GENERATE_NUMBER_TO_LOOK_FOR();
  FOR 1 TO N+N LOOP
    SELECT
      ACCEPT BUSCAR_NUMERO(NUM: OUT INTEGER) DO
        NUM = X;
      END BUSCAR_NUMERO;
    OR
      WHEN (RECIBIR_CANTIDAD'COUNT > 0) => ACCEPT RECIBIR_CANTIDAD(NUM: IN INTEGER) DO
        TOTAL:=TOTAL+NUM;
      END RECIBIR_CANTIDAD;
    END SELECT;
  END LOOP;
END ADMINISTRADOR;

TASK BODY BUSCADOR IS
  ARREGLO : ARRAY (1..N) OF INTEGER;
  CANTIDAD, NUM : INTEGER;
  CANTIDAD := 0;
  ADMINISTRADOR.BUSCAR_NUMERO(NUM);
  CANTIDAD := BUSCAR_NUMERO(ARREGLO, NUM);
  ADMINISTRADOR.RECIBIR_NUMERO(CANTIDAD);
END BUSCADOR

VAR
  BUSCADORES = ARRAY(1..B) OF BUSCADOR;
```
