# Ejercicio 6

Hay un sistema web para reservas de pasajes de micro donde existen N Clientes que solicitan un pasaje para un cierto destino; espera hasta que el servidor le indique el número de asiento reservado (-1 si no hubiese disponibles); y luego (si había asiento disponible) el cliente imprime su pasaje. El servidor atiende los pedidos de acuerdo al orden de llegada, cuando un cliente le solicita un pasaje a un cierto destino, busca un asiento disponible para ese destino y luego le indica a ese cliente el asiento reservado (o -1 si no hubiese ninguno disponible).

```ada
TASK TYPE CLIENTE;

TASK SERVIDOR IS
  ENTRY SOLICITAR_PASAJE(NUM: OUT INTEGER, DEST: IN STRING);
END SERVIDOR;

TASK BODY CLIENTE
  NUM: INTEGER;
  DEST: STRING;
  DEST := GENERAR_DESTINO();
  SERVIDOR.SOLICITAR_PASAJE(NUM, DEST);
  IF(NUM != -1) //IMPRIMIR PASAJE
END CLIENTE;

TASK BODY SERVIDOR
  WHILE(TRUE)LOOP
    ACCEPT SOLICITAR_PASAJE(NUM: OUT INTEGER, DEST: IN STRING) DO
      NUM := BUSCAR_PASAJE(DEST);
    END SOLICITAR_PASAJE;
  END LOOP;
END SERVIDOR;

VAR
  CLIENTES = ARRAY(1..A) OF CLIENTE;
```
