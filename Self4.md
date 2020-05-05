# RHadoop

Usando Hadoop desde R. 

1. Iniciamos hadoop : `start-yarn.sh`, `start-dfs.sh`
2. Configuramos variables de entorno en R
   ```R
    Sys.setenv(HADOOP_OPTS="-Djava.library.path=/usr/local/hadoop/lib/native")
    Sys.setenv(HADOOP_HOME="/usr/local/hadoop")
    Sys.setenv(HADOOP_CMD="/usr/local/hadoop/bin/hadoop")
    Sys.setenv(HADOOP_STREAMING="/usr/local/hadoop/share/hadoop/tools/lib/hadoop-streaming-2.6.5.jar")
    Sys.setenv(JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64") 
   ```
3. Cargamos las librerias  
   ```R
    library(rhdfs) # Creamos una conexion a HDFS
    library(rmr2) # Nos permite usar operaciones de MapReduce
    library(plyrmr) #Plyr pero sobre rmr2
    hdfs.init()
   ```

## Descripciones librerias 

### rhdfs

Permite la manipulación de archivos en terminos de lectura, escritura y movimiento. Se pueden categorizar como:  
1. Manipulación:
    ```hdfs.copy, hdfs.move, hdfs.rename, hdfs.delete, hdfs.rm, hdfs.del, hdfs.chown, hdfs.put, hdfs.get```
2. Lectura/Escritura
   ```hdfs.file, hdfs.write, hdfs.close, hdfs.flush, hdfs.read, hdfs.seek, hdfs.tell, hdfs.line.reader, hdfs.read.text.file```
3. Directorios 
   ```hdfs.dircreate, hdfs.mkdir```
4. Utility
   ```hdfs.ls, hdfs.list.files, hdfs.file.info, hdfs.exists```
5. Inicializacion 
   ```hdfs.init, hdfs.defaults```

### rmr2
Garantiza el uso de MapReduce de Hadoop al interior de R. Algunas de las funciones más importantes son:
* `from.dfs` para escribir objetos de R en el HDFS
* `mapreduce` MapReduce a partir de Hadoop Streaming
* `scatter` partir un archivo en multiples partes o combiar multiples partes en un solo archivo
* `keyval` crear o concatenar pares key-value
* `to.map` crear funciones de tipo map-reduce a través de otras funciones 

### plyrmr 
Una coleccion de funciones predefinidas que cubren algunas de las necesidades tradicionales de manipulación de datos. Se pueden dividir como:
1. Manipulación de datos:
   `bind.cols` (agregar columnas), `where`(filtrar filas), `select`(escoger columnas).
2. Resumenes de datos:
   `sample`, `count.cols`, `quantile.cols`, `top.k`
3. Operaciones de conjuntos:
   `union`, `intersection`, `unique`, `merge`