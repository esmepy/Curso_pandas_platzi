## 21 Aggregation y Group by


|Mi Notebook de clase  |  [Notebook del profe](/Notebooks/21_groupby_pivot.ipynb) |
|:---------| ----:|


# . 


Bien importante lo que menciona juan. Es un poco de teoría más avanzada, pero en estadística y la teoría de estimadores para una población, la media es sensible a la presencia de “atipicos” o outliers. Generalmente se buscan otro tipo de estimadores más robustos, al igual que métricas como la de Cook para determinar outliers. Es un tema que merece la pena revisarse a profundidad.

Si, la media aritmética es un estimador bastante sesgado, muy útil para tener cierta noción en ciertos análisis pero está sesgado.

¿Cómo podemos saber que está sesgado?

Supongamos que tenemos 2 amigos, 1 amigo no tiene carro, el otro tiene 2 carros, entonces usando la media aritmética podríamos concluir que nuestro amigos en promedio tienen un carro.

    ¿Eso es verdad? No!

Por esa razón se dice que la media aritmética es un estimador sesgado.

Una solución rápida que usó David es usar la mediana, pero como dice Johan hay estimadores más robustos que para abordarlos dan para uno o varios cursos 😃



Un tema que vale la pena tener en cuenta es que la función value_counts por defecto ignora los valores ausentes, para tomarlos en cuenta se puede añadir dropna=False
