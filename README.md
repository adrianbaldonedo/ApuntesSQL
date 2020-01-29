# Apuntes SQL
## Indice
1. [Estructura](#Estructura)
1. [Seleccionar una tabla](#selectTabla)
1. [SELECT DISTINC](#selectDistinc)
1. [WHERE](#where)
1. [IN](#in)
1. [BETWEEN](#between)
1. [LIKE](#like)



# Estructura <a name="Estructura"></a>
Depende de lo que necesitemos a la hora de hacer una consulta usaremos unas notaciones o otras , siempre partimos de una tabla con lo cual la notación *FROM* es la primera que SQL interpretará.
Estructura básica General:
1. Indicamos el o los campos a mostrar con *SELECT*
1. Inidcamos las tabla o tablas de las cuales vamos a sacar la información con *FROM*
1. Inidcamos en el caso de tener una condición lo indicamos a continuación con *WHERE*
1. En caso de necesitarlo le indicamos un Orden  con *ORDER BY*
1. En caso de necesitarlo indicamos un limite a mostrar con *LIMIT*
1. Al final de la consulta SQL cerramos el bloque de codigo con un **;**

Esta estructura no siempre es igual todo depende de nuestra consulta pero nos vale para tener una primera aproximación a una estructura simple de SQL.

# Seleccionamos una tabla Notaciones SELECT Y FROM <a name="selectTabla"></a>
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
FROM trabajadores;
```
Despues de la notación SELECT indicariamos que columnas queremos, en este caso con * seleccionamos todas
Despues de la notación FROM indicariamos el nombre de la tabla de la cual queremos consultar los datos


# Notación SELECT DISTINCT <a name="selectDistinc"></a>
Con esta notacion selccionamos solo las columnas con un valor distinto
Por ejemplo en una tabla con nombre de personas y direcciones en la columna pais es probable que el mismo pais salga mas veces, para evitar que se repita en nuestr consulta utilizamos la notación **SELECT DISTINCT**.

```sql
SELECT DISTICNCT Country
FROM trabajadores;
```

# Notación WHERE <a name="where"></a>
Utilizamos la notación **WHERE** para filtar resultados.
El **WHERE** se usa para extraer solo los resultados que cumplen cierta condición.

sintaxis:

```sql
SELECT columna1, columna2
FROM tabla
WHERE condicion;
```
Las condiciones pueden ser te varios tipos , inclus mas adelante dentro de la condición veremos que se pueden realizar consultas anidadas.

Lo operadores utilizados en el **WHERE** son:

| Operador | Descripción |
| -------- | ----------- |
| = |  igualdad extricta |
| > | Mayor que |
| < | Menor que |
| >= | Mayor o igual que |
| <= | Menor o igual que |
| <> | NO igual ( Diferente ) |
| BETWEEN | Comprendido entre un cierto rango  |
| LIKE | Comparación no extricta ( Con patrones) |
| IN | Para especificar multiple posibles valores para una columna |

# Notación IN <a name="in"></a>
Se utiliza la notación **IN** para los conjuntos, estes van entre paréntesis y en comillas simples y separados por comas.

Ejemplo donde seleccionamos el conjunto de estes tres paises
```sql
SELECT name, population
FROM world
WHERE name IN ('Sweden', 'Norway', 'Denmark');
```

# Notación BETWEEN <a name="between"></a>
# Notación LIKE <a name="like"></a>

