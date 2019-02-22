# Acciones Atómicas

1. ¿Qué tipo de acciones atómicas existen?

Hay dos tipos:

* De grano fino.
* De grano grueso.

Una acción atómica ***de grano fino*** es aquella que debe ser implementada por hardware.  
Sin embargo, dado que los programas concurrentes no suelen ser disjuntos, podemos caer en la ya debatida **interferencia** ya que una o varias variables podrían tornarse ***referencias críticas***, es decir, podrían ser modificadas por otro proceso. Esto es algo que no puede tolerarse por las inconsistencias que pudiere producir. Para evitar esta interferencia es que en un programa concurrente debe respetarse la propiedad ***a lo sumo una vez***(ASV).  
La propiedad ***a lo sumo una vez*** plantea que si tenemos una sentencia de asignación *x = e* entonces debe cumplirse una de estas proposiciones:
1. *e* contiene a lo sumo una referencia crítica y *x* no es referenciada por otro proceso.
2. *e* no contiene referencias críticas, en cuyo caso *x* puede ser leída por otro proceso.

En otras palabras, la propiedad ASV se cumple solo si la expresión *e* no contiene más de una referencia crítica. Si una sentencia cumple dicha propiedad entonces su ejecución ***parece*** atómica, pues la variable compartida será leída o escrita solo una vez.  
Sin embargo, hay expresiones que no es posible que repeten la propiedad ASV, por lo que debe asegurarse su ejecución atómica de algún modo. Es aquí en donde aparecen las acciones atómicas ***de grano grueso***, es decir, aquellas sentencias que se presentan como una secuencia de acciones atómicas de grano fino que aparecen como indivisibles. Si bien estas acciones tienen un alto poder expresivo, el costo de implementación puede(en ocasiones) ser muy alto.