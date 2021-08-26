## 21 Aggregation y Group by


|[Mi Notebook de clase](My_notebooks/21_aggregation_group_by.ipynb)  |  [Notebook del profe](/Notebooks/21_groupby_pivot.ipynb) |
|---------| ----:|




### Notas de compañeros. 


Es un poco de teoría más avanzada, pero en estadística y la teoría de estimadores para una población, la media es sensible a la presencia de “atipicos” o outliers. Generalmente se buscan otro tipo de estimadores más robustos, al igual que métricas como la de Cook para determinar outliers. Es un tema que merece la pena revisarse a profundidad.

Si, la media aritmética es un estimador bastante sesgado, muy útil para tener cierta noción en ciertos análisis pero está sesgado.


Un tema que vale la pena tener en cuenta es que la función `value_counts()` por defecto ignora los valores ausentes, para tomarlos en cuenta se puede añadir `dropna=False`
