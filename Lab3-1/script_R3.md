# Laboratorio 3-1: Comandos Básicos para HDFS y S3

Este laboratorio incluye los pasos básicos para conectar, verificar, cargar y copiar archivos en HDFS, así como transferir archivos a S3.

---

## Conexión por SSH a la instancia

```bash
ssh -i <your-key-pair.pem> hadoop@<master-public-dns-name>
```

## Verificar directorios en HDFS

```bash
hdfs dfs -ls /
hdfs dfs -ls /user
hdfs dfs -ls /user/hadoop
hdfs dfs -ls /user/hadoop/datasets
```

## Instalación de Git

```bash
sudo yum install git
```




## Clonación del repositorio

```bash
git clone https://github.com/st0263eafit/st0263-242.git
```

## Crear y verificar directorios en HDFS
### Crear directorios

```bash
hdfs dfs -mkdir /user/hadoop/datasets
hdfs dfs -mkdir /user/hadoop/datasets/gutenberg
```

## Verificar archivos cargados

```bash
hdfs dfs -ls /user/hadoop/datasets
hdfs dfs -ls /user/hadoop/datasets/gutenberg
```

## Cargar archivos a HDFS

```bash
hdfs dfs -put /home/ec2-user/st0263-242/bigdata/datasets/gutenberg-small/*.txt /user/hadoop/datasets/gutenberg/
```

## Obtener archivos de HDFS a la máquina local

```bash
hdfs dfs -get /user/hadoop/datasets/gutenberg-small/*.txt ~/mis_datasets/
```

## Copiar archivos a Hive

```bash
hdfs dfs -put /root/st0263-242/bigdata/datasets/gutenberg-small/*.txt /user/hadoop/datasets/gutenberg-small/
```

## Copiar archivos a AWS S3 vía SSH

```bash
hadoop fs -put /root/st0263-242/bigdata/datasets/ s3://aadiazm-datasets/
```
