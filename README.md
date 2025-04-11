# Ejemplos de MapReduce

✅ Requisitos:
- Docker instalado y funcionando.
- Clonado el repositorio docker-hadoop.
- Archivo hadoop-examples-0.20.205.0.jar descargado.
- Archivo puzzle1.txt (el Sudoku a resolver) descargado.

# Levantar el nodo de Hadoop
Desde la carpeta donde clonaste el repositorio docker-hadoop, ejecuta:
  `docker-compose up -d`
Esto levantará los servicios de Hadoop, incluyendo el namenode.

# Ejecutar el MapReduce de la tarea 3 (Sudoku)
1. Mover los archivos al contenedor
   `docker cp hadoop-examples-0.20.205.0.jar namenode:/tmp`  
   `docker cp puzzle1.txt namenode:/tmp`
Asegúrate de que el nombre del archivo de texto sea puzzle1.txt.

2. Ingresar al contenedor
   `docker exec -it namenode bash`

3. Ejecutar el script MapReduce
  `cd /tmp`    
  `hadoop jar hadoop-examples-0.20.205.0.jar sudoku puzzle1.txt`

Para guardar la salida en un archivo:
  `hadoop jar hadoop-examples-0.20.205.0.jar sudoku puzzle1.txt > solucion_puzzle1.txt`

4. Salir del contenedor
   `exit`

5. Copiar la solución al host
   `docker cp namenode:/tmp/solucion_puzzle1.txt .`

# Ejecutar el MapReduce de la tarea 2 (Contar palabras de un libro) 
En este caso estaba en otra computadora, donde tienen el libro "cronicas_de_una_muerte_anunciada.txt" entonces:

1. Descargar y mover los archivos
Descarga el archivo `.jar` desde Maven: 👉 `hadoop-mapreduce-examples-2.7.1-sources.jar`

Nos aseguramos de tener el archivo *Carroll, Lewis - Alicia En El País De Las Maravillas.txt* en la carpeta local.

Mueve los archivos al contenedor: 
* `docker cp hadoop-mapreduce-examples-2.7.1-sources.jar namenode:/tmp`
* `docker cp Carroll, Lewis - Alicia En El País De Las Maravillas.txt namenode:/tmp`

2. Ingresar al contenedor *namenode*
 `docker exec -it namenode bash`

3. Crear carpeta en HDFS y subir el archivo
`hdfs dfs -mkdir /user/root/input_contador`  
`cd /tmp`  
`hdfs dfs -put cronicas_de_una_muerte_anunciada.txt /user/root/input_contador`

4. Ejecutar el programa *WordCount*
`hadoop jar hadoop-mapreduce-examples-2.7.1-sources.jar org.apache.hadoop.examples.WordCount input_contador output_contador`

5. Guardar el resultado a un archivo .txt
`hdfs dfs -cat /user/root/output_contador/part-r-00000 > /tmp/cronicas_wc.txt`  
`exit`  
`docker cp namenode:/tmp/cronicas_wc.txt .`
Ahora tienes el archivo con el conteo de palabras en tu carpeta base.

🛑 Apagar el nodo de Hadoop: `docker-compose down`

📂 Ver los resultados
Abre el archivo `solucion_puzzle1.txt` (o el archivo correspondiente a otra tarea) con cualquier editor de texto para ver la solución generada por MapReduce.

🔗 Referencias
* [Documentacion de Apache para Dancing class](https://hadoop.apache.org/docs/stable/api/org/apache/hadoop/examples/dancing/package-summary.html)
* [Archivos de Docker Hadoop](https://github.com/big-data-europe/docker-hadoop)
* [Tutorial más detallado](https://miguelevangelista.gitbook.io/herramientasavanzadas/ejemplos-de-mapreduce/resolver-sudoku)
