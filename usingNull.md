## NULL para usar el NULL en nuestro where se usa con IS o con IS NOT
#### Listar todos los profesores con departamenteo nulo
```sql
SELECT teacher.name
FROM teacher
WHERE teacher.dept IS NULL;

```
#### NOTA en el INNER JOIN faltan los profesores sin departamento y los departamentos sin profesores 
Al realizar la consulta tal y como esta no salen lo nulos
```sql
SELECT 
      teacher.name As Profes,
      dept.name As Departamentos
 FROM teacher INNER JOIN dept
           ON (teacher.dept=dept.id);
```
#### Usamos un Join diferente que liste todos los profesores
## LEFT JOIN saca todos los elementos de la izquierda aun que los de la derecha sea nulo. 
```sql
SELECT 
      teacher.name As Profes,
      dept.name As Departamentos
 FROM teacher LEFT JOIN dept
           ON (teacher.dept=dept.id);

```
#### Usa un join diferente para que se listen todos los departamentos
## RIGHT JOIN  saca todos los elementos de la derecha aun que los de la izquierda sea nulo
```sql
SELECT 
      teacher.name As Profes,
      dept.name As Departamentos
 FROM teacher RIGHT JOIN dept
           ON (teacher.dept=dept.id);

```
#### Enseña el nombre de el profesor y su numero de movil y si el valor del numero de movil es nulo muestra 07986 444 2266
## COALESCE coje cualquier numero de argumentos y devuelve el primero valor que no es nullo
Esto se usa para dar valores a los nulos , por ejemplo poner a cero o poner a cadena vacia para que no rompan los programas
  COALESCE(x,y,z) = x if x is not NULL
  COALESCE(x,y,z) = y if x is NULL and y is not NULL
  COALESCE(x,y,z) = z if x and y are NULL but z is not NULL
  COALESCE(x,y,z) = NULL if x and y and z are all NULL

```sql
SELECT teacher.name, COALESCE(teacher.mobile, '07986 444 2266') As mobile
FROM teacher;

```
#### Usa la funcion COALESCE y la funcion LEFT JOIN para imprimir el nombre del profesor y el nombre de su departamento, usando 'None' donde no hay departamento
```sql
SELECT teacher.name, COALESCE(dept.name, 'None') AS Departamento
FROM teacher LEFT JOIN dept
           ON (teacher.dept=dept.id);
```
#### usa la funcion COUNT para enseñar el numero de profesores y el numero de numeros de movil
## COUNT por lo que se ve al realizar esta consulta no cuenta los valores nulos
```sql
SELECT COUNT(teacher.name) AS 'nº profes',
       COUNT(teacher.mobile) AS 'nº de moviles'
FROM teacher
```
#### Usa la funcion COUNT Y GROUP BY dept.name para enseñar cada departamento y el numero de personas en ese deptartamento, usar RIGHT JOIN para asegurarse que el departamento Engineering aparece en la lista 
```sql
SELECT dept.name, COUNT(teacher.name) AS Staff
FROM teacher RIGHT JOIN dept ON dept.id = teacher.dept
GROUP BY dept.name;

```
## EL uso de CASE nos permite devolver un valor diferente segun diferentes condiciones, si no hay condiciones y ningun else entonces se retorana el valor null 
Ejemplo de como se usa:

```sql
vacio:
 CASE WHEN condition1 THEN value1 
       WHEN condition2 THEN value2  
       ELSE def_value 
  END 
  con codigo:
  SELECT name, population
      ,CASE WHEN population<1000000 
            THEN 'small'
            WHEN population<10000000 
            THEN 'medium'
            ELSE 'large'
       END
  FROM bbc

```
#### usa la funcion CASE para mostrar el nombre de cada profesor seguido de 'Sci' si el departamento del profesor es 1 o 2 y si 'Art' si es de cualquier otro departamento
```sql
SELECT teacher.name, 
       CASE WHEN dept = 1
       THEN 'Sci'
       WHEN dept = 2
       THEN 'Sci'
       ELSE 'Art'
    END
FROM teacher LEFT JOIN dept ON dept.id = teacher.dept

```
####
```sql

```
####
```sql

```
####
```sql

```
####
```sql

```
####
```sql

```
