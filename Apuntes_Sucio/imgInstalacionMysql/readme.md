# Instalación Mysql Server en Ubuntu


### Abrimos la consola 

### Ejecutamos el comando sudo apt-get update para actualizar la lista de paquetes

  - ![captura01](https://github.com/dam108/ApuntesSQL/blob/master/Apuntes_Sucio/imgInstalacionMysql/img/Captura01.PNG)

### Ejecutamos los siguientes comandos para instalar MariaDB en nuestro equipo
  - ![captura02](https://github.com/dam108/ApuntesSQL/blob/master/Apuntes_Sucio/imgInstalacionMysql/img/Captura02.PNG)
  - ![captura03](https://github.com/dam108/ApuntesSQL/blob/master/Apuntes_Sucio/imgInstalacionMysql/img/Captura03.PNG)
  - ![captura04](https://github.com/dam108/ApuntesSQL/blob/master/Apuntes_Sucio/imgInstalacionMysql/img/Captura04.PNG)
  - ![captura05](https://github.com/dam108/ApuntesSQL/blob/master/Apuntes_Sucio/imgInstalacionMysql/img/Captura05.PNG)
  - ![captura06](https://github.com/dam108/ApuntesSQL/blob/master/Apuntes_Sucio/imgInstalacionMysql/img/Captura06.PNG)

### Empieza el proceso de instalacion y esperamos hasta que acabe
  - ![captura07](https://github.com/dam108/ApuntesSQL/blob/master/Apuntes_Sucio/imgInstalacionMysql/img/Captura07.PNG)
  
### Nos logueamos como root y entramos en el gestor
  - ![captura08](https://github.com/dam108/ApuntesSQL/blob/master/Apuntes_Sucio/imgInstalacionMysql/img/Captura08.PNG)
  
### Creamos la base de datos , en este caso nEspaciales
  - ![captura09](https://github.com/dam108/ApuntesSQL/blob/master/Apuntes_Sucio/imgInstalacionMysql/img/Captura09.PNG)
 
### Aqui podemos empezar a introducir las sentencias para crear las tablas y las Constraints o bien importar un archivo sql con todo ya escrito, en este caso importamos el archivo de la siguiente manera
  - ![captura10](https://github.com/dam108/ApuntesSQL/blob/master/Apuntes_Sucio/imgInstalacionMysql/img/Captura10.PNG)
  
### Entramos en el gestor y mostramos las bases de datos que hay
  - ![captura11](https://github.com/dam108/ApuntesSQL/blob/master/Apuntes_Sucio/imgInstalacionMysql/img/Captura11.PNG)
  
### Le indicamos que queremos unsar nuestra base de datos y que nos enseñe las tablas
  - ![captura12](https://github.com/dam108/ApuntesSQL/blob/master/Apuntes_Sucio/imgInstalacionMysql/img/Captura12.PNG)
  
### Tambien podemos ver una descripcion mas en profundidad de nuestras tablas con el comando desc
  - ![captura13](https://github.com/dam108/ApuntesSQL/blob/master/Apuntes_Sucio/imgInstalacionMysql/img/Captura13.PNG)

> NOTA: El archivo que nEspaciales.sql contiene el siguiente codigo 

```sql
CREATE SCHEMA IF NOT EXISTS nEspaciales;

CREATE TABLE nEspaciales.SERVIZO (
clave_Servizo CHAR(5),
nome_Servizo VARCHAR(40),
PRIMARY KEY (clave_Servizo, nome_servizo)
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

# Desinstalar MariaDB en Linux

1. Para desinstalar normal 
> sudo apt-get remove mariadb-server
2. Para desinstalar mariadb y todos los paquetes dependientes que ya no sean necesarios
> sudo apt-get remove --auto-remove mariadb-server
3. Para realizar una purga e eliminar la información de la configuración
> sudo apt-get purge mariadb-server
4.  Para realizar una purga e eliminar la información de la configuración y todos los paquetes dependientes
> sudo apt-get purge --auto-remove mariadb-server


