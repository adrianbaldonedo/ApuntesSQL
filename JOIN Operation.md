# JOIN
### Nos permite hacer consultas de varias tablas 
### Estructura de ejemplo: ( por cada tupla de la tabla game insertas todas las tuplas de la tabla goal y el punto de unon son las claves id y matchid )
```SQL
SELECT *
  FROM game JOIN goal ON (id=matchid)
```

#### Modifica para mostrar esclusivamente el matchid y el jugador, para identificar los jugadores alemanes comprueba por teamid = 'GER'

```SQL
SELECT matchid, player
FROM goal 
WHERE teamid = 'GER';

```
#### Muestra id, stadium, team1, team2 para el partido 1012

```SQL
SELECT id,stadium,team1,team2
  FROM game
WHERE id = 1012;

```
#### Modifica para que enseñe el player, teamid, stadium y mdate para cada gol aleman

```SQL
SELECT goal.player, goal.teamid, game.stadium, game.mdate
  FROM game JOIN goal ON game.id = goal.matchid
  WHERE goal.teamid = 'GER';

```
#### Muestra team1, team2 y player para cada goal marcado por el jugador Mario 

```SQL
SELECT game.team1, game.team2, goal.player
FROM game JOIN goal ON game.id = goal.matchid
WHERE goal.player LIKE 'Mario%'

```
#### Muestra el player, teamid, coach, gtime para todos los goles marcados en los primeros 10 minutos (gtime <=10)

```SQL
SELECT goal.player, goal.teamid, eteam.coach, goal.gtime
FROM goal JOIN eteam ON goal.teamid = eteam.id
WHERE goal.gtime<=10

```
#### Lista las fechas de los partidos y los nombre del equipo en el que 'Fernando Santos' fue entrenador del team1

```SQL
SELECT game.mdate, eteam.teamname
FROM game JOIN eteam ON game.team1=eteam.id
WHERE eteam.coach = 'Fernando Santos';

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
