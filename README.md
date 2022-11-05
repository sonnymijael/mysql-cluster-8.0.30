# Instalación y elaboración de un Clúster en MySQL
_Creacion de un cluster mysql con 4 nodos y 1 administrador_

## Nodo Administrador (Macos 12)
#### _Vamos a preparar el nodo administrador_
 
 ### Instalacion
 * _Descargamos [Mysql cluster](https://downloads.mysql.com/archives/cluster/) en su version 8.0.30_
 * _Instalamos [Mysql cluster](https://downloads.mysql.com/archives/cluster/)_
_Abrimos la terminal y verificamos si tenemos instalado ejecutando el siguiente comando_
``` shell 
  mysql --version
```
_Como respuesta deberia salir el siguiente mensaje_
> 8.0.30-cluster for macos12 on arm64

_De no ser asi, lee el siguiente articulo [aqui](https://stackoverflow.com/questions/35858052/how-to-fix-command-not-found-mysql-in-zsh)_

### Configuracion del nodo admin
* _Creamos el folder donde vamos a trabajar e ingresamos al folder_
  ```sh
    mkdir /var/lib/mysql-cluster/
    cd /var/lib/mysql-cluster/
  ```
* _Creamos el archivo [config.ini](https://github.com/sonnymijael/mysql-cluster-8.0.30/blob/4d98b0db2caa695413324ef72e40b647c8436803/config.ini)_
  ```sh
    vim config.ini
  ```
> Este documento es un ejemplo
  ```ini
  [ndbd default]
  DataMemory = 100M
  # Cantidad de replicas
  NoOfReplicas = 4 


  # Nodo Administrador (Macbook Air M1)
  [mgm]
  NodeId = 1 
  # ip de dispositivo
  HostName = 192.168.0.0
  # directorio donde se va a guardar el cache
  DataDir = /var/lib/mysql-cluster/ndb-cache/

  # Nodo de datos (Windows 10)
  [ndbd]
  NodeId = 2
  # ip del dispositivo
  HostName = 192.168.0.0
  # directorio donde se guardara el cache
  DataDir = c:\my_cluster\ndb-cache\

  # Nodo mysql 
  [mysqld]
  ```
* _Cargamos la configuracion y iniciamos el nodo administrador_
  ```sh
    sudo ndb_mgmd -f /var/lib/mysql-cluster/config.ini --initial --configdir=/var/lib/mysql-cluster/
  ```
  ![Nodo administrador iniciado](https://user-images.githubusercontent.com/98063818/200135664-50c047ba-cc3b-4a82-a2f7-652426287280.jpeg)

  
* _Con esto estara iniciado el nodo administrador ingresamos con_ 
  ```sh
    ndb_mgm
  ```

* _Una vez en el panel de administracion vemos verificamos que se haya realizado de forma correcta_
  ```sh
    show
  ```
   > _Ejemplo_
  
   ![Ejemplo](https://user-images.githubusercontent.com/98063818/200135748-41e942ae-c5ae-42d3-93c1-5676a9271578.jpeg)

 
  
## Nodos de Datos (Windows 10)
#### _Vamos a preparar el nodo de datos_
 
 ### Instalacion
 * _Descargamos el zip [Mysql cluster.zip](https://downloads.mysql.com/archives/cluster/) en su version 8.0.30_
 * _Descomprimimos [Mysql cluster.zip](https://downloads.mysql.com/archives/cluster/)_
 * _Renombramos el folder con el nombre de **msyql** y lo movemos a el disco local c:\_
 
 ### Nos conectamos al nodo administrador
 * _Abrimos el cmd_
 * _Ejecutamos el comando_
  > _Remplazar la ip 192.168.0.0:1186 por la ip de su administrador_  
  
  ```cmd
    c:\mysql\bin\ndbd -c 192.168.0.0:1186 
  ```
  
  > _Nodo 3 conectado a administrador_
  
  ![Nodo conectado](https://user-images.githubusercontent.com/98063818/200135590-adeca1b8-9fff-4b8d-a333-f17e76d6b1ea.jpeg)
  
  > _Nodo administrador con todos los nodos de datos conectados_
  
  <img width="857" alt="Captura de Pantalla 2022-10-20 a la(s) 7 33 33" src="https://user-images.githubusercontent.com/98063818/200134804-111ccf5a-4a5b-4403-8687-50896cccbecc.png">
  
## Nodos mysqld
#### _Ya lo tenemos instalado_ 

 ### Nos conectamos a la red del mysql cluster
 * _Abrimos otra terminal en el nodo de datos_ 
 * _Creamos el folder en el que vamos a trabajar de nombre my_cluster_
 * _Creamos el archivo [my.cnf](https://github.com/sonnymijael/mysql-cluster-8.0.30/blob/59c3b72ae436045c3ac35a4fbd3b70855002c2e5/my.cnf)_
  
  ```sh    
   [mysqld]
   ndbcluster
   # Definimos el puerto al que nos vamos a conectar
   port=4002
   # Ip del nodo administrador
   ndb-connectstring='host=192.168.0.0'
   [mysql_cluster]
   # Ip del nodo administrador
   ndb-connectstring='host=192.168.0.0'
 ```
 * _Iniciamos **mysqld**_
  ```sh
    c:\mysql\bin\mysqld --initialize-insecure --console
  ```
 * _Cargamos la configuracion_
  ```sh
    c:\mysql\bin\mysqld --defaults-file=c:\my_cluster\my.cnf --console
  ```
  * _Estamos conectados al cluster_
  
  ![Conectados al cluster](https://user-images.githubusercontent.com/98063818/200136674-10e2da98-2dc6-4ffc-a7a2-77d14156be57.jpeg)
  
  #### En el nodo administrador
  _Todos los nodos msyql y nodos de datos conectados_
  
  ![Todos los nodos conectados](https://user-images.githubusercontent.com/98063818/200136770-b846937f-2745-424f-b516-365513add64a.jpeg)
  
  #### En el nodo mysql
  _Entramos en mysql y nos conectamos al puerto del cluster_
  
  ```sh
    c:\mysql\bin\mysql -u root -P 4002
  ```
  
  ![Conectado](https://user-images.githubusercontent.com/98063818/200137193-ea77893b-7f45-4ebb-934b-302ec86bc6fc.jpeg)


