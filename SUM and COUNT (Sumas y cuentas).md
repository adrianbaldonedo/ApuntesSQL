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


#### cual es la poblacion de pais1 , pais2 , pais 3

```SQL


```

#### para cada continente muestra el continente y su numero de paises

```SQL
SELECT continent, COUNT(name) as 'Nº of countries'
FROM world
GROUP BY continent;

```
