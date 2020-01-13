# JOIN
### Nos permite hacer consultas de varias tablas 
### Estructura de ejemplo:
'''  SQL
SELECT *
  FROM game JOIN goal ON (id=matchid)
'''

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


```
#### 

```SQL


```
#### 

```SQL


```
