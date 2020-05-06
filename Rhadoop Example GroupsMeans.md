# Ejemplo RHadoop: Media por Columna Agrupada

1. Aumentemos el Dataset anterior con mediciones para los 5 días de la semana. 
    ```R
    N <- 1000000
    lunes <- rnorm(N,35,5)
    martes <- rnorm(N,60,5)
    miercoles <- rnorm(N,40,5)
    jueves <- rnorm(N,45,5)
    viernes <- rnorm(N,20,5)
    ciudad <-sample(c("a","b","c","d","e"), 1000, replace=TRUE)
    caminar <- data.frame(lunes,martes,miercoles,jueves,viernes,ciudad)
    ```   
2. Nuevamente lo enviamos al HDFS
    ```R
    to.dfs(caminar, "walking_2",format="native")
    ```
Ahora, queremos calcular el promedio de minutos caminados por día por ciudad. 

Definimos el Map
```R
mapper <- function(., X){
    
}
```


