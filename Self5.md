# Ejemplo RHadoop

Luego de haber ejecutado los pasos de setup, podemos proceder con el siguiente ejemplo:

1. Generemos datos (Imaginemos que tenemos n personas con el tiempo promedio en que camina en un día y la ciudad):
   ```R
    N <- 1000000
    X <- rnorm(N,50,5)
    Y <-sample(c("a","b","c","d","e"), 1000, replace=TRUE)
    Data <- data.frame(X,Y)
    colnames(Data)=c("tiempocaminar","ciudad")
   ```
2. Podemos enviar los datos al HDFS mediante:
   ```R
    to.dfs(Data, "walking",format="native")
    ``` 
    y recuperarlos con 
    ```R
    smalldata=from.dfs("/walking")
    ```
3. Podemos observar su extructura en HDFS usando `hdfs fsck \walking` en la terminal 
    <p align="center">
    <img img width="800" height="200" src="img\walking.jpg" >
    </p>
Vemos que no se descompone entre bloques ya que no es tan grande y además tenemos un solo datanode en la MV. 

Hagamos un procedimiento sencillo usando map reduce: ¿Cuantas personas hay por ciudad?
El resultado de R es:

```R
T <- table(walking$val[, "ciudad"])
``` 

y el resultado es: 
| a      | b      | c      | d      | e      |
| ------ | ------ | ------ | ------ | ------ |  
| 195000 | 196000 | 200000 | 214000 | 195000 |

Ahora, usando MapReduce:

En primer lugar, definimos el Map: 
```R
mapper <- function(., X){
    t <- table(X[, "ciudad"])
    key <- names(t)
    keyval(k, list(t))
}
```
Seguimos con el reduce: 
```R
reducer <- function(k, A){
    S <- Reduce('+', A)
    keyval(k, list(S))
}
```

Y finalmente ejecutamos el trabajo sobre hadoop y devolvemos el resultado
```R
n_por_ciudad<-from.dfs(
    mapreduce(
    input = "/caminar",
    map = mapper,
    reduce = reducer))
```



Resultado: 
    <p align="center">
    <img img width="400" height="500" src="img\example_1.jpg" >
    </p>


