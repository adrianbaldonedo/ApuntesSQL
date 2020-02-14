# Apuntes SQL segunda parte.

## SubLenguajes SQL <a name="subsql"></a>

1. DQL(DATA QUARY LANGUAGE) -- SELECT
1. DML (DATA MANIPULATION LANGUAGE) -- Opera sobre los datos -- INSERT, UPDATE, DELETE
1. DDL (DATA DEFINITION LANGUAGE) -- Opera sobre la los objetos de la base de datos (sobre la estructura) -- CREATE, ALTER, DROP
1. TCL( TRANSACCION CONTROL LANGUAGE) -- TRANSACCIÓN -> COMPOSICIÓN DE DISTINTAS INSTRUCCIONES SQL -- COMMIT, ROLLBACK, SAFE POINT
1. DCL (DATA CONTROL LANGUAGE) -- DAR PERMISOS -- GRANT, REVOKE
1. SCL ( SESION CONTROL LANGUAGUE) -- MANEJAR DINÁMICAMENTE PROPIEDADES DE UNA SESIÓN DE USUARIO -- ALTER SESSION

> EL DQL , DML y DDL son el nucleo de SQL

# DDL: Create
La notación **CREATE** se utiliza para crear objetos de una base de datos o una base de datos en si y tambien para crear usuarios.

Sintaxis:
```SQL
 CREATE ( SCHEMA | DATABASE )  
  [ IF NOT EXIST ] <nombre_bd> 
  [ CHARACTER SET ] <nombre_setCaracteres>
  [ COLLATE ] collation_name;
  
```
```sql
CREATE TABLE nombre_tabla (
 COLUMNA TIPO_DATO
 id INTEGER [ NOT NULL ] PRIMARY KEY,
 nombre NCHAR(50),
 apellido NCHAR(200),
 fecha (DATE | TIME | TIMESTAMP)
);
TIPOS DE CADENAS (VARCHAR | CHAR | NCHAR | TEXT | NCHAR VARYING)
```
``` sql
CREATE USER username IDENTIFIED BY password IDENTIFIED WITH auth_plugin;
```

***

### DDL: CREATE -- CONSTRAINT
**Restriccion de Clave primaria**

[**CONSTRAINT** <nombre-restriccion>]
 
 PRIMARY KEY (<atributos>)
 
 Esta opcion se usa cuando la clave primaria se compone de dos columnas
 
 ```sql
 CONSTRAINT PK_WORLD
 PRIMARY KEY (name, continent)
 ```
Esta es la otra opcion pero solo se usa cuando hay una columna como clave primaria
```sql
name char(80) PRIMARY KEY
```
**Restriccion de clave ajena**
Se refencia la tabla de la clave ajena y los atributos de la tabla de la clave ajena.
 

[CONSTRAINT <nombre-restriccion>]
 
 FOREIGN KEY (<atributos>)
 
 REFERENCES <nombre-tabla-referecniada>
 
 [(<atributos-referenciados>)]
 
 [ON DELETE CASCADE | NO ACTION | SET NULL | SET DEFAULT]
 
 [ON UPDATE CASCADE | NO ACTION | SET NULL | SET DEFAULT]
 
 por defecto se pone No ACTION pero la mejor opcion casi siempre SET NULL
 
