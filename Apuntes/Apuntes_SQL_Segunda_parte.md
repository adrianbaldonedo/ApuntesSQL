# Apuntes SQL - DDL y DML.

## Indice  <a name="INDICE"></a>
1. [SubLenguajes SQL](#subsql)
1. [Dominios](#tiposDominios)
1. [DDL: Create](#ddlCreate)
1. [DDL: CREATE -- CONSTRAINT](#ddlCreateContraint)
    - [RestricciÓn de clave primaria](#ConstraintPK)
    - [RestricciÓn de clave ajena](#ConstraintFK)
    - [RestricciÓn de Unicidad](#ConstraintUnique)
    - [RestricciÓn de verificación](#ConstraintCheck) 
1. [DDL : DROP](#ddlDrop)
    - [DROP SCHEMA](#ddlDropSchema)
    - [DROP TABLE](#ddlDropTable)
1. [DDL : ALTER TABLE](#ddlAlter)
    - [ADD Y DROP COLUMN](#add_drop_col)
1. [DML INSERT - UPDATE - DELETE](#dmlInsUODEL)
    - [INSERT](#dmlInsert)
    - [UPDATE](#dmlUpdate)
    - [DELETE](#dmlDelete)
__________________________________________________________________________


## SubLenguajes SQL <a name="subsql"></a>

> EL DQL , DML y DDL son el nucleo de SQL

| Notación | Nombre | Contenido |
| -------- | ------ | --------- |
| DQL | DATA QUERY LANGUAGE | SELECT |
| DML | DATA MANIPULATION LANGUAGE | Opera sobre los datos -- INSERT, UPDATE, DELETE |
| DDL | DATA DEFINITION LANGUAGE | Opera sobre la los objetos de la base de datos (sobre la estructura) -- CREATE, ALTER, DROP |
| TCL | TRANSACCION CONTROL LANGUAGE | TRANSACCIÓN -> COMPOSICIÓN DE DISTINTAS INSTRUCCIONES SQL -- COMMIT, ROLLBACK, SAFE POINT |
| DCL | DATA CONTROL LANGUAGE | DAR PERMISOS -- GRANT, REVOKE |
| SCL | SESION CONTROL LANGUAGUE | MANEJAR DINÁMICAMENTE PROPIEDADES DE UNA SESIÓN DE USUARIO -- ALTER SESSION |

[VOLVER AL INDICE](#INDICE)

# TIPOS DE DATOS (DOMINIOS) <a name="tiposDominios"></a>

> Si al escoger el tipo de datos utilizamos el que mas se ajuste, las bases de datos trabajan mas rápido

> Escoger el mejor tipo de datos = mejor rendimiento

Lista de Dominios mas comunes ( en negrita los que utilizaremos).

| Tipo de Dato | Descripción | Rango |
| ------------ | ----------- | ------|
| SMALLINT | numeros enteros cortos | -32768 to +32767 |
| **INTEGER** | **números enteros, 4bytes** | **-2147483648 to +2147483647** |
| BIGINT |  enteros largos, 8 bytes | -9223372036854775808 to 9223372036854775807 |
| **DECIMAL** | **números decimales** | **hasta 131072 de dígitos antes de la coma y hasta 16383 dígitos decimales despues de la coma** |
| **REAL** | **números decimales pequeños** | **6 dígitos decimales** |
|  |  |  |
| **MONEY** | **monedas, 8 bytes** | **-92233720368547758.08 to +92233720368547758.07** |
|  |  |  |
| **TEXT** | **texto** | **longitud ilimitada variable** |
| **CHAR(N)** | **N caracteres, 1 byte por carácter** | **Longitud fija** |
| **VARCHAR(N)** | **cadena de N caracteres** | **Longitud variable** |
|  |  |  |
| **DATE** | **fechas** | **4 bytes** |
| **TIME** | **hora del día** | **8 bytes** |
| **TIMESTAMP** | **hora del día y fecha del día** |  |
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

**Si queremos crear un dominio utilizamos la notacion CREATE DOMAIN <nombre_dominio>;**
```sql
CREATE DOMAIN tipo_Codigo CHAR(5);
CREATE DOMAIN tipo_DNI    CHAR(9);
CREATE DOMAIN nome_valido VARCHAR(30);
```

[VOLVER AL INDICE](#INDICE)

# DDL: Create <a name="ddlCreate"></a>
La notación **CREATE** se utiliza para crear objetos de una base de datos o una base de datos en si y también para crear usuarios.

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

### Para crear una nueva tabla dentro de una base de datos utilizamos la notación CREATE TABLE

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
[VOLVER AL INDICE](#INDICE)

### DDL: CREATE -- CONSTRAINTS <a name="ddlCreateContraint"></a>

**Restricción de Clave primaria**<a name="ConstraintPK"></a> 

Cuando creamos una tabla tenemos que especificar la clave primaria de la tabla , la cual no puede ser null , ni repetirse. Para esto utilizamos notación **CONSTRAINT**.

Hay varias maneras de utilizar esta formula , cuando la clave es una sola utilizamos la siguiente notación:

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

La siguiente opción se usa cuando la clave primaria se compone de dos columnas

Sintaxis: 
```sql
PRIMARY KEY (<nombre_clave1>, <nombre_clave2>)
```
Ejemplo:
```sql
CREATE TABLE proyectoDeInvestigacion.UBICACION(
nombre_Dep VARCHAR(50),
nombre_Sede VARCHAR(50),
PRIMARY KEY (nombre_Dep, nombre_Sede)
);
```
[VOLVER AL INDICE](#INDICE)

**Restricción de clave ajena** <a name="ConstraintFK"></a>

En las tablas también tenemos que indicar cuales son las claves ajenas e indicar a que columna de otra tabla que referencia.

Se referencia la tabla de la clave ajena y los atributos de la tabla de la clave ajena.
 
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
> On Delete --> por defecto se pone No ACTION pero la mejor opción casi siempre SET NULL
 
> On Update --> lo mas propio sera usar la modificación en CASCADE

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
[VOLVER AL INDICE](#INDICE)

**Restricción de unicidad** <a name="ConstraintUnique"></a>

> Para las claves secundarias ( los descartes de claves primarias ) se usa la restricción de unicidad **UNIQUE**

> Se usa en las claves ajenas por que son descartes de clave primarias pero se tienen que seguir guardando de manera que no se pueda tener dos claves ajenas con los mismos datos.

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
[VOLVER AL INDICE](#INDICE)

**Restricción de verificación** <a name="ConstraintCheck"></a>

>La notación **CHECK** es como el **WHERE** , permite hacer predicados.

> El predicado tiene que hacerse sobre los atributos

> Sirve como mecanismo para ejecutar inmediatamente o que se espere a un momento posterior . cada vez que se intenta algo del DML ( insertar un nuevo registro ,modificar o borrar).

> Solo se permite la actualización borrado o inserción cuando el predicado devuelve true.

> El valor determinado es no aplazable. (NOT DEFERRABLE INITIALLY INMEDIATE)

> La otra opción seria posponer (DEFERRABLE INITIALLY DEFERRABLE)

> Con el NOT lo que se nos indica que no es aplazable tiene que hacerse en el momento

> Primero se escoje si es aplazable o no, si no es aplazable tiene que se initially inmediate y si es aplazable lo lógico es initially deferrable

> La opción DEFEREABLE solo tiene sentido cuando hacemos "transacciones"

Sintaxis:
```sql
[CONSTRAINT <nombre_de_restricción>]
CHECK predicado (atributos)
[[NOT] DEFERRABLE ]
[INITIALLY INMEDIATE | DEFERABLE]
```

Ejemplo: 
```sql
CHECK ( 
    saldo >= (
    SELECT saldo
    FROM empleado
    WHERE departamento = 'A')
); 
```
[VOLVER AL INDICE](#INDICE)

## DDL : DROP <a name="ddlDrop"></a>

### DROP SCHEMA <a name="ddlDropSchema"></a>

> Si queremos borrar una base de datos utilizamos la notación **DROP SCHEMA** o **DROP DATABASE**

> Con la notación **IF EXISTS** en caso de que la base de datos no exista nos devolvería false y no un error. 

Sintaxis: 
```SQL
DROP SCHEMA 
   [ IF EXISTS ] <nombre-de-la-BD>;
```

Ejemplo:
```sql
DROP SCHEMA IF EXISTS proyectoDeInvestigacion;
```
[VOLVER AL INDICE](#INDICE)

### DROP TABLE <a name="ddlDropTable"></a>

> Si queremos borrar una tabla de una base de datos utilizamos la notación **DROP TABLE**

> Con la notación **RESTRICT** se especifica que una tabla nos e puede borrar si existe alguna dependencia.

> Con la notación **CASCADE** se borra todo.

Sintaxis:

```SQL
DROP TABLE
[IF EXISTS] <nombre-de-la-tabla>
[ CASCADE | RESTRICT ];
```
Ejemplo: 
```sql
DROP TABLE IF EXISTS proyectoDeInvestigacion.DEPARTAMENTO CASCADE;
```
[VOLVER AL INDICE](#INDICE)

## DDL : ALTER <a name="ddlAlter"></a>
### ADD Y DROP COLUMN <a name="add_drop_col"></a>

> Para alterar una tabla utilizamos la notación ALTER TABLE, con ella podemos modificar una columna, añadir restricciones nuevas, borrar restricciones existentes

> A nivel columna: con ADD y con DROP

> A nivel restricction con ADD y con DROP 

> Entonces ALTER table tiene 4 opciones , dos para la columna y dos para la restricción

Sintaxis: 
```sql
ALTER TABLE <nombre-tabla> ADD [COLUMN] <atributo> <tipo-dato> NOT NULL ...
                           DROP COLUMN <atributo> [CASCADE | RESTRICT]
                           ADD <nombre_de_restricción>
                           DROP <nombre_de_restricción>
```
Ejemplos:
```sql
ALTER TABLE proyectoDeInvestigacion.DEPARTAMENTO ADD COLUMN telefono_movil INTEGER;

ALTER TABLE proyectoDeInvestigacion.DEPARTAMENTO DROP COLUMN telefono;

ALTER TABLE proyectoDeInvestigacion.DEPARTAMENTO ADD CHECK(LENGTH(telefono_movil) < 10 );

ALTER TABLE proyectoDeInvestigacion.DEPARTAMENTO DROP CONSTRAINT telefono_check;

```

> NOTA: si no se le da un nombre a las Restricciones se puede comprobar cual es el nombre por defecto con el siguiente comando 
```sql
SELECT *
FROM INFORMATION_SCHEMA.table_constraints
WHERE TABLE_NAME = 'proyectodeinvestigacion.departamento';
```

[VOLVER AL INDICE](#INDICE)

# DML INSERT - UPDATE - DELETE <a name="dmlInsUODEL"></a>

## INSERT <a name="dmlInsert"></a>

> Sirve para insertar una nueva tupla, hay que indicar cuales son las columnas y que valores tienen las columnas

Sintaxis:
```sql
INSERT INTO <nombre_de_la_tabla> [(<atributo_1>, <atributo_2>, ...)]
VALUES (<valor1>, <valor2>, ...) | SELECT ... );
```

> Los atributos si no se indica el orden estes se insertaran en el orden de cuando se creo la tabla.

> En el **SELECT** se tiene que indicar con el mismo número de columnas, mismos dominios(tipo de datos (NCHAR, DATETIME, INTEGER)) que los de esa tabla

> Se puede insertar varias tuplas a la vez de esta manera , justo después del VALUES se separan con comas.

Ejemplo:

```sql
INSERT INTO proyectoDeInvestigacion.DEPARTAMENTOS
VALUES 
( 'Informática', '981725631', 'Juan Pérez'),
( 'Laboratorio', '981778952', 'Pedro Rivas'),
( 'RRHH', '981763984', 'Miguel Otero');
```
[VOLVER AL INDICE](#INDICE)

## UPDATE <a name="dmlUpdate"></a>

> Se utiliza para actualizar las columnas en de cada tupla

Sintaxis: 

```sql
UPDATE <nombre_de_la_tabla>
SET <atributo1> = <valor1>, <columna2> = <valor2>, ...
[WHERE <predicado>];
```

Ejemplo:
```sql
UPDATE world
SET name='España', continent ='África'
WHERE name='spain';
```
> En este ejemplo se actualizan todas las tuplas de la tabla mundo donde el nombre sea spain, y le ponemos de nombre España y el continente África. 

[VOLVER AL INDICE](#INDICE)

## DELETE <a name="dmlDelete"></a>

> Se utiliza la notación **DELETE** para borrar tuplas de una tabla

> La notación **WHERE** es opcional pero si no se usa borramos todo el contenido de la tabla.

Sintaxis:

```sql
DELETE FROM <nombre_de_la_tabla>
[WHERE <predicado>]*;
```
Ejemplo: 
```sql
DELETE FROM world
WHERE population > 100000000;
```
> Borramos todas las tuplas de la tabla world donde la población sea mayor que cien millones de personas.

[VOLVER AL INDICE](#INDICE)
