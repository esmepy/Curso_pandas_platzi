## Tabla de contenido

[14 Múltiples índices](#14-mltiples-ndices)

[15 Cómo trabajar con variables tipo texto en Pandas](#15-cmo-trabajar-con-variables-tipo-texto-en-pandas)

[16 Concatenación de Data Frames: concat y append](#16-concatenacin-de-data-frames-concat-y-append)

---
## 14 Múltiples índices

Permiten estructurar los datos para un mejor análisis.

En clases anteriores se vió que para cambia una variable a tipo categórico se usa: 

```python
df_pob['year']=df_pob['year'].astype('category')
```

en esta clase vemos otro forma incluida en pandas

```python
df_pob['year'] = pd.Categorical(df_pob['year'].apply(str))
```
Realizando pruebas, no es necesario convertir el año a string pues al final será convetido a tipo categórico.

>Para formatear  numeros en notación científica:
>`pd.options.display.float_format='{:,.1f}'.format`
>
>otra forma:
> `pd.set_option('display.float_format', '{:,.1f}'.format)`

### creación de múltiples índices
En esta clase, del data frame tomaremos una pequeña fracción de datos, 2 países por ejemplo de los cuales se incluye información por año para cada uno

* filtramos
 ```python
filtro = df_pob['Country'].isin(['Ecuador', 'Colombia'])
df_sample = df_pob[filtro]
```
* luego con ellos creamos / especificamos 2 índices: por país y por año

```python
df_sample = df_sample.set_index(['Country', 'year']).sort_index() 
df_sample
```

|Country |  year |  Pop  |
|---------|---------|---------|
|Colombia  |  2015  |   47520667.0  |
|     |   2016  |48171392.0   |
|   |   2017  |48901066.0  |
|     |    2018 | 49648685.0 |
|Ecuador  |  2015  | 16212020.0  |
|    |  2016   |16491115.0   |
|    |  2017   | 16785361.0 |
|    |  2018   |17084357.0   |

### Selección de múltiples índices
Para seleccionar por índices podemos anidas la función loc, por ejemplo:

```python
df_sample.loc['Ecuador', :].loc[2018,:]
```

hay otra forma de hacerlo con `xs`, el cual también permite buscar

```python
df_sample.xs(['Ecuador', '2018'])
```

También podemos hacer la selección a un bajo nivel especificando el nivel de búsqueda:

```python
df_sample.xs('2018', level='year')
```
### Ordenando por índces

Al crear los múltiples índices podemos especificar cómo queremos que se ordenen:

```python
f_countries = df_pob.set_index(['Country', 'year']).sort_index(ascending=[True, True])

df_countries
```



### **Notas**

para saltar filas al leer un archivo:


```python
df_pob = pd.read_csv('poblacion.csv', skiprows=4)
df_pob
```
para organizar el dataset como la clase:

1. descarga el dataset y guardalo en drive
2. crea una copia y cambiale el nombre a ‘poblacion.csv’
3. monta el drive a google colab y lee el dataset, utilizo skiprows para saltar las 4 primeras filas que no son del dataset, sino obtendrias un error

```python
df_pob = pd.read_csv('poblacion.csv', skiprows=4)
df_pob
```
* obtener los nombres de las columnas para solo copiar y pegar al eliminarlas debido a que son bastantes, el ejercicio se hizo con los años 2015 al 2018, yo incluire 2019

```python
list(df_pob)
```
* Elimino las columnas que no se van a utilizar
```python
df_pob = df_pob.drop(columns=['Country Code',
 'Indicator Name',
 'Indicator Code',
 '2005',
 '2006',
 '2007',
 '2008',
 '2009',
 '2010',
 '2011',
 '2012',
 '2013',
 '2014', 'Unnamed: 64'])
df_pob
```

* Pivoteo las filas para convertirlas en columnas y ordeno la data primero por ‘year’ y luego por ‘Country’, y ya queda para trabajar con el mismo dataset de la clase

```python
df = pd.melt(df_pob, id_vars=['Country Name'], var_name='year', value_name='pob')
df = df.sort_values(by=['year','Country Name'])
df
```
Mas info de .melt  [aquí](https://towardsdatascience.com/reshape-pandas-dataframe-with-melt-in-python-tutorial-and-visualization-29ec1450bb02)

**Cosas que *aprendí* tratando de hacer esta clase**

La forma de cambiar valores a tipo categórico: 

* hay 2 formas de convertir a categoria con `.astype('category')` y con pandas `pd.Categorical`
* si la variable es numéricay la convertimos directamente a categoría al trabajar con ella se debe seguir tratando como numérica (sin comillas)
* si es la convertimos a tipo texto y luego a categórico, se debe tratar como texto (con comillas)
    
## 15 Cómo trabajar con variables tipo texto en Pandas

Para usar las funciones asociadas a texto usamos `str` en nuestro DataFrame, por ejemplo, si se quiere colocar el texto en minúscula, basta con escribir:

```python
df['names'].str.lower()
```

Para dividir el texto por espacios usamos split y definimos el carácter por el que queremos dividir, en este caso, un espacio vacío 

```python
df['names'].str.split(' ') 
```

* Puedes aplicar funciones de búsqueda, conteo `len`, reemplazo `replace` y expresiones regulares.
    * `df['names'].str.findall('ara')`
    * `df['names'].str.contains('or')`
* podemos contar el número de ocurrencias de un caracter en específico,
por ejemplo, cuántas veces aparece la letra 'a':
    * `df['names'].str.lower().str.count('a')`


Existen comandos más avanzados usando Regex, por ejemplo, si quiero extraer los caracteres numéricos:

```python
df['names'].str.extract('([0-9]+)', expand=False)
```


## 16 Concatenación de Data Frames: concat y append


### Concatenación en Numpy

>Primeramente para mostrar _n_ decimales de números flotantes de elementos de Numpy usamos:
```python
np.set_printoptions(precision=2)
```
En numpy podemos concatenar Arrays con el comando `np.concatenate([A1,A2])` , por default de concatenará hacia abajo, pero si definimos `axis=1` lo hará hacia la derecha como en el siguiente ejemplo
```python
#creamos 2 arreglos de 2x5 aleatorios
x1 = np.random.rand(2, 5)*100 
x2 = np.random.rand(2, 5)*-10 

np.concatenate([x1, x2], axis=1)
```
> ``np.random.rand(n)`` generará un array con n números aleatorios entre 0 y 1



### Concatenación con Pandas

Se pueden concatenar Series o Dataframes usando `pd.concat()`. por default se agregarán abajo, como filas nuevas


**Series**
```python
s1 = pd.Series(x1[0], index=['a','b','c','d','e'])
s2 = pd.Series(x2[0], index=['c', 'd', 'e', 'f', 'g'])

pd.concat([s1, s2], axis=1)
```
Con `axis = 1` se agregará como columna y además respetará los índices, de encontrarse los mismos índices en ambas series; si no, se agregarán los nuevos índices y en los demas campos aparecerá ***NaN***

**DataFrames**

ejemplo con Dataframes
```python
df1 = pd.DataFrame(np.random.rand(3, 2)*10, columns=['a', 'b'])
df2 = pd.DataFrame(np.random.rand(3, 2)*-10, columns=['a', 'b'], index=[2, 3, 4])
```
por default al usar `pd.concat` se agregarán como filas nuevas, pero si especificamos `axis=1` se gregarán como columnas, en este caso, aunque compraten nombres de columnas la concatenación crea un dataframe con 4 columnas y nombres repetidos

```python
pd.concat([df1, df2], axis=1)
```
Del resultado de arriba si solo queremos las filas que no tienen NaN, podemos usar

```python
pd.concat([df1, df2], axis=1, join='inner')

	a    	b	    a    	b
2	5.29	1.79	-8.47	-8.75
```
### Concatenación con append
* Podemos usar **append** para concatenar
`df1.append(df2)` lo que agregará el dataframe abajo, como filas.

* También para crear una transpuesta usar **`.T`**
y con un poco de imaginación hacer que la concatenación de el mismo recultado que con `.concat()`
```python
df1.T.append(df2.T).T
```

### **Notas**

Se pueden resetear los índices con 
```python
s1.reset_index(drop=True) #drop=False  te crea una columna con los índices anteriores
```

para que se despliegen solo 2 decimales de elementos  creados con Pandas en todo el notebook:
``` python
pd.options.display.float_format = '{:.2f}'.format
```

numpy tambien tiene la propiedad para mostrar _n_ decimales de números flotantes de elementos de Numpy usamos:
```python
np.set_printoptions(precision=2)
```
>A los arrays de numpy no tiene la propiedad append

Una funcionalidad extra de pandas que puede ser útil es pd.update().
Donde básicamente si tengo df1 y df2, al hacer `pdf1.update(df2)` me actualiza o cambia los valores de df1 tomando los de df2 , pero sólo de las columnas que tienen en común.

## 17 Merge de Dataframes

[Link al colab der ésta clase](https://colab.research.google.com/drive/1X_7pbPE9D-K5jz447BgrxD7y7bVUL5dL)


