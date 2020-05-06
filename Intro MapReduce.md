# Introducción a MapReduce

Consiste en el procesamiento de grandes volumenes de datos a traves de algoritmos que se ejectuan de forma paralela y distribuida en N servidores dentro de un Cluster de Hadoop.

Como sugiere el nombre, consta de dos operaciones. La operacion **Map** se encarga de asignar labels de modo que se construyan grupos que serán procesados independientemente. Los worker nodes se encargan de procesar los datos teniendo en cuenta los labels. 
<p align="center">
  <img img width="600" height="400" src="img\map_concep.jpg" >
</p>

La operación  **Reduce** se ejecuta una vez por cada label asignado usando los resultados de los worker nodes. 
<p align="center">
  <img img width="600" height="400" src="img\reduce_concep.jpg" >
</p>


## Ilustración 

Retomemos el dataset *Term_frequencies_sentence-level_lemmatized* usado anteriormente. Este se compone de la ocurrencia de una palabra particular en un corpus de texto. 
La primera columna es la palabra, la segunda columna el numero de ocurrencias de esa palabra en un contexto positivo, la tercera en un contexto neutral y la cuarta la ocurrencia en un contexto negativo. La quinta columna carece de interés. 

Deseamos saber cuales palabras son las 2 palabras de mayor ocurrencia en cada uno de los contextos. Para esto debemos organizar de forma descendente por las columnas. 
Para resolver este problema usando MapReduce en primer lugar debemos crear el Map donde divimos los datos por columna.

1.  Map
   
   ```(column_id, data), donde column_id ∈ {1,2,3} y data es una lista de la forma [number of occurrences, word]```
 
Tomemos por ejemplo, la primer fila `biti;74061;145951;37957;Va-n` y la convertimos en: 

    1 (key1, value1) -> (1, [74061, biti]) 
    2 (key1, value1) -> (2, [145951, biti])
    3 (key1, value1) -> (3, [37957, biti])

En un sentido muy simple podemos pensar que los datos serán repartidos en 3 workers dados los labels y cada worker se encargara de organizar los datos. Sin embargo, en un escenario realista la división es mucho más fragmentada y cada nodo organiza solo una porción de los datos. 

2. Reduce

    ```reduce: (key, list(value)) -> list(key, value) ```

Dado que estamos interesados en hallar las primeras 2 palabras el task reduce toma el input del Map y los organiza en orden descendente por sus labels. Luego, toma el output hasta la 3 linea: 

    1 74061 biti
    1 25816 v 

    2 145951 biti
    2 51246 v

    3 37957 biti
    3 14936 v

