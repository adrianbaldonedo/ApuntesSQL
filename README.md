# Apuntes SQL
## Indice
1. [Estructura](#Estructura)
1. [Seleccionar una tabla](#selectTabla)
1. [SELECT DISTINC](#selectDistinc)
1. [WHERE](#where)
1. [IN](#in)
1. [BETWEEN](#between)
1. [LIKE](#like)
    - [CONCAT](#concat)
    - [REPLACE](#replace)
1. [ALIAS](#alias)
1. [Operaciones con SELECT](#selectOperations)
1. [SubLenguajes SQL](#subsql)


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
Se utiliza un guión bajo como sustituto de un carácter y solo de un carácter
Con este patrón en una tabla con paises nos saldria los paises que como segundo caracter tienen una t
```sql
WHERE name LIKE '_t%'
```
En este patrón seguimos utilizando el guión bajo , en este caso dos veces para buscar algo que este dos letras separado
Con este patrón en una tabla con paises nos saldrían los paises con dos letras cualquiera en medio de dos "o"
```sql
WHERE name LIKE '%o__o%'
```
Seguimos utilizando guión bajo puesto que tambien se utiliza para sacar resultado con x numero de carácteres
Con este patron en una tabla de paises nos saldrían los paises con 5 letras y solo 5 letras
```sql
WHERE name LIKE '_____'
```
NOTA: si nos piden como minimo 4 letras utlizamos 4 guiones bajos y luego un signo de porcentaje

También se puede usar LIKE para comparaciones extrictas, como si estubiesemos usando el operador = 

En el siguiente patrón en una tabla de paises nos enseñaría los paises que su pais es su propia capital
```sql
SELECT capital
FROM world
WHERE name LIKE capital;
```

## Función CONCAT <a name="concat"></a>
Se usa la función **CONCAT** para juntar una busqueda y una cadena ( Matching + String ) cuando usamos la notación **LIKE**

Con este patrón en una tabla con paises nos va a mostrar los paises que su capital sea la concatenación del nombre del pais + cadena "city"
```sql
WHERE capital LIKE CONCAT(name, 'City')
```
También se usa para incluir
Con este patrón en una tabla de paises nos mostraría todos los paises que su capital fuera una concatenación de CUALQUIER PALABRA + NOMBRE PAIS + CUALQUIER PALABRA ( sin espacios )
```sql
WHERE capital LIKE CONCAT ('%', name, '%')
```
Por ultimo usamos la función **CONCAT** para concatenar una extension a un atributo dejando un espacio entre ellos

Con este patrón en un tabla de paises nos mostraría todos los paises que su capital sea el nombre del pais + (espacio en blanco) + 
String

# **PREGUNTAR AL PROFESOR SI ESTO ESTA BIEN**
```sql
WHERE capital LIKE CONCAT(name, '_%')
```

## Función REPLACE <a name="replace"></a>
La función **REPLACE** remplaza todas las ocurrencias de una subcadena dentro de una cadena con una nueva cadena
Con esta función nos mostraria el resultado "SQL MuMorial"
```sql
SELECT REPLACE ('SQL Tutorial', 'T', 'M')
```

# ALIAS <a name="alias"></a>
Se utiliza los Alias cuando queremos dar a una tabla o a una columna en una tabla un nombre temporal
Los Alias se utilizan generalmete para hacer que los nombres de las columnas sean mas legibles
Para dar un Alias se utiliza la notación **AS**

Alias para columnas
```sql
SELECT nombre_Columnas AS alias_Columnas
FROM nombre_Tabla
```

Alias para tablas
```sql
SELECT nombre_columnas
FROM nombre_Tabla AS alias_Tabla
```

# Operaciones con SELECT <a name="selectOperations"></a>
Dentro del **SELECT** se pueden realizar operaciones, si una tabla nos proporciona ciertos valores y queremos hacer calculos con ellos podemos realizarlo de forma sencilla

Tenemos una tabla con datos de paises y nos proporcionan la poblacion de un pais y el producto interior bruto podemos calcular el producto interior bruto per capita
```sql
SELECT name, (GDP / population ) AS 'GDP per Capita'
```

Con la misma tabla podemos también calcularla poblacion en millones si dividimos la poblacion entre 1000000
```sql
SELECT name (population / 1000000 ) As population
```





# SubLenguajes SQL <a name="subsql"></a>

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
SCL ( SESION CONTROL LANGUAGUE) MANEJAR DINAMICAMENTE PROPIEDADES DE UNA SESION DE USUARIO
ALTER SESSION
