
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
WHERE  name LIKE '%United%' 
```



```SQL
```
