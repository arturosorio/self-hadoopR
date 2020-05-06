# Setup VM y Rhadoop

## Setup VM

1. Obtención maquina virtual :  http://login.hpc.fs.uni-lj.si/download/
2. Setup Maquina Virtual:
    1. Usando [VirtualBox](https://www.virtualbox.org/wiki/Downloads) instalamos la imagen de la maquina virtual obtenida anteriormente
    2. Durante la importación de la imagen debemos proveer como minimo **4GB** de Ram y más de una CPU
    3. Iniciamos la maquina virtual desde el boton de start y entramos por el usuario **hdcluster** con la constraseña **hadoop**
3. Los servicios de hadoop no inician por defecto por lo que debemos lanzarlos desde la terminal (parte inferior izquierda) con `start-dfs.sh` y `start-yarn.sh`

## Testing R

Para probar que R funciona correctamente, luego de inicializar hadoop (paso 3 anterior) podemos usar la terminal para lanzar R:  `rstudio &`.  
En R debemos crear un script (init.R) para configuar las variables de entorno que usaremos: 
```R 
{init.R}
Sys.setenv(HADOOP_OPTS="-Djava.library.path=/usr/local/hadoop/lib/native")
Sys.setenv(HADOOP_HOME="/usr/local/hadoop")
Sys.setenv(HADOOP_CMD="/usr/local/hadoop/bin/hadoop")
Sys.setenv(HADOOP_STREAMING="/usr/local/hadoop/share/hadoop/tools/lib/hadoop-streaming-2.6.5.jar")
Sys.setenv(JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64") 
```

Luego, probamos la instación de Rhadoop ejecutando los siguientes comandos: 
```R
library(rhdfs)
library(rmr2)
hdfs.init()
```

Finalmente, tendremos algunos warnings que no comprometen el uso. 

Al momento de terminar debemos detener hadoop: `stop-yarn.sh`, `stop-dfs.sh`
 
