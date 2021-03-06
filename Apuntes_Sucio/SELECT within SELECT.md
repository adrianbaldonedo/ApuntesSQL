# SELECT anidados
### Mayor que otra consulta
#### Seleccionamos el nombre de los paises que su poblacion sea mayor que la consulta de la poblacion de rusia
```SQL
SELECT name 
FROM world
WHERE population >
     (SELECT population FROM world
      WHERE name='Russia');

```
#### Selecionamos los paises de un continente con un producto interior bruto mayor que el de otro pais  
```SQL
SELECT name
FROM world
WHERE (GDP/population) > 
             (SELECT (GDP/population) FROM world WHERE name = 'United Kingdom')
AND continent = 'Europe';

```
#### Seleccionamos el pais y el continente de los paises que su continente sea argentina o australia ( consulta con varios selects anidados)

```SQL
SELECT name, continent
FROM world
WHERE continent = ( SELECT continent
                    FROM world
                    WHERE name = 'Argentina')
OR continent = ( SELECT continent
                    FROM world
                    WHERE name = 'Australia')
ORDER by name ASC;
```

OTRA MANERA UTILIZANDO IN:
```SQL
SELECT name, continent
FROM world
WHERE continent IN ( SELECT continent
                    FROM world
                    WHERE name IN ('Argentina', 'Australia'))
ORDER by name;
```
#### Mostrar paises que tienen mas gente que canada y menos que polonia ( consulta con varios selects anidados)

```SQL
SELECT name, population
FROM world
WHERE population > ( SELECT population
                     FROM world
                     WHERE name = 'Canada')
AND population < ( SELECT population
                     FROM world
                     WHERE name = 'Poland');
```
OTRA MANERA con BETWEEN

```SQL
SELECT name, population
FROM world
WHERE population BETWEEN ( SELECT population + 1
                     FROM world
                     WHERE name = 'Canada')
                 AND ( SELECT population - 1
                       FROM world
                       WHERE name = 'Poland');

```

#### mostrar todos nombres del pais y la poblacion en porcentaje de todos los paises de Europa pero el porcentaje es comparado con alemania.

```SQL
SELECT name, 
            CONCAT(ROUND(100 * population /( SELECT population 
                                      FROM world 
                                      WHERE name = 'Germany'), 0) , '%') As 'population (%)'
FROM world
WHERE continent = 'Europe';

```
#### Selecionamos los paises que tengan un GDP mayor que la suma de todos los paises de Europa

```SQL
SELECT name
FROM world
WHERE gdp > ALL (SELECT gdp
                 FROM world
                 where continent = 'Europe'
                 AND gdp is NOT NULL);

```
### Correlated or Synchronized sub query
#### Mostramos el continente , el nombre y el area del pais mas grande con el area mas grande

```SQL
SELECT continent, name, area
FROM world x
WHERE area >= ALL (SELECT area
                   FROM world y
                   WHERE y.continent=x.continent
                   AND area>0);

```
#### Mostramos el nombre y el continente del primer pais ( por orden alfabetico de cada continente).

```SQL
SELECT continent, name 
FROM world x
WHERE name <= ALL (SELECT name 
                   FROM world y
                   WHERE y.continent=x.continent);

```

#### Encuentra los continentes donde todas los paises tiengan una poblacion menor a 250000000 despues encuentra los nombre de los paises asociados con eses continentes, muestra nombre , continente y poblacion

```SQL
SELECT name, continent, population 
FROM world x
WHERE 25000000 >= ALL(SELECT population
	                FROM world y
		        WHERE x.continent = y.continent
                        AND y.population>0);

```
#### Algunos paises tienen una poblacion 3 veces mayor que cualquiera de sus vecinos  ( en el mismo continente)

```SQL
SELECT name, continent 
FROM world x
WHERE population >= ALL(SELECT population*3
                        FROM world y
                        WHERE x.continent = y.continent
                        and y.name != x.name);

```

#### 

```SQL


```
