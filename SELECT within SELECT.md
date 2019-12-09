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
#### 

```SQL


```
