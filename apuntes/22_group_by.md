## 22 group by: extraer valor con variable categóricas

|[Mi Notebook de clase](My_notebooks/22_group_by.ipynb)  |  [Notebook del profe](/Notebooks/21_groupby_pivot.ipynb) |
|---------| ----:|
---

Podemos crear categorías para variables numéricas, sin importar si son variables continuas, podemos hacerlo agrupando por intervalos, para ello usamos la función `pd.cut`, con _`bins`_ especificamos el número de categorías o podemos espcificar una lista de valores para definir los intervalos.

ejemplo:

```python
pd.cut(df['total_bill'], bins=3)
# con .value_counts() podemos ver el conteo para cada intervalo
#agregamos la columna nueva
df['bin_total'] = pd.cut(df['total_bill'],bins = [3,18,35,60])

```

si con ello creamos un columna de categorías, podemos hacer el conteo. con `.count()` o con `.sum()`

Para el caso de variables que ya están definidas como categorías podemos ya sea contar o usar una columna extra de **unos**, que nos ayude a saber la cantidad de casos de _x_ categoría 


además para ver los datos como porcentaje por categoría en cada grupo, podemos usar `.apply` y una función lambda que opere sobre el conteo. En el siguiente ejemplo, se agregó una columna de **unos** a todo el dataframe y con ello se realizó el conteo, la suma y el porcentaje.


```python
df.groupby(['time','bin_total'])[['ones']].count().groupby(level=0).apply(

lambda x:
x / x.sum() * 100

)
```

En este ejemplo se agrupa por `time` seguido de categorías, y muestra el porcentaje en cada gurpo `time`

---

### Notas de compañeros


Se puede crear categorías a nivel de cuantiles utilizando pd.qcut, por ejemplo para deciles:

```python
pd.qcut(df["total_bill"], q = 10)
```

Resumen sobre groupby y aggregate 😃

**groupby:** para aplicar una función a un conjunto de datos. Se puede usar de varias formas

- Forma simple

```python
df.groupby('column1').function
```

Donde function puede ser: mean(), median(), etc. En este sentido, Pandas tomará a ‘column1’ como índice y al resto del dataframe le aplicará la función definida.

- Con funciones lambdas o auxiliares

```python
df.groupby('column1')[['column2','column3']].apply(some_function)
```

Donde some_function pueden ser funciones lambdas u otras que hayamos definido. En esta operación, Pandas tomará a ‘column1’ como índice y las columnas serán el resultado de aplicar some_function sobre ‘column2’ y ‘column3’.

Nota: también pueden tomarse varias columnas como índices escribiendolas en una lista.

- aggregate (o agg): para aplicar más de una función. La sintaxis de esta función sigue la misma lógica que la de groupby, solo que se añade ‘agg’ (o ‘aggregate’) al final de la sentencia con una lista de funciones a aplicar. Ejemplos:

```python
df.groupby('column1').agg([np.mean,np.max])

df.groupby(['column1','column2'])[['column3','column4']].aggregate([np.mean,np.max])
```

De igual manera, Pandas realizará lo siguiente:

- tomará como índice las columnas que esten entre paréntesis.
- tomará como columnas el resultado de aplicar las funciones especificadas en ‘aggregate’ a la lista de columnas entre corchetes.
