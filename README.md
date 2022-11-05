# Mysql Cluster 8.0.30 
_Creacion de un cluster mysql con 4 nodos y 1 administrador_

## Introduccion 🚀
* [Mysql cluster](https://downloads.mysql.com/archives/cluster/)

## Nodo Administrador
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
* _Con esto estara iniciado el nodo administrador ingresamos con_ 
  ```sh
    ndb_mgm
  ```

* _Una vez en el panel de administracion vemos verificamos que se haya realizado de forma correcta_
  ```sh
    show
  ```
   > Ejemplo
   <img width="857" alt="Captura de Pantalla 2022-10-20 a la(s) 7 33 33" src="https://user-images.githubusercontent.com/98063818/200134804-111ccf5a-4a5b-4403-8687-50896cccbecc.png">
  
