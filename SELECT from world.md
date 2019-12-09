
### Operaciones con el SELECT
dentro del SELECT indicamos la operacion que nos indica el producto interior bruto per capita, diviendo el producto interior bruto en tre el numero de la población
```SQL
SELECT name, (GDP / population ) AS GDPpercapita ***hacemos la operacion dentro del SELECT)***
FROM world
WHERE population >= 200000000;
```
### Mostrar en millones una cantidad
dentro del SELECT le indicamos que queremos la poblacion en millones y no el numero en RAW para esto dividimos la poblacion entre un millon

```SQL
SELECT name, (population / 1000000) AS population
FROM world
WHERE continent LIKE 'South America';
```
### IN
Utilizamos la expresion in para indicar dentro de que campos queremos buscar 

```SQL
SELECT name, population
FROM world
WHERE name IN ( 'France', 'Germany', 'Italy');

Otra manera:

SELECT name, population
FROM world
WHERE name ='France'
OR name = 'Germany'
OR name ='Italy';

```
### LIKE
Utilizamos LIKE para buscar algo que incluya la expresion que le indicamos (Caracteres, strings , caracteres especiales , etc)

```SQL
SELECT name
FROM world
WHERE  name LIKE '%United%';
```
### Filtar por tamaño
Utilizamos el operador > para buscar mayores que x numero tambien usamos el OR para que haga una comprobación doble 

```SQL
SELECT name, population, area
FROM world
WHERE population > 250000000
OR area > 3000000;

```
### XOR ( OR exclusivo )
Utilizamos el OR exclusivo cuando queremos buscar por una cosa o la otra pero no por los dos.

```SQL
Manera sin XOR:
SELECT name, population, area
FROM world
WHERE ( area > 3000000 OR population > 250000000)
AND NOT ( area > 3000000 AND population > 250000000);

Manera con XOR:
SELECT name, population , area
FROM world
WHERE area > 3000000
XOR population > 250000000;

```
```
OR a b XOR
 0 0 0 0
 1 0 1 1
 1 1 0 1
 1 1 1 0
 ```

### ROUND 
Utilizamos ROUND para redondear un resultado, el formato es ROUND( 'Dato'/numero, cantidad de decimales)
En este caso mostramos poblacion entre un millon y enseñamos 2 decimales y tambien Producto interior butro entre un billon y mostramos 2 decimales

```SQL
SELECT name, ROUND(population / 1000000, 2) AS population, ROUND(GDP / 1000000000, 2) AS GDP
FROM world
WHERE continent LIKE 'South America';

```
###

```SQL
```

```SQL
```
