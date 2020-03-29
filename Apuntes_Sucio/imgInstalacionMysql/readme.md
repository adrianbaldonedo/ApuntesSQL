# Instalación Mysql Server en Ubuntu

1. Abrimos la consola 
1. Ejecutamos el comando sudo apt-get update para actualizar la lista de paquetes

1. Ejecutamos el comando sudo apt-get install mysql-server para instalar el mysql server con el que trabajaremos

1. Ejecutamos Mysql con el comando sudo mysql y ya podemos trabajar

1. Creamos las BBDD de el Ejercicio naves espaciales con este codigo

```sql
CREATE SCHEMA IF NOT EXISTS nEspaciales;

CREATE TABLE nEspaciales.SERVIZO (
clave_Servizo CHAR(5),
nome_Servizo VARCHAR(40),
PRIMARY KEY (clave_Servizo, nome_servizo),
);

CREATE TABLE nEspaciales.DEPENDENCIA(
codigo_Dependencia CHAR(5) PRIMARY KEY,
nome_Dependencia VARCHAR(40) NOT NULL UNIQUE,
clave_Servizo CHAR(5) NOT NULL,
nome_Servizo VARCHAR(40) NOT NULL,
funcion VARCHAR(20),
localizacion VARCHAR(20)
);

CREATE TABLE nEspaciales.CAMARA (
codigo_Dependencia CHAR(5) PRIMARY KEY,
categoria VARCHAR(40) NOT NULL,
capacidade INTEGER NOT NULL,
CHECK (capacidade > 0)
);

CREATE TABLE nEspaciales.TRIPULACION (
codigo_Tripulacion CHAR(5) PRIMARY KEY,
nome_Tripulacion VARCHAR(40) NOT NULL,
codigo_Camara CHAR(5) NOT NULL,
codigo_Dependencia CHAR(5) NOT NULL,
categoria VARCHAR(40) NOT NULL,
antiguedade INTEGER NOT NULL,
procedencia VARCHAR(50) NOT NULL,
adm boolean NOT NULL,
CHECK (antiguedade > 0)
);

CREATE TABLE nEspaciales.VISITA (
codigo_Tripulacion CHAR(5),
codigo_Planeta CHAR(5),
data_visita DATE,
tempo INTEGER NOT NULL,
PRIMARY KEY (codigo_Tripulacion, codigo_Planeta, data_visita)
);

CREATE TABLE nEspaciales.PLANETA (
codigo_Planeta CHAR(5) PRIMARY KEY,
nome_Planeta VARCHAR(40) NOT NULL UNIQUE,
galaxia VARCHAR(40) NOT NULL,
coordenadas VARCHAR(20) NOT NULL UNIQUE
);

CREATE TABLE nEspaciales.HABITA(
codigo_Planeta CHAR(5),
nome_Raza VARCHAR(40),
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
ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE nEspaciales.CAMARA ADD CONSTRAINT clave_ajena1_camara
FOREIGN KEY (codigo_Dependencia) REFERENCES nEspaciales.DEPENDENCIA (codigo_Dependencia)
ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE nEspaciales.TRIPULACION ADD CONSTRAINT clave_ajena1_tripulacion
FOREIGN KEY (codigo_Camara) REFERENCES nEspaciales.CAMARA(codigo_Dependencia)
ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE nEspaciales.TRIPULACION ADD CONSTRAINT clave_ajena2_tripulacion
FOREIGN KEY (codigo_Dependencia) REFERENCES nEspaciales.DEPENDENCIA(codigo_Dependencia)
ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE nEspaciales.VISITA ADD CONSTRAINT clave_ajena1_visita
FOREIGN KEY (codigo_Tripulacion) REFERENCES nEspaciales.TRIPULACION (codigo_Tripulacion)
ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE nEspaciales.VISITA ADD CONSTRAINT clave_ajena2_visita
FOREIGN KEY (codigo_Planeta) REFERENCES nEspaciales.PLANETA (codigo_Planeta)
ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE nEspaciales.HABITA ADD CONSTRAINT clave_ajena1_habita
FOREIGN KEY (codigo_Planeta) REFERENCES nEspaciales.PLANETA (codigo_Planeta)
ON DELETE CASCADE ON UPDATE CASCADE;

ALTER TABLE nEspaciales.HABITA ADD CONSTRAINT clave_ajena2_habita
FOREIGN KEY (nome_Raza) REFERENCES nEspaciales.RAZA (nome_Raza)
ON DELETE CASCADE ON UPDATE CASCADE;

```


