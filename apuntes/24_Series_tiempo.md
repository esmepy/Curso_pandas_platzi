## 24  Series de Tiempo

| [Mi Notebook de clase](My_notebooks/24_Series_tiempo.ipynb)  |  [Notebook del profe](/Notebooks/24_datetime.ipynb) |
|---------| ----:|

----


en esta clase usamos una base de datos de covid de  Kaggle (tiene 8 columnas)

el procesos a seguir es:
- seleccionar las columnas con las que trabajaremos
- -aplicar el formato adecuado a las variables (por ejemplo tiempo)
- crear un Dataframe nuevo con los datos agrupados por fecha

Cuando tenemos un datafame con índices de fecha podemos restar o sumar entre esos datos (los índices se relacionan).

El dataframe que creamos tiene todos los casos de Covid desde el primer día de observación, 22 de Enero 2020. Con ello podemos obtener:

- el número de 'sobrevivientes' por día
- el incremento de casos por día , usando `.diff()`
- El número de casos nuevos promedio por día
```python
df_time.diff().mean()
```

Al aplicar la diferencia a un dataframe, la primera fila tendrá valores **nan*

Podemos completar esa fila usando un diccionario que contenga el primer registro del dataframe como sige:
- guardamos el Dataframe de diferencias
- creamos el diccionario que contenga el primer renglón
- rellenemos los espacios Nan con el diccionario

```python
dict_1=df_time.head(1).to_dict()
df_diff=df_diff.fillna(dict_1)
```
Una vez agrupados los datos podemos aplicar algunos métodos al dataframe como:
- `cumsum()` suma acumulada
- `resample()` para hacer muestreo podr rando de días 

```python
#suma de los datos de 7 dias
df_diff.resample('7D').sum()
#Cada domingo
df_diff.resample('W-Sun').sum()
```


**NOTAS**
para calcular las variaciones porcentuales
```python
df_time.pct_change()
```

`.resample()` Método de conveniencia para conversión de frecuencia y remuestreo de series de tiempo. método de dataframes de pandas

Algunas opciones de resample
Alias - Description
B - Business day
D - Calendar day
W - Weekly
M - Month end
Q - Quarter end
A - Year end
BA - Business year end
AS - Year start
H - Hourly frequency
T, min - Minutely frequency
S - Secondly frequency
L, ms - Millisecond frequency
U, us - Microsecond frequency
N, ns - Nanosecond frequency


## 25  Series de Tiempo: variables nulas

| [Mi Notebook de clase](My_notebooks/24_Series_tiempo.ipynb)  |  [Notebook del profe](/Notebooks/24_datetime.ipynb) |
|---------| ----:|

---



