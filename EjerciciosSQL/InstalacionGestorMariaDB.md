

__________________________________________________________________________________________________

# Instalación MariaDB Server en Ubuntu

### Abrimos la consola 

### Ejecutamos el comando sudo apt-get update para actualizar la lista de paquetes

  - ![captura01](https://github.com/dam108/ApuntesSQL/blob/master/EjerciciosSQL/img/Captura01.PNG)

### Ejecutamos los siguientes comandos para instalar MariaDB en nuestro equipo
  - ![captura02](https://github.com/dam108/ApuntesSQL/blob/master/EjerciciosSQL/img/Captura02.PNG)
  - ![captura03](https://github.com/dam108/ApuntesSQL/blob/master/EjerciciosSQL/img/Captura03.PNG)
  - ![captura04](https://github.com/dam108/ApuntesSQL/blob/master/EjerciciosSQL/img/Captura04.PNG)
  - ![captura05](https://github.com/dam108/ApuntesSQL/blob/master/EjerciciosSQL/img/Captura05.PNG)
  - ![captura06](https://github.com/dam108/ApuntesSQL/blob/master/EjerciciosSQL/img/Captura06.PNG)
  
### Empieza el proceso de instalacion y esperamos hasta que acabe
  - ![captura07](https://github.com/dam108/ApuntesSQL/blob/master/EjerciciosSQL/img/Captura07.PNG)
  
__________________________________________________________________________________________________
 
# Creación de la base de datos nEspaciales
  
### Nos logueamos como root y entramos en el gestor
  - ![captura08](https://github.com/dam108/ApuntesSQL/blob/master/EjerciciosSQL/img/Captura08.PNG)
  
### Creamos la base de datos , en este caso nEspaciales
  - ![captura09](https://github.com/dam108/ApuntesSQL/blob/master/EjerciciosSQL/img/Captura09.PNG)
 
### Aqui podemos empezar a introducir las sentencias para crear las tablas y las Constraints o bien importar un archivo sql con todo ya escrito, en este caso importamos el archivo de la siguiente manera
  - ![captura10](https://github.com/dam108/ApuntesSQL/blob/master/EjerciciosSQL/img/Captura10.PNG)
  
### Entramos en el gestor y mostramos las bases de datos que hay
  - ![captura11](https://github.com/dam108/ApuntesSQL/blob/master/EjerciciosSQL/img/Captura11.PNG)
  
### Le indicamos que queremos unsar nuestra base de datos y que nos enseñe las tablas
  - ![captura12](https://github.com/dam108/ApuntesSQL/blob/master/EjerciciosSQL/img/Captura12.PNG)
  
### Tambien podemos ver una descripcion mas en profundidad de nuestras tablas con el comando desc
  - ![captura13](https://github.com/dam108/ApuntesSQL/blob/master/EjerciciosSQL/img/Captura13.PNG)

> NOTA: El archivo de nEspaciales.sql contiene el siguiente codigo 

[Gist con el código sql de la base de datos naves espaciales](https://gist.github.com/dam108/0c0eb886708765cb92d0cfd01aaf9310#file-nespaciales-sql)

__________________________________________________________________________________________________

# Creación de la base de datos proyecto de investigación

### Entramos en el gestor y creamos la base de datos proyecto de investigación
  - ![captura14](https://github.com/dam108/ApuntesSQL/blob/master/EjerciciosSQL/img/Captura14.PNG)
  
### Importamos el archivo sql con las sentencias para la creación de las tablas, entramos en el gestor y comprobamos que se crearon correctamente
  - ![captura15](https://github.com/dam108/ApuntesSQL/blob/master/EjerciciosSQL/img/Captura15.PNG)

> NOTA: El archivo de proyectoDeInvestigacion.sql contiene el siguiente codigo

[Gist con el código sql de la base de datos proyecto de investigación](https://gist.github.com/dam108/f8101d74bb39870132f5843920477d7a#file-proyectodeinvestigacion-sql)

__________________________________________________________________________________________________

# Desinstalar MariaDB en Linux

1. Para desinstalar normal 
> sudo apt-get remove mariadb-server
2. Para desinstalar mariadb y todos los paquetes dependientes que ya no sean necesarios
> sudo apt-get remove --auto-remove mariadb-server
3. Para realizar una purga e eliminar la información de la configuración
> sudo apt-get purge mariadb-server
4.  Para realizar una purga e eliminar la información de la configuración y todos los paquetes dependientes
> sudo apt-get purge --auto-remove mariadb-server


