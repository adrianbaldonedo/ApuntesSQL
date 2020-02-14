**La estructura de una sentencia SQL** 

 1. indicamos las tablas de las cuales vamos a sacar la información.
 2. Indicamos una condición.
 3. Indicamos si queremos un Orden
 4. Indicamos un limite

### SELECCIONAR DE UNA TABLA

Utilizamos la notación **FROM** para indicar la tabla de la cual vamos a recoger información.

SELECT name, population

FROM world   **SELECCIONAMOS DE LA TABLA world**

WHERE name >= 'France'

ORDER BY population ASC

LIMIT 10

### CONJUNTOS
Utilizamos la notación **IN** para los conjuntos, estes van entre paréntesis y en comillas simples y separados por comas.

SELECT name, population 

FROM world

WHERE name IN ('Sweden', 'Norway', 'Denmark');  **SELECCIONAMOS EL CONJUNTO DE ESTES TRES PAÍSES** 

### BETWEEN

Utilizamos esta notación **BETWEEN** para seleccionar un rango de valores intermedios entre dos números

SELECT name, area FROM world

  WHERE area BETWEEN 200000 AND 250000  **SELECCIONA LOS VALORES ENTRE 200000 Y 250000 AMBOS INCLUIDOS** 
  
  
  ### LIKE ( PATTERN MATCHING STRINGS)
  
  Utilizamos esta notación **LIKE** para buscar un patrón / carácter / símbolo  y que coincida
  **En SQL usar Mayúsculas y Minúsculas no es lo mismo. Es estricto el Matching de caracteres.**
  
  La diferencia entre LIKE Y "=" a la hora de comparar es que LIKE busca una expresion regular,  y "=" es para buscar una cadena tal    cual y como esta escrita ( LIKE 'The%' vs = 'The%')
  
  SELECT name FROM world
  
  WHERE name LIKE 'B%' **SELECCIONA DE LA TABLA LOS NOMBRES QUE TENGAN UNA B, SE PUEDE PONER %B%, %GER% %Germany%**
  
  ### Finalicen con con un carácter se indica con %char
  
  SELECT name FROM world
  
  WHERE name LIKE 'Y%'
  
   ### Contengan un carácter %char%
  
  SELECT name FROM world
  
  WHERE name LIKE '%x%'
    
   ### Finalicen con varios caracteres(string)
  
  SELECT name FROM world
  
  WHERE name LIKE '%land'
  
   ### Que empiecen con un carácter y terminen con un conjunto de caracteres
  
  SELECT name FROM world
  
  WHERE name LIKE 'C%ia' 
  
   ### Contiene un carácter doble
  
  SELECT name FROM world
  
  WHERE name LIKE '%ee%'
  
   ### Varios caracteres repetidos pero separados 
  
  SELECT name FROM world
  
  WHERE name LIKE '%a%a%a%' **en una lista de países encontraría Bahamas** 
  
 ### Segundo carácter Se utiliza un guion bajo como sustituto de un carácter y solo un carácter 
  
SELECT name 

FROM world

WHERE name LIKE '_t%'  **En una lista de países solo nos mostraría los países que como segundo carácter tienen una t** 

ORDER BY name
  
 ### mismo carácter separado por dos caracteres 
  
SELECT name FROM world

WHERE name LIKE '%o__o%'  **UTILIZAMOS DOS GUIONES BAJOS ENTRE LAS DOS o's PARA BUSCAR PAISES QUE TENGAN DOS SEPARADAS POR DOS CARACTERES** 

 ### Exactamente x números de caracteres

SELECT name FROM world

WHERE name LIKE '____' /* SE UTILIZA UN NUMERO DE GUIONES BAJOS IGUAL AL NUMERO DE CARACTERES PEDIDOS */

 ### NOTA -> si piden como mínimos x letras usamos guiones bajos y un porcentaje '____%' y si solo pide x numero (ej: 4) '____'
 
 ### Exacto mismo nombre en una tabla y en otra

SELECT capital

FROM world

WHERE name LIKE capital  **SE SELECCIONA UN PAÍS DONDE EL NOMBRE DE LA CAPITAL ES EL MISMO QUE EL DEL PAÍS EN VEZ DEL LIKE TAMBIÉN VALE CON =** 

 ### CONCAT 
Usamos la función concat para machear pero añadiendo algún string )
En este ejemplo queremos los países que su capital sea el nombre del país + City 

SELECT name /* SELECCIONAR NOMBRE*/

FROM world /* DESDE LA TABLA world */

WHERE capital LIKE concat(name, ' city')  **DONDE LA CAPITAL ES IGUAL A LA CONCATENACIÓN DEL NOMBRE MAS CITY** 
 
  ### CONCAT Cuando incluye 
 
SELECT capital, name  **FIJARSE EN EL ENUNCIADO POR QUE PIDE CAPITAL Y NAME** 

FROM world

WHERE capital LIKE CONCAT ( '%', name, '%') **CUALQUIER COSA + NOMBRE DE PAÍS + CUALQUIER COSA** 

 ### CONCAT concatenando extensiones a un atributo

SELECT capital, name

FROM world

WHERE capital LIKE CONCAT ( name, '_%')  **ES ASÍ AUN NO SE MUY BIEN POR QUE , CREO QUE ES POR QUE EL GUION BAJO INDICA QUE TIENE QUE HABER ALGO SI O SI ENTRE EL NOMBRE DEL PAÍS Y CUALQUIER COSA** 

## REPLACE
Usamos el replace para remplazar , el formato es REPLACE ( 'cadena' , ' a ' ,'e ') daria como resultado cedene
SELECT name, REPLACE(capital, name, '') AS ext ***en la capital replazamos el nombre por un espacio vacio y a la fila le llamamos ext***

FROM world

WHERE capital LIKE CONCAT ( name, '_%')
