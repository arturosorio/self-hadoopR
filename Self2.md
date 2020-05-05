## Basic Hadoop filesystem commands lines

1. Inicializar hadoop file system: `start-dfs.sh` y `start-yarn.sh`
2. Interacturamos con el HDFS mediante `hadoop fs`, por ejemplo, para listar todos los archivos en el HDF: `hadoop fs -ls`
3. Creamos nuevos archivos en HDFS con `hadoop fs -mkdir {example}`
4. Podemos copiar archivos desde directorios locales al HDFS con `hadoop fs  -copyFromLocal {source_path} {destination_path}` 
5. Copiar desde archivos de HDFS  `hadoop fs -copyToLocal {source_path} {destination_path}`
6. Optener ayuda ``hadoop fs -help`



## Hands On

Usaremos el siguiente [dataset](https://bscw.lecad.fs.uni-lj.si/pub/bscw.cgi/d226569/Term_frequencies_sentence-level_lemmatized_utf8.csv) con explicaciones adicionales [aquí](https://github.com/19Joey85/Sentiment-annotated-news-corpus-and-sentiment-lexicon-in-Slovene/tree/master/Automatically%20sentiment%20annotated%20Slovenian%20news%20corpus%20AutoSentiNews%201.0) 

### Exploración
* Descargamos el dataset en algún lugar dentro de la MV.
* Lo enviamos al HDFS: `hadoop fs -copyFromLocal PATH/TO/Term_frequencies_sentence-level_lemmatized_utf8.csv`
* Listamos la segunda, tercera y cuarta columna : `hadoop fs -cat Term_frequencies_sentence-level_lemmatized_utf8.csv | awk -F ";" '{print $2 "," $3 "," $4}' | head`
<<<<<<< HEAD
    ![resultados](img\hadoop_cat_awk.JPG)
=======
    ![resultados](img\hadoop_cat_awk.jpg)
>>>>>>> ce0f982d875cbc7ebb292bf5c9dea32eeb4e3da6
* Podemos enviar directamente el resultado al HDFS a tráves de los pipes : 
  ```
   hadoop fs -cat Term_frequencies_sentence-level_lemmatized_utf8.csv \
   | awk -F ";" '{print $2 ";" $3 ";" $4}' \
   | hadoop fs -put - results.txt
  ```
  
## HDFS
### Ventajas
* Best data pattern -> Write once, read many times 
* Hadoop can run on commodity hardware 
* Failure torelant

### Desventajas
* Low-latency data access -> Optimized for high throughput of data
* Lots of small files -> NameNode holds file system metadata in memory (Directory of the file ~ 150 bytes)
* Multiple file writers, arbitrary file modifications -> Writes at the end of the file, in append only fashion 