# Fairness

1. ¿Qué es el fairness?

La característica de ***fairness*** garantiza que todos los procesos tengan chance de avanzar, sin importar lo que hagan los demás. Partiendo de la base de que una acción atómica es ***elegible*** si es la próxima acción atómica en el proceso que será ejecutada; que, de haber varios procesos, hay varias acciones atómicas elegibles; y que una ***política de scheduling*** determina cuál será la próxima acción elegible, entonces hay cuatro escenarios de *fairness* que pueden darse:
1. *Fairness* incondicional: si toda acción atómica incondicional que es elegible eventualmente es ejecutada.
2. *Fairness* debil: si es incondicionalmente fair y toda acción atómica condicional que se vuelve elegible eventualmente es ejecutada, asumiendo que su condicion se vuelve true y permanece true hasta que es vista por el proceso que ejecuta la acción atómica condicional.
3. *Fairness* fuerte: si es incondicionalmente fair y toda acción atómica condicional que se vuelve elegible eventualmente es ejecutada pues su guarda se convierte en true con infinita frecuencia.

2. ¿Es fairness una propiedad de un programa concurrente?

No, es más bien una cualidad deseable. Lo importante a tener en cuenta es que los procesos puedan continuar siempre.

3. ¿En qué se relaciona el fairness con las políticas de schedulling?

Partiendo de que el fairness es el equilibrio en el acceso a recursos compartidos por todos los procesos, y que las políticas de schedulling determinan cúal es la sentencia a ejecutar, la relación está en que el avance de un proceso está dado por la elección de la próxima sentencia atómica que se debe ejecutar, por lo que algunas políticas pueden resultar más o menos fair dependiendo de qué sentencia atómica se ejecuta luego.