# Lo que tengo que preguntar:

- Ventajas de la concurrencia relacionadas al hardware.
- Como podria comportarse un proceso: dificultades.
- Speedup y eficiencia: necesito una mejor definicion.
- El fairness cuenta como propiedad concurrente??
- *A priori*, este algoritmo tiene un problema potencial que es que los valores de **próximo** y **turno** son ilimitados: esto se soluciona reseteandolos a un valor chico en cada corrida(1, por ejemplo). //NO ENTIENDO POR QUÉ DEBERÍA SER ASÍ. ESTO ESTA PLANTEADO DESDE UN PUNTO DE VISTA EN EL QUE SE DAN SUCESIVAS CORRIDAS EN LAS QUE ES NECESARIO REINICIAR??
- Algoritmo Iterativo: computan sucesivas mejores aproximaciones a una respuesta, y terminan al encontrarla o al converger//NO ENTIENDO MUY BIEN ESTO. POR QUÉ UN ALGORITMO ITERATIVO RESULTA SER INEFICIENTE??
- NECESITO ENTENDER POR QUÉ LA SINCRONIZACIÓN BARRIER MEJORA LOS ALGORITMOS ITERATIVOS. LA SINCRONIZACIÓN BARRIER ES MEJOR?? LA FORMA EN QUE SE PROPONEN LOS ALGORITMOS(DESDE EL BUSY WAITING HASTA EL BAKERY) SON DE MEJOR HASTA PEOR?? PODRIA USAR UNA APROXIMACIÓN HISTÓRICA?(SI SURGIERON DE ESA FORMA)?
- SINCRONIZACION BARRIER ES HACER LOS MISMO QUE ALGORITMOS ITERATIVOS O ES HACERLO MEJOR??
- SI EN SINCRONIZACION BARRIER NO EXISTE FETCH & ADD, SE CAE EN LA MISMA SITUACION QUE EN EL ALGORITMO TICKET POR LO CUAL ES NECESARIO IMPLEMENTAR ESE FETCH & ADD CON UNA SECCIÓN CRÍTICA.
- Los problemas que este algoritmo tiene, *a priori* son los mismos que en el algoritmo *ticket* ya que, dado que en cada corrida el contador debe ser reseteado a 0 puede haber contención de memoria o incoherencias en la cache. //CONSULTAR
- Los problemas que este algoritmo tiene, *a priori* son los mismos que en el algoritmo *ticket* ya que, dado que en cada corrida el contador debe ser reseteado a 0 puede haber contención de memoria o incoherencias en la cache. //EL ALGORITMO TICKET TAMBIEN TIENE INCONSISTENCIAS EN LA CACHE Y PROBLEMAS EN LA CONTENCION DE MEMORIA???
- 1. Requerir un proceso(y, por ende, un procesador) extra. //CONSULTAR SI ESTA BIEN
- A LO ULTIMO HABLA DE DEFECTOS DE BUSY WAITING... EL BUSY WAITING ES INHERENTE A TODOS LOS ALGORITMOS??
- Semaforo binario dividido es un especie de butterfly barrier?? //NECESITO AYUDA PARA EXPLICARLO BIEN.
- No es un TAD dado que es compartido por procesos que ejecutan concurrentemente //ESTA BIEN ACERCA DE LOS MONITORES??
- PREGUNTAR SI ENTRA MPI(BIBLIOTECAS PARA MANEJO DE PM) Y CSP(LENGUAJE PARA PMS).
- NO ENTIENDO ESTO: Por sí mismo, RPC es solo un mecanismo de comunicación.