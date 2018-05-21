# Ejercicio 8

En una empresa hay 5 controladores de temperatura y una Central. Cada controlador toma la temperatura del ambiente cada 10 minutos y se la envía a una central para que analice el dato y le indique que hacer. Cuando la central recibe una temperatura que es mayor de 40 grados, detiene a ese controlador durante 1 hora.

```ada
TASK TYPE CONTROLLER;

TASK CENTRAL IS
  ENTRY TEMPERATURE(TEMP: IN INTEGER; CODE: OUT INTEGER);
END CENTRAL;

TASK BODY CONTROLLER IS
  TEMP, CODE: INTEGER;
  WHILE(TRUE)LOOP
    TEMP := MEASURE_TEMP();
    CENTRAL.TEMPERATURE(TEMP, CODE);
    IF(CODE){
      DELAY(60*60);
    }
  END LOOP;
END CONTROLLER;

TASK BODY CENTRAL IS
  WHILE(TRUE)LOOP
    ACCEPT TEMPERATURE(TEMP: IN INTEGER; CODE: OUT INTEGER) DO
      IF(TEMP > 40){
        CODE := 0;
      }ELSE{
        CODE := -1;
      }
    END TEMPERATURE;
  END LOOP;
END CENTRAL;

VAR
  CONTROLLERS := ARRAY(1..5) OF CONTROLLER;
```
