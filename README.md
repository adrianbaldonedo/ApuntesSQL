# Apuntes SQL
## Indice
1. [Estructura](#Estructura)
1. [Seleccionar una tabla](#selectTabla)



# Estructura <a name="Estructura"></a>
Depende de lo que necesitemos a la hora de hacer una consulta usaremos unas notaciones o otras , siempre partimos de una tabla con lo cual la notación *FROM* es la primera que SQL interpretará.
Estructura básica General:
1. Indicamos el o los campos a mostrar con *SELECT*
1. Inidcamos las tabla o tablas de las cuales vamos a sacar la información con *FROM*
1. Inidcamos en el caso de tener una condición lo indicamos a continuación con *WHERE*
1. En caso de necesitarlo le indicamos un Orden  con *ORDER BY*
1. En caso de necesitarlo indicamos un limite a mostrar con *LIMIT*

Esta estructura no siempre es igual todo depende de nuestra consulta pero nos vale para tener una primera aproximación a una estructura simple de SQL.

# Seleccionamos una tabla <a name="selectTabla"></a>
Partimos de una tabla sencilla con nombres y apellidos 

TABLA **trabajadores**

| ID | Nombre | Apellidos |
| -- | ------ | --------- |
| 01 | Jesus  | Garcia Naveira|
| 02 | Antonio| Gonzalez Nuñez|
| 03 | Juan   | Rodriguez Castro|


Lo mas basico para hacer una consulta y seleccionar todos los campos de una tabla seria:
```sql
SELECT *
FROM trabajadores
```
Despues de la notación SELECT indicariamos que columnas queremos, en este caso con * seleccionamos todas
Despues de la notación FROM indicariamos el nombre de la tabla de la cual queremos consultar los datos
