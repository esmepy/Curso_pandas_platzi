## 18 Cómo lidiar con datos faltantes

|[Mi Notebook de clase](My_notebooks/18_datos_faltantes.ipynb)  |  No hay Notebook del profe |
|---------| ----:|

---


El objeto *nan* de **Numpy** interactúa con cálculos numéricos y siempre devolverá *nan*; el objeto de **pandas** NA interactúa además con texto y otro tipo de variables como las tipo booleanos.

se generan
```python
np.nan
ps.NA
```
algunas variables no definidas son:

    - None,
    -  NaN, 
    - -INF, 
    - INF 
    - y NA 

La opción: `pd.options.mode.use_inf_as_na = True` trata a todas ellas como NA, por default es *False*, en cuyo caso None y NaN son variables *null*, y las inf **no son NA**.

las funciones `.isnull()` y `.isna()` devuelven el mismo resultado, son equivalentes; detectan valores nulos de tipo: **Nan, None, NA,** no consideran string vacíos o objetos **`np.inf`** a no ser que `pd.options.mode.use_inf_as_na` sea *True*.
También estan `notnull()` y `notna()`.

df.isnull().sum().sum() hará un conteo de todos los valores nulos en el Dataframe.

`.dropna()` elimina filas que contengan valores nulos, podemos especificar como parámetro `axis=1` para eliminar columnas que contengan valores nulos.

`.fillna()` rellena los valores nulos, acepta como parámetro un valor o un dictionario o una serie e incluso un Dataframe. con `method= 'ffill'` llenará los espacios vacíos con el valor anterior, su opuesto : `bfill`. Con axis =1 lo hará a lo largo de la fila, por default lo hace a lo largo de la columna.


```python
df.fillna(method="bfill",axis=1)
```
para llenar con una serie, cada valor de la serie tomará el índice correspondiente
```python
fill = pd.Series([100, 101, 102])
df['d'].fillna(fill) 
```

- `df.fillna(df.mean())` aplicará la media a lo largo de cada fila
- `df.fillna(df.median())` aplicará la mediana (según  es un mejor estimador)

**Interpolación**

`df.interpolate()` llenará los valores NaN  con una inetrpolación. **no lo hará sobre valores NA**. para ello podemos convertirlos primero a valores NaN

```python
df2=df2.fillna(np.nan)
df2.interpolate()
```

por default hace interpolación lineal a lo largo de las columnas.

[Ir a mi notebook](My_notebooks/18_datos_faltantes.ipynb)