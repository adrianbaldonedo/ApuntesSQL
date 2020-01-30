** METER PARA LOS SIGUIENTES APUNTES
DDL (DATA DEFINITION LANGUAGE)
CREATE, ALTER, DROP
DML (DATA MANIPULATION LANGUAGE)
INSERT, UPDATE, DELETE
DQL(DATA QUARY LANGUAGE)
SELECT
TCL( TRANSACCION CONTROL LANGUAGE) TRANSACCION -> COMPOSICION DE DISTINTAS INSTRUCCIONES SQL
COMMIT, ROLLBACK, SAFE POINT
DCL (DATA CONTROL LANGUAGE) DAR PERMISOS
GRANT, REVOKE
CUANDO VEAMOS DE DQL ESTAMOS HABLANDO DE SELECT 
** METER PARA LOS SIGUIENTES APUNTES

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
Se utiliza la notación **BETWEEN** para seleccionar un rango de valores intermedios entre dos números, siempre usando la notación **AND** entre los dos valores del rango.

Ejemplo donde seleccionamos los valores entre 200000 y 250000 Ambos incluidos:
```sql
SELECT name, area
FROM world
WHERE area BETWEEN 200000 AND 250000
```

# Notación LIKE <a name="like"></a>
Se utiliza la notación **LIKE** para buscar un patrón / carácter / símbolo y que coincida.

En SQL usar Mayúsculas y minúsculas no es lo mismo debido a que el Matching de caracteres es extricto.

La diferencia entre **LIKE** Y **"="** a la hora de comparar es que **LIKE** busca una expresión regular y **"="** sirve para buscar una cadena tal cual y como esta escrita ( **LIKE 'The%'** vs **= 'The%'**

% - el signo de porcentaje representa cero, uno o mucho caracteres
_ - el signo del guión bajo representa a un solo caracter.

Estes dos signos se tambien se pueden usar combinados.

Ejemplos:
Nombres que contengan un carácter (%char%) :
```sql
WHERE name LIKE '%B%'
```
Empiecen por un carácter (char%)
```sql
WHERE name LIKE 'B%'
```
Finalicen con varios carácteres ( %string)
```sql
WHERE name LIKE '%land'
```
Que empiecen con un carácter y terminen con un conjunto de carácteres (char%charchar)
```sql
WHERE name LIKE 'C%ia'
```
Contiene un carácter doble  (%charchar%)
```sql
WHERE name LIKE '%ee%'
```
Varios caracteres repetidos pero separados (%char%char%char%char%)
Con este patrón en una tabla con paises nos saldria bahamas
```sql
WHERE name LIKE '%a%a%a%'
```
                    
















