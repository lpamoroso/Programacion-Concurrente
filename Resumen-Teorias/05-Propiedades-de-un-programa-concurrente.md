# Propiedades de un programa concurrente

1. ¿Qué es un propiedad de programa?

Es un atributo que es verdadero en cualquier posible historia del programa y por lo tanto de todas la ejecuciones del programa.

2. ¿Qué propiedades existen?

Hay tres propiedades:
1. Seguridad.
2. Vida.

La propiedad de ***seguridad*** garantiza que el programa nunca entra en un estado malo(por ejemplo, valores indeseables en alguna variable). Una **falla de seguridad** indica que algo anda mal, es decir, la exclusión mutua por ejemplo. El estado malo, en tal caso, sería uno en el cual las acciones en las regiones críticas, en distintos procesos, fueran ambas elegibles para su ejecución.  
La propiedad de ***vida*** garantiza, por su parte, que el programa, eventualmente, entra en un estado bueno. Una ***falla de vida*** indica que el programa se ha detenido y no progresa. La eventual entrada es un ejemplo de ello: el estado bueno para cada proceso es uno en el cual su sección crítica es elegible.