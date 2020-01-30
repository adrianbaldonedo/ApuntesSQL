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
1. [AND, OR y NOT](#and)
1. [OR y XOR (OR exclusivo)](#orxor)
1. [Redondeos con ROUND](#round)
1. [LENGTH](#length)
1. [LEFT](#left)
1. [NOT LIKE](#notLike)
1. [ANY Y ALL](#anyAll)
1. [SELECT Anidados](#selectAnidados)
1. [Funciones SUM y COUNT](#sumCount)
1. [HAVING](#having)
1. [GROUP BY](#groupby)
1. [ORDER BY](#orderby)
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

La diferencia entre **LIKE** Y **"="** a la hora de comparar es que **LIKE** busca una expresión regular y **"="** sirve para buscar una cadena tal cual y como esta escrita ( **LIKE 'The%'** vs **= 'The%'** )

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

Con la misma tabla podemos también calcular la poblacion en millones si dividimos la poblacion entre 1000000
```sql
SELECT name (population / 1000000 ) As population
```

# Operadores AND, OR y NOT <a name="and"></a>
La clausula **WHERE** puede combinarse con los operadores **AND, OR y NOT**

AND y OR se usa para filtrar resultaos basandose en mas de una condicion:
    - El operador AND muestra un resultado si todas las condiciones separadas por un AND son verdaderas
    - El operador OR muestra un resultado si alguna de las condiciones separadas por un OR es verdadera
El operador NOT muestra el resultado si la condición es NO VERDADERA.

sintaxis: 
```sql
SELECT columna1, columna2
FROM tabla
WHERE condicion1
OR condicion2
AND NOT condicion3;
```

# OR y XOR (OR exclusivo) <a name="orxor"></a>
Cuando necesitamos hacer una comparación en la que se indica "o bien compara con esto", "o bien compara con esto otro" en nuestra consulta utlizamos la notación **OR**

En esta consulta pedimos el nombre , la poblacion y el area donde la poblacion es mayor que 250000000 o o el area es mayor que 3000000
```sql
SELECT name, population, area
FROM world
WHERE population > 250000000
OR area > 3000000;
```
Utilizamos la notación XOR ( OR exclusivo ) cuando queremos buscar por una cosa o por la otra, pero no por las dos

Esto tambien se puede hacer de dos maneras:
```sql
Manera con XOR:
SELECT name, population , area
FROM world
WHERE area > 3000000
XOR population > 250000000;

Manera sin XOR:
SELECT name, population, area
FROM world
WHERE ( area > 3000000 OR population > 250000000)
AND NOT ( area > 3000000 AND population > 250000000);
```

# Redondeos con ROUND <a name="round"></a>
## ROUND CON DECIMALES
Usamos **ROUND** para redondear un resultado , el formato es ROUND ( DATO / NUMERO , cantidad de decimales)

En este caso mostramos poblacion entre un millon y enseñamos 2 decimales y tambien Producto interior butro entre un billon y mostramos 2 decimales
```sql
SELECT name, ROUND(population / 1000000, 2) AS population, ROUND(GDP / 1000000000, 2) AS GDP
```

## ROUND CON NUMEROS MUY GRANDES 
El número de decimales puede ser negativo, esto se redondeará al 10 más cercano (cuando p es -1) o 100 (cuando p es -2) o 1000 (cuando p es -3), etc.

En esta consulta redondeamos un trillón al 1000 mas cercano
```sql
SELECT name, ROUND(GDP / population, -3) As GDPperCapita
FROM world
WHERE GDP >= 1000000000000;
```

# LENGTH <a name="length"></a>
Con la función LENGTH podemos sacar el largo de algo

Lo que es muy bueno si queremos comparar longitudes

En esta consulta comparamos las longitudes de los nombres de los paises con las longitudes de los nombres de las capitales
```sql
SELECT name, capital
FROM world
WHERE LENGTH(name) LIKE LENGTH(capital);
```

# LEFT <a name="left"></a>
Utilizamos la función **LEFT** para aislar el primer carácter

Enseñamos los paises y las capitales de cuyos nombre de pais y de capital son diferentes pero la primera letra coinciden
```sql
SELECT name, capital
FROM world
WHERE LEFT(name, 1) = LEFT(capital,1)
AND name <> capital;
```

# NOT LIKE <a name="notLike"></a>
Se utiliza la notación **NOT LIKE** para exluir cierto carácter o frase de la consulta

Usamos **NOT LIKE** para buscar palabras que contengan todas las vocales pero NO tengan espacios en blanco
```sql
SELECT name
FROM world
WHERE name LIKE '%a%'
AND name LIKE '%e%'
AND name LIKE '%i%'
AND name LIKE '%o%'
AND name LIKE '%u%'
AND name NOT LIKE '% %';
```

# Operadores ANY Y ALL <a name="anyAll"></a>
El operador **ANY** y el operador **ALL** se usan en las clausulas WHERE o HAVING

El operador **ANY** devuelve verdadero si uno se los valores de la subconsulta cumple la condición

El operador **ALL** devuelve verdadero si todos los valores de la subconsulta cumplen la condición

Sintaxis:
```sql
ANY:
SELECT nombre_columna
FROM nombre_tabla
WHERE nombre_columna operator ANY ( SELECT nombre_columna
                                    FROM nombre_tabla 
                                    WHERE condition);

ALL:
SELECT nombre_columna
FROM nombre_tabla
WHERE nombre_columna operator ALL ( SELECT nombre_columna
                                    FROM nombre_tabla
                                    WHERE condition);
```

# SELECT Anidados <a name="selectAnidados"></a>
En **SQL** podemos realizar consultas con donde nuestra condición ( lo que va despues de la notación WHERE) sea otra consulta.

Un ejemplo básico seria cuando una consulta de un resultado mayor que otra consulta

Seleccionamos el nombre de los paises que su poblacion sea mayor que la consulta de la poblacion de rusia.
```sql
SELECT name
FROM world
WHERE population > ( SELECT population
                     FROM world
                     WHERE name = 'Rusia');                 
```

También se puede hacer consultas con varias consultas anidadas

ejemplo: Seleccionamos el pais y el continente de los paises que su continente sea argentia o australia
```sql
SELECT name, continent
FROM world
WHERE continent = ( SELECT continent
                    FROM world
                    WHERE name = 'Argentina')
OR continent = ( SELECT continent
                 FROM world
                 WHERE name = 'Australia');
```
** Otra manera de utilizar las consultas anidadas en con la notación IN ** 

La misma consulta de antes con **IN** seria de la siguiente manera:
```sql
SELECT name, continent
FROM world
WHERE continent IN ( SELECT continent
                     FROM world
                     where name IN ('Argentina','Australia'));
```

## SUBCONSULTA CORRELACIONADA O SINCRONIZADA
En una consulta de base de datos SQL, una subconsulta correlacionada (también conocida como subconsulta sincronizada) es una subconsulta  que utiliza valores de la consulta externa. Debido a que la subconsulta puede evaluarse una vez por cada fila procesada por la consulta externa.

Ejemplo: Mostramos el continente, el nombre y el area de pais con con el area mas grande
```sql
SELECT continent, name, area
FROM world x
WHERE area >= ALL ( SELECT area
                    FROM world y
                    WHERE y.continent = x.continent
                    AND area > 0 );
```
Otro ejemplo: Encuentra los continentes donde todas los paises tienen una población menor a 250000000 , despues encuentra los nombres de los paises asociados con eses continentes, muestra el nombre, continente y población
```sql
SELECT name, continent, population
FROM world x
WHERE 250000000 >= ALL ( SELECT population
                        FROM world y
                        WHERE x.continent = y.continent
                        AND y.population > 0 );
```

# Funciones SUM , COUNT y AVG <a name="sumCount"></a>
 La función COUNT() devuelve el numero de filas que coinciden con el criterio especificado
 ```sql
SELECT COUNT(nombre_columna)
FROM nombre_tabla
WHERE condicion;
```
 La función SUM() devuelve la suma total de una columna numérica
```sql
SELECT AVG(nombre_columna)
FROM nombre_tabla
WHERE condicion;
``` 
 La función AVG() devuelve el valor medio de una columna numérica
```sql
SELECT SUM(nombre_columna)
FROM nombre_tabla
WHERE condicion;
```

# Notación HAVING <a name="having"></a>
La notación **HAVING** se utiliza para incluir una condición en agrupaciones, por ejemplo funciones SUM() o MAX()

Ejemplo: Listamos los continentes que **TIENEN** una población total de por lo menos 100 millones
```sql
SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000;
```

# Notación GROUP BY <a name="groupby"></a>
La notación **GROUP BY** es una sentencia que agrupa filas que tienen el mismo valor en dentro de una tabla como "encuentra el numero de clientes en cada pais"

Usualmete se usa con funciones de agregado como **COUNT, MAX, MIN, SUM, AVG** para agurpar el resultado en una o mas columnas

Sintaxis:
```sql
SELECT columna
FROM tabla
WHERE condicion
GROUP BY columna
ORDER BY columna;
```

# Notación ORDER BY <a name="orderby"></a>
La notación **ORDER BY** se utiliza para ordernar el resultado de modo ascendiente o descendiente

Modo ascendeinte (**ASC**) es la opcion por defecto, para ordenar de modo descenciente se utiliza la palabra clave **DESC**

sintaxis:
```sql
SELECT columna1, columna2
FROM tabla
ORDER BY columna1, columna2 ASC | DESC;
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
