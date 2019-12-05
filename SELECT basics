La estructura de una sentencia SQL 
1. indicamos las tablas de las cuales vamos a sacar la informacion.
2. Indicamos una condición.
3 Indicamos si queremos un Orde
4. Indicamos un limite

##SELECCIONAR DE UNA TABLA

Utilizamos la notación FROM para indicar la tabla de la cual vamos a recoger información.

SELECT name, population 
FROM world   /* SELECCIONAMOS DE LA TABLA world */
WHERE name >= 'France'
ORDER BY population ASC
LIMIT 10

##CONJUNTOS
Utilizamos la notacion IN para los conjuntos, estes van entre parentesis y en comillas simples y separados por comas.

SELECT name, population 
FROM world
WHERE name IN ('Sweden', 'Norway', 'Denmark'); /* SELECCIONAMOS EL CONJUNTO DE ESTES TRES PAISES */

##BETWEEN

Utilizamos esta notación BETWEEN para seleccionar un rango de valores intermedios entre dos numeros

SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000 /* SELECCIONA LOS VALORES ENTRE 200000 Y 250000 AMBOS INCLUIDOS */
  
  ##LIKE ( PATTERN MATCHING STRINGS)
  
  Utilizamos esta notación LIKE para buscar un patron / caracter / simbolo  y que coincida
  En SQL usar Mayusculas y Minusculas no es lo mismo. Es extricto el Matching de caracteres.
  
  SELECT name FROM world
  WHERE name LIKE 'B%' /* SELECCIONA DE LA TABLA LOS NOMBRES QUE TENGAN UNA B, SE PUEDE PONER %B%, %GER% %Germany% */
  
  #Finalizen con con un caracter se indica con %char
  
  SELECT name FROM world
  WHERE name LIKE 'Y%'
  
  #Contengan un caracter %char%
  
  SELECT name FROM world
  WHERE name LIKE '%x%'
  
  #Finalicen con varios caracteres(string)
  
  SELECT name FROM world
  WHERE name LIKE '%land'
  
  #Que empiecen con un caracter y terminen con un conjunto de caracteres
  
  SELECT name FROM world
  WHERE name LIKE 'C%ia' 
  
  #Contiene un caracter doble
  
  SELECT name FROM world
  WHERE name LIKE '%ee%'
  
  #Varios caracteres repetidos pero separados 
  
  SELECT name FROM world
  WHERE name LIKE '%a%a%a%' /* en una lista de paises encontraria bahamas */
  
#Segundo caracter Se utiliza un guion bajo como sustituto de un caracter y solo un caracter 
  
SELECT name 
FROM world
WHERE name LIKE '_t%' /* En una lista de paises solo nos mostraria los paises que como segundo caracter tienen una t */
ORDER BY name
  
# mismo caracter separado por dos caracteres 
  
SELECT name FROM world
WHERE name LIKE '%o__o%' /* UTILIZAMOS DOS GUIONES BAJOS ENTRE LAS DOS o's PARA BUSCAR PAISES QUE TENGAN DOS SEPARADAS POR DOS CARACTERES */

#Exactamentes x numeros de caracteres

SELECT name FROM world
WHERE name LIKE '____' /* SE UTILIZA UN NUMERO DE GUIONES BAJOS IGUAL AL NUMERO DE CARACTERES PEDIDOS */

##NOTA -> si piden como minimos x letras usamos guines bajos y un porcentaje '____%' y si solo pide x numero (ej: 4) '____'
 
# Exacto mismo nombre en una tabla y en otra

SELECT capital
FROM world
WHERE name LIKE capital /* SE SELECCIONA UN PAIS DONDE EL NOMBRE DE LA CAPITAL ES EL MISMO QUE EL DEL PAIS */ /* EN VEZ DEL LIKE TAMBIEN VALE CON = */

# CONCAT Usamos la funcion concat para machear pero añadiendo algun string )
En este ejemplo queremos los paises que su capital sea el nombre del pais + City 

SELECT name /* SELECCIONAR NOMBRE*/
FROM world /* DESDE LA TABLA world */
WHERE capital LIKE concat(name, ' city') /* DONDE LA CAPITAL ES IGUAL A LA CONCATENACION DEL NOMBRE MAS CITY */
 
 # CONCAT Cuando incluye 
 
SELECT capital, name /* FIJARSE EN EL ENUNCIADO POR QUE PIDE CAPITAL Y NAME */
FROM world
WHERE capital LIKE CONCAT ( '%', name, '%') /* CAULQUIER COSA + NOMBRE DE PAIS + CUALQUIER COSA */

# CONCAT concatenando extensiones a un atributo

SELECT capital, name
FROM world
WHERE capital LIKE CONCAT ( name, '_%') /* ES ASI AUN NO SE MUY BIEN POR QUE , CREO QUE ES POR QUE EL GUION BAJO INDICA QUE TIENE QUE HABER ALGO SI O SI ENTRE EL NOMBRE DEL PAIS Y CUALQUIER COSA */




 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
  
