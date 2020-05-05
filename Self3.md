# Introducción a MapReduce

Consiste en el procesamiento de grandes volumenes de datos a traves de algoritmos que se ejectuan de forma paralela y distribuida en N servidores dentro de un Cluster de Hadoop.

Como sugiere el nombre, consta de dos operaciones. La operacion **Map** se encarga, de cierta forma, de asignar labels de tal forma que se construyan grupos que se pueden procesar de forma independiente. Los worker nodes se encargar de procesas los datos teniendo en cuenta los labels. 
<p align="center">
  <img img width="600" height="400" src="img\map_concep.jpg" >
</p>