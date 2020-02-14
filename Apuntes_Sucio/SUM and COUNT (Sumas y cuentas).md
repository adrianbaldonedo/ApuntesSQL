## SUM and COUNT

#### Muestra la poblacion total del Mundo

```SQL
SELECT SUM(population) as "Poblacion Total"
FROM world;
```

#### Lista todos los continentes ( solo una vez cada uno)

```SQL
SELECT DISTINCT(continent)
FROM world;
```

#### Total del Gdp de Africa 

```SQL
SELECT SUM(GDP)
  FROM world
  WHERE continent = 'Africa';
```


#### Cuantos paises tienen un area de al menos 1000000 de area

```SQL
SELECT COUNT(name)
FROM world
WHERE area >= 1000000;
```


#### cual es la poblacion de pais1, pais2, pais3

```SQL
SELECT SUM(population) as 'Total Population'
FROM world
WHERE name = 'Estonia' 
      OR name = 'Latvia'
      OR name = 'Lithuania';

```

#### para cada continente muestra el continente y su numero de paises

```SQL
SELECT continent, COUNT(name) as 'Nº of countries'
FROM world
GROUP BY continent;

```
#### para cada continente muestra el continente y el numero de paises donde la poblacion es de por lo menos 10 millones

```SQL
SELECT continent, COUNT(name) AS 'Nº of countries'
FROM world
WHERE population >= 10000000
GROUP BY continent;

```
## HAVING 
### CUANDO EJECUTAMOS ALGO SOBRE LA TABLA PRICIPAL SE USA EL WEHERE , SI LO QUE EJECUTAMOS SE UTILIZA SOBRE LA AGRUPACION UTILIZAMOS HAVING
#### Lista los continentes que TIENEN una poblacion totan de por lo menos 100 millones

```SQL
SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000;

```
