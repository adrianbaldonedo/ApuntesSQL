# Exercicio DDL 2 Naves espaciais
O Ministerio da Exploración Interplanetaria da Federación Unida de Planetas desexa desenvolver un Sistema de Información para a nave espacial Stanisław Lem 72 que proximamente se lanzará ao espazo.


A nave espacial componse de distintas dependencias, e cada unha delas ten un nome, un código (único para cada dependencia), unha función e unha localización. Cada dependencia está baixo o control dun determinado servizo, identificado por un nome e unha clave.

Todo servizo da nave (Servizo de Operacións, Comando e Control, Seguridade, etc.) ha de estar asignado polo menos a unha dependencia.


Quérese levar ao día unha relación da tripulación da nave. Esta información contén o nome, código, categoría, antigüidade, procedencia e situación administrativa (en servizo, de baixa, etc). Cada tripulante está asignado a unha dependencia que desexa coñecer, así como a cámara na que se aloxa. Unha cámara é unha dependencia que posúe dúas características propias, a súa categoría e a súa capacidade.


Doutra banda, deséxanse coñecer os planetas que visitou cada membro da tripulación e o tempo que permaneceron neles para saber as persoas con quen se pode contar á hora de realizar unha exploración interplanetaria.

De cada planeta coñécese o seu nome e código, a galaxia e coordenadas nas que se atopa. Algúns planetas atópanse poboados por diversas razas, cada unha nunha certa cantidade de individuos. De cada raza almacénase información sobre o nome, poboación total e dimensións medias (altura, anchura, peso).

A partir do esquema relacional proporcionado, implementalo en PostgreSQL.

![Imagen ejercicio Naves Espaciales](https://github.com/dam108/ApuntesSQL/blob/master/EjerciciosSQL/img/Ejercicios_NE.PNG)


________________________________________________________

suponemos que el número de tripulantes no supera la capacidad de las camaras , esto se comprobaría con un **ASSERT**

```sql

CREATE SCHEMA IF EXISTS nEspaciales;

CREATE TABLE nEspaciales.SERVIZO (
clave_Servizo INTEGER,
nome_Servizo VARCHAR(30),
PRIMARY KEY (clave_Servizo, nome_servizo),
CHECK (clave_Servizo > 0)
);

CREATE TABLE nEspaciales.DEPENDENCIA(
codigo_Dependencia INTEGER PRIMARY KEY,
nome_Dependencia VARCHAR(30) NOT NULL UNIQUE,
clave_Servizo INTEGER NOT NULL,
nome_Servizo VARCHAR(30) NOT NULL,
funcion VARCHAR(20),
localizacion VARCHAR(50)
);

CREATE TABLE nEspaciales.CAMARA (
codigo_Dependencia INTEGER PRIMARY KEY,
categoria NOT NULL,
capacidade NOT NULL,
CHECK (capacidade > 0)
);

CREATE TABLE nEspaciales.TRIPULACION (
codigo_Tripulacion INTEGER PRIMARY KEY,
nome_Tripulacion VARCHAR(30) NOT NULL,
codigo_Camara INTEGER NOT NULL,
codigo_Dependencia INTEGER NOT NULL,
categoria VARCHAR(20) NOT NULL,
antiguedade DATE NOT NULL,
procedencia VARCHAR(50) NOT NULL,
adm boolean NOT NULL,
CHECK (antiguedade > 0)
);

CREATE TABLE nEspaciales.VISITA (
codigo_Tripulacion INTEGER,
codigo_Planeta INTEGER,
data_visita DATE,
tempo INTEGER NOT NULL,
PRIMARY KEY (codigo_Tripulacion, codigo_Planeta, data_visita)
);

CREATE TABLE nEspaciales.PLANETA (
codigo_Planeta INTEGER PRIMARY KEY,
nome_Planeta VARCHAR(30) NOT NULL UNIQUE,
galaxia VARCHAR(30) NOT NULL,
coordenadas VARCHAR(20) NOT NULL UNIQUE
);

CREATE TABLE nEspaciales.HABITA(
codigo_Planeta INTEGER,
nome_Raza VARCHAR(30),
poblacion_Parcial INTEGER NOT NULL,
PRIMARY KEY (codigo_Planeta, nome_Raza),
CHECK (poblacion_Parcial > 0)
);

CREATE TABLE nEspaciales.RAZA (
nome_Raza VARCHAR(30) PRIMARY KEY,
altura DECIMAL NOT NULL,
anchura DECIMAL NOT NULL,
peso DECIMAL NOT NULL, 
poblacion_Total INTEGER NOT NULL
);

ALTER TABLE nEspaciales.DEPENDENCIA ADD CONSTRAINT clave_ajena1_dependencia
FOREIGN KEY (clave_Servizo, nome_Servizo) REFERENCES nEspaciales.SERVIZO (clave_Servizo, nome_Servizo)
ON DELETEE CASCADE ON UPDATE CASCADE;

ALTER TABLE nEspaciales.CAMARA ADD CONSTRAINT clave_ajena1_camara
FOREIGN KEY (codigo_Dependencia) REFERENCES nEspaciales.DEPENDENCIA (codigo_Dependencia)
ON DELETE CASCACDE ON UPDATE CASCADE;

ALTER TABLE nEspaciales.TRIPULACION ADD CONSTRAINT clave_ajena1_tripulacion
FOREIGN KEY (codigo_Camara) REFERENCES nEspaciales.CAMARA(codigo_Dependencia)
ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE nEspaciales.TRIPULACION ADD CONSTRAINT clave_ajena2_tripulacion
FOREIGN KEY (codigo_Dependencia) REFERENCES nEspaciales.DEPENDENCIA(codigo_Dependencia)
ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE nEspaciales.VISITA ADD CONSTRAINT clave_ajena1_visita
FOREIGN KEY (codigo_Tripulacion) REFERENCES nEspaciales.TRIPULACION (codigo_Tripulacion)
ON DELTE CASCADE ON UPDATE CASCADE;

ALTER TABLE nEspaciales.VISITA ADD CONSTRAINT clave_ajena2_visita
FOREIGN KEY (codigo_Planeta) REFERENCES nEspaciales.PLANETA (codigo_Planeta)
ON DELTE CASCADE ON UPDATE CASCADE;

ALTER TABLE nEspaciales.HABITA ADD CONSTRAINT clave_ajena1_habita
FOREIGN KEY (codigo_Planeta) REFERENCES nEspaciales.PLANETA (codigo_Planeta)
ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE nEspaciales.HABITA ADD CONSTRAINT clave_ajena2_habita
FOREIGN KEY (nome_Raza) REFERENCES nEspaciales.RAZA (nome_Raza)
ON DELETE CASCADE ON UPDATE CASCADE;
```
