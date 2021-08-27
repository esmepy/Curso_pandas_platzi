## 23  Tablas dinámicas con Pivot Table  

| [Mi Notebook de clase](My_notebooks/22_group_by.ipynb)  |  [Notebook del profe](/Notebooks/21_groupby_pivot.ipynb) |
|---------| ----:|

### 

___

Con groupby podemos generar un dataframe como el siguiente

<img src="../images/23_00.jpg" alt="drawing" align="center"/>

usando el siguiente código:
```python
df_gp = df.groupby(['sex','time'])[['total_bill']].mean().reset_index()
```

el cual tiene múltiples índices `sex` y `time` y muestra la media.

Una mejor forma de vizualizarlo es pasando el segundo grupo como columnas, para ello pusamos `.pivot_table()`, con los siguientes parámetros:
- `values` los valores que se mostrarán dentro de la tabla
- `index` el nombre de las filas
- `columns` el nombre de las columnas
- 
```python
df.pivot_table(values='total_bill', index='sex', columns='time')
```

El resultado es el siguiente:

<img src="../images/23_01.jpg" alt="drawing">

Por default `.pivot_table` obtiene la **media**, pero podríamos definir qué función aplicar `aggfunc`

```python
df.pivot_table(values='total_bill', index='sex', columns='time', aggfunc='median')
# o podemos usar np.median sin comillas
```

Al igual que con `aggregate` podemos pasar un diccionario o una lista de funciones

```python
df_pivot = df.pivot_table(values='total_bill', index='sex', columns='time', aggfunc=[np.median,'std'])
```
Parara deshacer las categorías usamos `.unstack()` , lo cual devuelve al parecer un objeto de tipo **serie** con múltiples índices, para convertirlo a dataframe aplicamos `.reset_index()`

```python
df_pivot.unstack().reset_index()
```

<img src="https://pandas-docs.github.io/pandas-docs-travis/_images/reshaping_pivot.png" alt="drawing" width="600"/>



