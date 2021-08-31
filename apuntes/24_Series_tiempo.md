## 24  Series de Tiempo

| [Mi Notebook de clase](My_notebooks/24_Series_tiempo.ipynb)  |  [Notebook del profe](/Notebooks/24_datetime.ipynb) |
|---------| ----:|

### 

en esta clase usamos una base de datos de covid de  Kaggle (tiene 8 columnas)

el procesos a seguir es:
- seleccionar las columnas con las que trabajaremos
- -aplicar el formato adecuado a las variables (por ejemplo tiempo)
- crear un Dataframe nuevo con los datos agrupados por fecha

Cuando tenemos un datafame con índices de fecha podemos restar o sumar entre esos datos (los índices se relacionan).

El dataframe que creamos tiene todos los casos de Covid desde el primer día de observación, 22 de Enero 2020. Con ello podemos obtener:
- El número de casos promedio por día
- el número de 'sobrevivientes' por día
- el incremento de casos por día , usando `.diff()`

Al aplicar la diferencia a un dataframe, la primera fila tendrá valores **nan*

