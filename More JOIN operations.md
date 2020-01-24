### Cuando tenemos varias tablas pero alguna de ellas es una tabla que sale de una relacion , y necesitemos hacer JOINS entre otras dos tablas que no se comunican directamente , lo que necesitamos hacer es en el JOIN pasando por las relaciones
#### lista las peliculas donde el año es 1962
``` SQL
SELECT id, title
FROM movie
WHERE yr=1962

```
#### Cuando se estreno Ciudadano Kane
```SQL
SELECT movie.yr
FROM movie
WHERE movie.title = 'Citizen Kane'
```
#### lista las peliculas de star Trek incluyendo id titulo y año y ordenalos por año
```SQL
SELECT movie.id, movie.title, movie.yr
FROM movie
WHERE movie.title LIKE '%Star Trek%'
ORDER BY movie.yr ASC;

```
#### Cual es el numero de identificacion que tiene el actor Glenn Close
```SQL
SELECT actor.id
FROM actor
WHERE actor.name = 'Glenn Close'

```
#### cual es el id de la pelicula casablanca
```SQL
SELECT movie.id
FROM movie
WHERE movie.title = 'Casablanca';

```
#### Lista de actores que acutan en Casablanca
Pista : movieid = 27
```SQL
SELECT actor.name
FROM actor JOIN casting ON actor.id = casting.actorid
WHERE casting.movieid = 27;

```
Sin Pista
``` SQL
SELECT actor.name
FROM actor JOIN casting ON actor.id = casting.actorid
WHERE casting.movieid = (
           SELECT movie.id
           FROM movie
           WHERE movie.title = 'Casablanca'
          
);

```
Lo mismo pero con JOIN
``` SQL
SELECT actor.name
FROM actor JOIN casting ON actor.id = casting.actorid
           JOIN movie ON movie.id = casting.movieid
WHERE movie.title = 'Casablanca';

```
#### consigue la lista de actores de la pelicula Alien
```SQL
SELECT actor.name
FROM actor JOIN casting ON actor.id = casting.actorid   -- por cada tubpla de la tabla actor insertarmos todas las tuplas de la tabla casting pero filtramos donde el id del actor sea igual al  idactor
           JOIN movie ON movie.id = casting.movieid     -- por cada tubpla de la tabla actor insertarmos todas las tuplas de la tabla movie pero filtramos donde el id de la pelicula sea igual al  idmovie
WHERE movie.title = 'Alien';

```
#### lista todas las peliculas donde aparezca Harrison Ford
```SQL
SELECT movie.title
FROM movie JOIN casting ON movie.id = casting.movieid
           JOIN actor ON actor.id = casting.actorid
WHERE actor.name = 'Harrison Ford';

```
#### lista todas las peliculas donde aparezca Harrison Ford pero que no sea el actor principal
```SQL
SELECT movie.title
FROM movie JOIN casting ON movie.id = casting.movieid
           JOIN actor ON actor.id = casting.actorid
WHERE actor.name = 'Harrison Ford'
AND ord <> 1;

```
#### Lista todas las peliculas con su actor principal las pelicualas estrenadas en el año 1962
```SQL
SELECT movie.title, actor.name
FROM movie JOIN casting ON movie.id = casting.movieid
           JOIN actor ON actor.id = casting.actorid
WHERE movie.yr = 1962 AND casting.ord = 1;

```
#### Cuales fueron los años mas ocupados para Rock Hudson, enseña el año y el numero de peliculas que hizo cada años en las cuales actuo en mas de dos peliculas
```SQL
SELECT movie.yr, COUNT( movie.title) AS 'nº of films'
FROM movie JOIN casting ON movie.id = casting.movieid
           JOIN actor ON casting.actorid = actor.id
WHERE actor.name = 'Rock Hudson'
GROUP BY movie.yr
HAVING COUNT(movie.title) > 2;

```
#### list las peliculas y el actor principal para todos las peliculas en las que aparece Julie Andrews
```SQL
SELECT movie.title, actor.name
FROM movie JOIN casting ON movie.id = movieid
           JOIN actor ON actorid = actor.id
WHERE casting.ord = 1 
AND movie.id IN (SELECT movie.id
                        FROM movie JOIN casting ON movie.id = movieid
                                   JOIN actor ON actorid = actor.id
                        WHERE actor.name = 'Julie Andrews'
                        );
```
#### obten un lista , en orden alfabetico , de los actores que tuvieron por lo menos 30 papeles principales
```SQL
SELECT actor.name
FROM actor JOIN casting ON actor.id = casting.actorid
WHERE casting.ord = 1
GROUP BY actor.name
HAVING COUNT(casting.movieid) >= 30
ORDER BY actor.name asc;

```
#### lista las peliculas que se estrenaron en el año de 1978Y EL NUMERO DE ACTORES ordenados por el numero de actores participantes y luego por titulo
```SQL
SELECT movie.title, COUNT(casting.actorid)
FROM movie JOIN casting ON movie.id = movieid
           JOIN actor ON actorid = actor.id
WHERE movie.yr = 1978
GROUP BY movie.title
ORDER BY COUNT(casting.actorid) DESC, movie.title ASC;

```
#### lista toda la lista de gente que trabajo con 'Art Garfunkel'
```SQL

-- SOLUCION CON JOINS

SELECT DISTINCT Actor1.name   
FROM actor AS Actor1 JOIN casting AS casting1
                     ON Actor1.id = casting1.actorid
                     JOIN casting AS casting2
                     ON casting1.movieid = casting2.movieid
                     JOIN actor AS Actor2
                     ON Actor2.id = casting2.actorid
WHERE Actor2.name = 'Art Garfunkel'
AND Actor2.id <> Actor1.id;

-- SOLUCION CON SUBCONSULTAS
SELECT
FROM
WHERE

SELECT
FROM
WHERE







```
####
```SQL

```
####
```SQL

```
