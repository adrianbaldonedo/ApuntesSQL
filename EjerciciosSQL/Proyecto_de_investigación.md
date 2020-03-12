# Exercicio DDL 1 Proxectos de investigación
Na Universidade de A Coruña deséxase levar un control sobre os proxectos de investigación que se desenvolven.

Para iso decídese empregar unha base de datos que conteña toda a información sobre os proxectos, departamentos, grupos de investigación e profesores.

Un departamento identifícase polo seu nome (Informática, Enxeñería, etc).

Ten unha sede situada nun determinado campus, un teléfono de contacto e un director, tamén profesor da Universidade de A Coruña.

Dentro dun departamento créanse grupos de investigación. Cada grupo ten un nome único dentro do departamento (pero que pode ser o mesmo en distintos departamentos) e está asociado a unha área de coñecemento (bases de datos, intelixencia artificial, sistemas e comunicacións, etc). Cada grupo ten un líder, tamén profesor da Universidade de A Coruña.

Un profesor está identificado polo seu DNI. Del deséxase saber o nome, tilulación, anos de experiencia en investigación, grupo de investigación no que desenvolve o seu labor e proxectos nos que traballa.

Cada proxecto de investigación ten un nome, un código único, un orzamento, datas de inicio e terminación e un grupo que o desenvolve. Doutra banda, pode estar financiado por varios programas. Dentro de cada programa cada proxecto ten un número asociado e unha cantidade de diñeiro financiada (por exemplo, o proxecto BDE - Bases de Datos Espaciais ten o número 1337 dentro do programa A Solaris e volta que lle financia con 10.000 euros).

Un profesor pode participar en varios proxectos. En cada proxecto incorpórase nunha determinada data e cesa noutra, tendo unha determinada dedicación (en horas á semana) durante ese período.

A partir do esquema relacional proporcionado, implementalo en PostgreSQL.


![Imagen ejercicio proyecto de investigacion](https://github.com/dam108/ApuntesSQL/blob/master/EjerciciosSQL/img/Ejercicios_PDI.PNG)


____________________________________________________________________________

Supuestos sin implemementar: 

La fecha de inicio tiene que ser antes que la fecha de fin esto se implementaria con un **CHECK**
Se podria usar un **ASERT** para comprobar qwue todo el dinero financiado se invierte en el proyecto. ( pero esto no lo se hacer)

Formulas: 

```sql
CREATE SCHEMA proyectoDeInvestigacion;

CREATE DOMAIN proyectoDeInvestigacion.tipo_Codigo CHAR(5);
CREATE DOMAIN proyectoDeInvestigacion.tipo_DNI    CHAR(9);
CREATE DOMAIN proyectoDeInvestigacion.telefono    CHAR(9);
CREATE DOMAIN proyectoDeInvestigacion.nome_valido VARCHAR(30);

CREATE TABLE proyectoDeInvestigacion.SEDE (
nome_Sede proyectoDeInvestigacion.nome_valido PRIMARY KEY,
campus proyectoDeInvestigacion.nome_valido NOT NULL
);

CREATE TABLE proyectoDeInvestigacion.UBICACION (
nome_Sede proyectoDeInvestigacion.nome_valido,
nome_Departamento proyectoDeInvestigacion.nome_valido,
PRIMARY KEY (nome_sede, nome_Departamento)
);

CREATE TABLE  proyectoDeInvestigacion.DEPARTAMENTO (
nome_Departamento proyectoDeInvestigacion.nome_valido PRIMARY KEY,
telefono proyectoDeInvestigacion.telefono NOT NULL,
director proyectoDeInvestigacion.tipo_DNI
-- FK (director)
);

CREATE TABLE proyectoDeInvestigacion.GRUPO (
nome_Grupo proyectoDeInvestigacion.nome_valido,
nome_Departamento proyectoDeInvestigacion.nome_valido,
area proyectoDeInvestigacion.nome_valido NOT NULL,
lider proyectoDeInvestigacion.tipo_DNI,
-- FK lider
PRIMARY KEY (nome_Grupo, nome_Departamento)
);

CREATE TABLE proyectoDeInvestigacion.PROFESOR (
DNI proyectoDeInvestigacion.tipo_DNI PRIMARY KEY,
nome_Profesor proyectoDeInvestigacion.nome_valido NOT NULL,
titulacion VARCHAR(30) NOT NULL,
experiencia INTEGER,
nome_Grupo proyectoDeInvestigacion.nome_valido,
nome_Departamento proyectoDeInvestigacion.nome_valido
-- FK nome_Grupo, nome_Departamento 
);

CREATE TABLE proyectoDeInvestigacion.PARTICIPA (
DNI proyectoDeInvestigacion.tipo_DNI,
codigo_Proxecto proyectoDeInvestigacion.tipo_Codigo,
data_Inicio DATE NOT NULL,
data_cese DATE,
dedicacion INTEGER NOT NULL,
PRIMARY KEY (DNI, codigo_Proxecto),
CHECK( data_ces > data_Inicio)
);

CREATE TABLE proyectoDeInvestigacion.PROXECTO (
codigo_Proxecto proyectoDeInvestigacion.tipo_Codigo PRIMARY KEY,
nome_Proxecto proyectoDeInvestigacion.nome_valido NOT NULL UNIQUE,
orzamento MONEY NOT NULL,
data_Inicio DATE NOT NULL,
data_Fin DATE,
nome_Grupo proyectoDeInvestigacion.nome_valido,
nome_Departamento proyectoDeInvestigacion.nome_valido
);

CREATE TABLE proyectoDeInvestigacion.PROGRAMA (
nome_Programa proyectoDeInvestigacion.nome_valido PRIMARY KEY
);

CREATE TABLE proyectoDeInvestigacion.FINANCIA (
nome_Programa proyectoDeInvestigacion.nome_valido,
codigo_Proxecto proyectoDeInvestigacion.tipo_Codigo,
numero_Proxecto INTEGER NOT NULL,
cantidade_Financianda MONEY NOT NULL,
PRIMARY KEY (nome_Programa, codigo_Proxecto)
);

ALTER TABLE  proyectoDeInvestigacion.UBICACION ADD CONSTRAINT  clave_ajena1_ubicacion 
FOREIGN KEY (nome_Sede) REFERENCES proyectoDeInvestigacion.SEDE (nome_Sede) 
ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE proyectoDeInvestigacion.UBICACION ADD CONSTRAINT clave_ajena2_ubicacion 
FOREIGN KEY (nome_Departamento) REFERENCES  proyectoDeInvestigacion.DEPARTAMENTO (nome_Departamento) 
ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE proyectoDeInvestigacion.DEPARTAMENTO  ADD CONSTRAINT clave_ajena1_departamento 
FOREIGN KEY  ( director ) REFERENCES proyectoDeInvestigacion.PROFESOR (DNI) 
ON DELETE SET NULL ON UPDATE CASCADE;

ALTER TABLE  proyectoDeInvestigacion.GRUPO ADD CONSTRAINT  clave_ajena1_grupo 
FOREIGN KEY ( lider ) REFERENCES proyectoDeInvestigacion.PROFESOR (DNI) 
ON DELETE SET NULL ON UPDATE CASCADE;

ALTER TABLE  proyectoDeInvestigacion.GRUPO ADD CONSTRAINT  clave_ajena2_grupo 
FOREIGN KEY ( nome_Departamento ) REFERENCES proyectoDeInvestigacion.DEPARTAMENTO (nome_Departamento) 
ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE  proyectoDeInvestigacion.PROFESOR ADD CONSTRAINT clave_ajena1_profesor 
FOREIGN KEY (nome_Grupo, nome_Departamento) REFERENCES proyectoDeInvestigacion.GRUPO (nome_Grupo, nome_Departamento) 
ON DELETE SET NULL ON UPDATE CASCADE ;

ALTER TABLE  proyectoDeInvestigacion.PARTICIPA ADD CONSTRAINT clave_ajena1_participa 
FOREIGN KEY (DNI) REFERENCES  proyectoDeInvestigacion.PROFESOR (DNI) 
ON DELETE RESTRICT ON UPDATE CASCADE;

ALTER TABLE  proyectoDeInvestigacion.PARTICIPA ADD CONSTRAINT clave_ajena2_participa 
FOREIGN KEY (codigo_Proxecto) REFERENCES proyectoDeInvestigacion.PROXECTO (codigo_Proxecto) 
ON DELETE RESTRICT ON UPDATE CASCADE;

ALTER TABLE  proyectoDeInvestigacion.PROXECTO ADD CONSTRAINT clave_ajena1_proxecto 
FOREIGN KEY (nome_Grupo, nome_Departamento) REFERENCES  proyectoDeInvestigacion.GRUPO (nome_Grupo, nome_Departamento) 
ON DELETE SET NULL ON UPDATE CASCADE;

ALTER TABLE  proyectoDeInvestigacion.FINANCIA ADD CONSTRAINT clave_ajena1_financia 
FOREIGN KEY (nome_Programa) REFERENCES  proyectoDeInvestigacion.PROGRAMA (nome_Programa) 
ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE  proyectoDeInvestigacion.FINANCIA ADD CONSTRAINT clave_ajena2_financia 
FOREIGN KEY (codigo_Proxecto) REFERENCES  proyectoDeInvestigacion.PROXECTO (codigo_Proxecto) 
ON DELETE CASCADE ON UPDATE CASCADE;

-- A FOREIGHN KEY de Profesor esta mal. Facer unha nova
-- FOREIGN KEY con B:N M:N

ALTER TABLE proyectoDeInvestigacion.PROFESOR
DROP CONSTRAINT clave_ajena1_profesor;

ALTER TABLE proyectoDeInvestigacion.PROFESOR 
ADD CONSTRAINT clave_ajena1_profesor
FOREIGN KEY (nome_Grupo, nome_Departamento)
REFERENCES proyectoDeInvestigacion.GRUPO (nome_Grupo, nome_Departamento)
ON DELETE SET NULL
ON UPDATE NO ACTION;
```
