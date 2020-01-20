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
####
```SQL

```
####
```SQL

```
####
```SQL

```
####
```SQL

```
####
```SQL

```
