# Propiedades de un programa concurrente

1. ¿Qué propiedades existen?

Hay tres propiedades:
1. Seguridad.
2. Vida.
3. *Fairness*.

La propiedad de ***seguridad*** garantiza estados concurrentes. Una **falla de seguridad** indica que algo anda mal, es decir, interferencias entre procesos, por ejemplo.  
La propiedad de ***vida*** garantiza que, eventualmente, una activdad progresa. Una ***falla de vida*** indica que el programa se ha detenido y no progresa. En estos casos, el problema suele ser que hay algo que no se está asegurando, ya sea que un mensaje llegare a destino o que un proceso eventualmente alcanzare su sección crítica.  
La propiedad de ***fairness*** garantiza que todos los procesos tengan chance de avanzar, sin importar lo que hagan los demás. Partiendo de la base de que una acción atómica es ***elegible*** si es la próxima acción atómica en el proceso que será ejecutada; que, de haber varios procesos, hay varias acciones atómicas elegibles; y que una ***política de scheduling*** determina cuál será la próxima acción elegible, entonces hay cuatro escenarios de *fairness* que pueden darse:
1. *Fairness* incondicional: si toda acción atómica incondicional que es elegible eventualmente es ejecutada.
2. *Fairness* debil: si es incondicionalmente fair y toda acción atómica condicional que se vuelve elegible eventualmente es ejecutada, asumiendo que su condicion se vuelve true y permanece true hasta que es vista por el proceso que ejecuta la acción atómica condicional.
3. *Fairness* fuerte: si es incondicionalmente fair y toda acción atómica condicional que se vuelve elegible eventualmente es ejecutada pues su guarda se convierte en true con infinita frecuencia.