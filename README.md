[![Gitter chat](https://badges.gitter.im/gitterHQ/gitter.png)](https://gitter.im/big-data-europe/Lobby)

# Descripción de los archivos:

Loa archivos anteriores sirvieron para elaborar un ejercicio dentro del salón de clases:

- Se clonó el repositorio de Docker-Hadoop desde GitHub y se descomprimió en una carpeta.

- Se iniciaron los contenedores Docker con docker-compose up -d, lo que levantó un clúster de Hadoop con los servicios necesarios.

- Se accedió al contenedor namenode, que es el nodo maestro de Hadoop, para administrar el sistema de archivos HDFS.

- Se creó la estructura de carpetas en HDFS para alojar los archivos de entrada.

- Se descargaron dos archivos:

  - Un archivo .jar con un ejemplo de MapReduce.

  - Un archivo .txt con el texto de Don Quijote de La Mancha.

- Se copiaron estos archivos al contenedor namenode y se movió el archivo de texto a HDFS.

- Se ejecutó un trabajo de MapReduce, utilizando el ejemplo de recuento de palabras (WordCount).

Se visualizaron los resultados, accediendo a la carpeta de salida en HDFS y exportando el archivo con el recuento de palabras (quijote_wc.txt) a la carpeta del repositorio en la máquina local.

Por último, se detuvieron los contenedores desde Docker Desktop.

# Changes

Version 2.0.0 introduces uses wait_for_it script for the cluster startup

# Hadoop Docker

## Supported Hadoop Versions
See repository branches for supported hadoop versions

## Quick Start

To deploy an example HDFS cluster, run:
```
  docker-compose up
```

Run example wordcount job:
```
  make wordcount
```

Or deploy in swarm:
```
docker stack deploy -c docker-compose-v3.yml hadoop
```

`docker-compose` creates a docker network that can be found by running `docker network list`, e.g. `dockerhadoop_default`.

Run `docker network inspect` on the network (e.g. `dockerhadoop_default`) to find the IP the hadoop interfaces are published on. Access these interfaces with the following URLs:

* Namenode: http://<dockerhadoop_IP_address>:9870/dfshealth.html#tab-overview
* History server: http://<dockerhadoop_IP_address>:8188/applicationhistory
* Datanode: http://<dockerhadoop_IP_address>:9864/
* Nodemanager: http://<dockerhadoop_IP_address>:8042/node
* Resource manager: http://<dockerhadoop_IP_address>:8088/

## Configure Environment Variables

The configuration parameters can be specified in the hadoop.env file or as environmental variables for specific services (e.g. namenode, datanode etc.):
```
  CORE_CONF_fs_defaultFS=hdfs://namenode:8020
```

CORE_CONF corresponds to core-site.xml. fs_defaultFS=hdfs://namenode:8020 will be transformed into:
```
  <property><name>fs.defaultFS</name><value>hdfs://namenode:8020</value></property>
```
To define dash inside a configuration parameter, use triple underscore, such as YARN_CONF_yarn_log___aggregation___enable=true (yarn-site.xml):
```
  <property><name>yarn.log-aggregation-enable</name><value>true</value></property>
```

The available configurations are:
* /etc/hadoop/core-site.xml CORE_CONF
* /etc/hadoop/hdfs-site.xml HDFS_CONF
* /etc/hadoop/yarn-site.xml YARN_CONF
* /etc/hadoop/httpfs-site.xml HTTPFS_CONF
* /etc/hadoop/kms-site.xml KMS_CONF
* /etc/hadoop/mapred-site.xml  MAPRED_CONF

If you need to extend some other configuration file, refer to base/entrypoint.sh bash script.
