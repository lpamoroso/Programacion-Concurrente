# Ejercicio 5

En una clínica existe un médico de guardia que recibe continuamente peticiones de atención de las E enfermeras que trabajan en su piso y de las P personas que llegan a la clínica ser atendidos.  
Cuando una persona necesita que la atiendan espera a lo sumo 5 minutos a que el médico lo haga, si pasado ese tiempo no lo hace, espera 10 minutos y vuelve a requerir la atención del médico. Si no es atendida tres veces, se enoja y se retira de la clínica.
Cuando una enfermera requiere la atención del médico, si este no lo atiende inmediatamente le hace una nota y se la deja en el consultorio para que esta resuelva su pedido en el momento que pueda (el pedido puede ser que el médico le firme algún papel). Cuando la petición ha sido recibida por el médico o la nota ha sido dejada en el escritorio, continúa trabajando y haciendo más peticiones.  
El médico atiende los pedidos dándoles prioridad a los pacientes que llegan para ser atendidos. Cuando atiende un pedido, recibe la solicitud y la procesa durante un cierto tiempo. Cuando está libre aprovecha a procesar las notas dejadas por las enfermeras.

```ada
TASK TYPE ENFERMERA;

TASK TYPE PACIENTE;

TASK MEDICO IS
  ENTRY ATENDER()
  ENTRY REQUERIR_ATENCION(NOTA: IN STRING);
END MEDICO;

TASK ESCRITORIO IS
  ENTRY DEJAR_NOTA(NOTA: IN STRING);
  ENTRY TOMAR_NOTA(NOTA: OUT STRING);
END ESCRITORIO;

TASK BODY PACIENTE IS
  ATENDIDO : BOOLEAN := FALSE;
  FOR 1 TO 3 LOOP
    SELECT
      MEDICO.ATENDER();
      ATENDIDO := TRUE;
    OR DELAY 5
      DELAY(10);
    END SELECT;
  END LOOP;
END PACIENTE;  

TASK BODY ENFERMERA IS
  NOTA : STRING;
  WHILE(TRUE)LOOP
    NOTA := GENERAR_PETICION();
    SELECT
      MEDICO.REQUERIR_ATENCION(NOTA);
    ELSE
      ESCRITORIO.DEJAR_NOTA(NOTA);
    END SELECT;
  END LOOP;
END ENFERMERA;

TASK ESCRITORIO IS
  QUEUE COLA;
  SELECT
    ACCEPT DEJAR_NOTA(NOTA: IN STRING) DO
      COLA.PUSH(NOTA);
    END DEJAR_NOTA;
  OR
    WHEN (!COLA.EMPTY()) ACCEPT TOMAR_NOTA(NOTA: OUT STRING) DO
      NOTA := COLA.POP();
    END TOMAR_NOTA;
  END SELECT;
END ESCRITORIO;

TASK MEDICO IS
  NOTA : STRING;
  SELECT
    ACCEPT ATENDER();
  OR
    WHEN (ATENDER'COUNT == 0) --> ACCEPT REQUERIR_ATENCION();
  ELSE
    SELECT
      ESCRITORIO.TOMAR_NOTA()
    ELSE
    END SELECT
  END SELECT;
END MEDICO;
```
