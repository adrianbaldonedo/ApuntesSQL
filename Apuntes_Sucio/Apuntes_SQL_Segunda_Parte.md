# Apuntes SQL segunda parte.

## SubLenguajes SQL <a name="subsql"></a>

> EL DQL , DML y DDL son el nucleo de SQL

| Notación | Nombre | Contenido |
| -------- | ------ | --------- |
| DQL | DATA QUARY LANGUAGE | SELECT |
| DML | DATA MANIPULATION LANGUAGE | Opera sobre los datos -- INSERT, UPDATE, DELETE |
| DDL | DATA DEFINITION LANGUAGE | Opera sobre la los objetos de la base de datos (sobre la estructura) -- CREATE, ALTER, DROP |
| TCL | TRANSACCION CONTROL LANGUAGE | TRANSACCIÓN -> COMPOSICIÓN DE DISTINTAS INSTRUCCIONES SQL -- COMMIT, ROLLBACK, SAFE POINT |
| DCL | DATA CONTROL LANGUAGE | DAR PERMISOS -- GRANT, REVOKE |
| SCL | SESION CONTROL LANGUAGUE | MANEJAR DINÁMICAMENTE PROPIEDADES DE UNA SESIÓN DE USUARIO -- ALTER SESSION |


# TIPOS DE DATOS (DOMINIOS) <a name="tiposDominios"></a>

> Si al escoger el tipo de datos utilizamos el que mas se ajuste, las bases de datos trabajan mas rapdio

> Escoger el mejor tipo de datos = mejor rendimiento

Lista de Dominios mas comunes ( en negrita los que utilizaremos).

| Tipo de Dato | Descripción | Rango |
| ------------ | ----------- | ------|
| SMALLINT | numeros enteros cortos | -32768 to +32767 |
| **INTEGER** | **numeros enteros, 4bytes** | **-2147483648 to +2147483647** |
| BIGINT |  enteros largos, 8 bytes | -9223372036854775808 to 9223372036854775807 |
| **DECIMAL** | **numeros decimales** | **hasta 131072 de digitos antes de la coma y hasta 16383 digitos decimales despues de la coma** |
| **REAL** | **numeros decimales pequeños** | **6 digitos decimales** |
|  |  |  |
| **MONEY** | **monedas, 8 bytes** | **-92233720368547758.08 to +92233720368547758.07** |
|  |  |  |
| **TEXT** | **texto** | **longitud ilimitada variable** |
| **CHAR(N)** | **N caracteres, 1 byte por carácter** | **Longitud fija** |
| **VARCHAR(N)** | **cadena de N caracteres** | **Longitud variable** |
|  |  |  |
| **DATE** | **fechas** | **4 bytes** |
| **TIME** | **hora del dia** | **8 bytes** |
| **TIMESTAMP** | **hora del dia y fecha del dia** |  |
| INTERVAL |  |  |
|  |  |  |
| **BOOLEAN** | **verdadero o falso, 1 byte** | **TRUE , FALSE , NULL** |
|  |  |  |
| POINT | punto en un plan, 16 bytes | x,y |
| BOX | caja rectangular, 32 bytes |  |
|  |  |  |
| CIDR | direcciones ipv4 y ipv6 | 7 o 19 bytes |
| INET | direcciones ipv4 y ipv6 host and networks | 7 o 19 bytes |
| MACADDR | direcciones mac | 6 bytes |
| UUID  |  |  |
| JSON |  |  |
| HSTORE |  |  |


# DDL: Create <a name="ddlCreate"></a>
La notación **CREATE** se utiliza para crear objetos de una base de datos o una base de datos en si y tambien para crear usuarios.

### Para crear una base de datos nueva utilizamos la notación CREATE SCHEMA

Sintaxis:
```SQL
 CREATE ( SCHEMA | DATABASE )  
  [ IF NOT EXIST ] <nombre_bd> 
  [ CHARACTER SET ] <nombre_setCaracteres>
  [ COLLATE ] collation_name;
  
```

Ejemplo: 
```sql
CREATE SCHEMA proyectoDeInvestigacion;
```

### Para crear una nueva tabla dentro de una base de datos utilizamos la notacion CREATE TABLE

Sintaxis:

```sql
CREATE TABLE nombre_tabla (
 COLUMNA TIPO_DATO
 Columna1 INTEGER [ NOT NULL ] PRIMARY KEY,
 Columna2 VARCHAR(50),
 Columna3 VARCHAR(200),
 Columna4 DATE
 [CONSTRAINT]
);
```

Ejemplo: 
```sql
CREATE TABLE proyectoDeInvestigacion.DEPARTAMENTO(
nombre_Dep VARCHAR(50) PRIMARY KEY,
telefono INTEGER NOT NULL,
director INTEGER,
FOREIGN KEY (director) REFERENCES proyectoDeInvestigacion.PROFESOR (DNI)
);
```

Para crear usuarios usamos la notación CREATE USER 

Sintaxis:
``` sql
CREATE USER username IDENTIFIED BY password IDENTIFIED WITH auth_plugin;
```
Ejemplo:
``` sql
CREATE USER jdovalf IDENTIFIED BY abc123. INDENTIFIED WITH auth_plugin;
```

### DDL: CREATE -- CONSTRAINT <a name="ddlCreateContraint"></a>

**Restriccion de Clave primaria**

Cuando creamos una tabla tenemos que especificar la clave primaria de la tabla , la cual no puede ser null , ni repetirse. Para esto utilizamos notación **CONSTRAINT**.

Hay varias maneras de utlizar esta formula , cuando la clave es una sola utlizamos la siguiente notación:

Sintaxis:
```sql
CREATE TABLE <nombre_tabla>(
<nombre_clave> <tipo_dato> PRIMARY KEY
);
```

Ejemplo:
```sql
CREATE TABLE proyectoDeInvestigacion.DEPARTAMENTO(
nombre_Dep VARCHAR(50) PRIMARY KEY
);
```

La siguiente opcion se usa cuando la clave primaria se compone de dos columnas

Sintaxis: 
```sql
PRIMARY KEY (<nomre_clave1>, <nombre_clave2>)
```
Ejemplo:
```sql
CREATE TABLE proyectoDeInvestigacion.UBICACION(
nombre_Dep VARCHAR(50),
nombre_Sede VARCHAR(50),
PRIMARY KEY (nombre_Dep, nombre_Sede)
);
```

**Restriccion de clave ajena**

En las tablas tambien tenemos que indicar cuales son las claves ajenas e indicar a que columna de otra tabla referencian.

Se refencia la tabla de la clave ajena y los atributos de la tabla de la clave ajena.
 
Sintaxis 
```sql
[CONSTRAINT <nombre-restriccion>]
FOREIGN KEY (<atributos>) REFERENCES <nombre-tabla-referecniada> [(<atributos-referenciados>)]
[MATCH FULL | PARTIAL]
[ON DELETE 
    CASCADE | NO ACTION | SET NULL | SET DEFAULT]
[ON UPDATE 
    CASCADE | NO ACTION | SET NULL | SET DEFAULT]
```
> On Delete --> por defecto se pone No ACTION pero la mejor opcion casi siempre SET NULL
 
> On Update --> lo mas propio sera usar la modificacion en CASCADE

```sql
CREATE TABLE proyectoDeInvestigacion.GRUPO(
nombre_Grupo VARCHAR(50),
nombre_Dep VARCHAR(50),
area INTEGER NOT NULL,
lider INTEGER,
PRIMARY KEY (nombre_Grupo, nombre_Dep),
FOREIGN KEY (lider) REFERENCES (nombre_Dep, nombre_Sede) proyectoDeInvestigacion.PROFESOR (DNI)
    ON DELETE SET NULL,
    ON UPDATE CASCADE
);
```

**Restriccion de unicidad**

> Para las claves secundarias ( los descartes de claves primarias ) se usa la resticcion de unicidad **UNIQUE**

> Se usa en las claves ajenas por que son descartes de clave pirmarias pero se tienen que seguir guardando de manera que no se pueda tener dos claves ajenas con los mismos datos.

Sintaxis:
```sql
[CONSTRAINT <nombre-de-la-restriccion>]
UNIQUE (<atributos>),
```

Ejemplo:
```sql
CREATE TABLE proyectoDeInvestigacion.PROYECTO (
cod_Proyecto INTEGER PRIMARY KEY,
nombre_Proyecto VARCHAR(50) NOT NULL UNIQUE,
fecha_inicio DATE NOT NULL,
fecha_fin DATE
);
```

**Restriccion de verificacion**

>La notacion **CHECK** es como el **WHERE** , permite hacer predicados.

> El predicado tiene que hacerse sobre los atributos

> Sirve como mecanismo para ejecutar inmediatamente o que se espere a un momento posterior . cada vez que se intenta algo del DML ( insertar un nuevo registro ,modificar o borrar).

> Solo se permite la actualizacion borrado o insercion cuando el predicado devuelve true.

> El valor determinado es no aplazable. (NOT DEFERRABLE INITIALLY INMEDIATE)

> La otra opcion seria posponer (DEFERRABLE INITIALLY DEFERRABLE)

> Con el NOT lo que se nos indica que no es aplazable tiene que hacerse en el momento

> Primero se escoje si es aplazable o no, si no es aplazable tiene que se initially inmediate y si es aplazable lo logico es initially deferrable

> La opcion DEFEREABLE solo tiene sendito cuando hacemos "transacciones"

Sintaxis:
```sql
[CONSTRAINT <nombre_de_restriccion>]
CHECK predicado (atributos)
[[NOT] DEFERRABLE ]
[INITIALLY INMEDIATE | DEFERABLE]
```

Ejemplo: 
```sql
CHECK saldo >= 0 (
     SELECT saldo
     FROM empleado
     WHERE departamento = 'A'); 
```


## DDL : DROP <a name="ddlDrop"></a>
### DROP SCHEMA <a name="ddlDropSchema"></a>

```SQL
DROP SCHEMA 
   [ IF EXISTS ] <nombre-de-la-BD>;
```

Con IF EXITS si la base de datos no existe daria false y no un error.

### DROP TABLE <a name="ddlDropTable"></a>

```SQL
DROP TABLE
[IF EXISTS] <nombre-de-la-tabla>
[ CASCADE | RESTRICT ];
```
RESTRICT se especifica que una tabla no se puede borrar si existe alguna dependencia

CASCADE borra todo.


## DDL : ALTER <a name="ddlAlter"></a>
### ALTER TABLE <a name="ddlAlterTable"></a>

Para alterar una tabla podemos , cargarnos una columna y poner una columna nueva, añadir restricciones nuevas, borrar restricciones existentes

a nivel columna: con ADD y con DROP
a nivel restricction con ADD y con DROP 

Entonces ALTER table tiene 4 opciones , dos para la culumna y dos para la restriccion

```sql
ALTER TABLE <nombre-tabla> ADD [COLUMN] <atributo> <tipo-dato> NOT NULL ...
                           DROP COLUMN <atributo> [CASCADE | RESTRICT]
                           ADD <nombre_de_restriccion>
                           DROP <nombre_de_restriccion>
```
#DML 
##DML: DELETE, INSERT, UPDATE
## INSERT <a name="ddlInsert"></a>
sirve para insertar una nueva tupla , entonces hay que indicar cuales son las columnas y que valores tienen las columnas
Formula:
```sql
INSERT INTO <nombre_de_la_tabbla> [(<atributo_1>, <atributo_2>, ...)] * l
(VALUES (<valor1>, <valor2>, ...) | SELECT ... );** 
```
* os atributos si no se pone nada se ponen en orden de creación
** En el Select tiene que ser el mismo numero de columnas, mismos dominios(tipo de datos (NCHAR, DATETIME, INTEGER)) que los de esa tabla

Se puede insertar varias tuplas a la vez de esta manera , justo despues del VALUES se separan con comas.
```sql
VALUES (
 1, 'queseo', 9.99),
(2, 'pan', 3.85),
(3, 'leche', 4.62);
```
## UPDATE <a name="ddlUpdate"></a>
se utiliza para actualizar las columnas en de cada tupla
La Formula: 
```sql
UPDATE <nombre_de_la_tabla>
SET <atributo1> = <valor1>, <columna2> = <valor2>, ...
[WHERE <predicado>];

Ejemplo:*
UPDATE world
SET name='España', continent ='Africa'
WHERE name='spain';

```
* actualizamos la tabla mundo, todas las tuplas donde el nombre sea españa le ponemos de nombre España y el continente Africa. 

## DELETE <a name="ddlDelete"></a>
se utiliza delete para borrar tuplas de una tabla
Formula: 
```sql
DELETE FROM <nombre_de_la_tabla>
[WHERE <predicado>]*;
´´´
* WHERE es opcional pero si no se indica una condicion nos cargariamos toda la tabla.


Ejemplo: 
```sql
DELETE FROM world
WHERE population > 100000000;
```
