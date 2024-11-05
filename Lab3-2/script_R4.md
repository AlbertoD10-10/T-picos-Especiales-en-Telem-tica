# Laboratorio 3-2: Scripts y Comandos para HDFS y S3 con Hive

Este laboratorio describe los comandos necesarios para la creación y consulta de tablas en Hive, la gestión de datos en HDFS y S3, y el procesamiento de datos usando SQL.

```bash
## Conexión y Configuración de Usuario

Para conectarse como usuario de Hadoop:
- **Username:** `hadoop`
- **Password:** `********`
```

## Ubicación de Archivos de Trabajo en HDFS

Ubicación temporal en HDFS para los archivos `hdi-data.csv` y `export-data.csv`:

## /user/hadoop/datasets/onu/hdi

```bash
## Gestión de Tablas y Consultas en Hive

### 1. Crear la Tabla `HDI` en HDFS usando Beeline

```sql
beeline
use usernamedb;
CREATE TABLE HDI (
    id INT, 
    country STRING, 
    hdi FLOAT, 
    lifeex INT, 
    mysch INT, 
    eysch INT, 
    gni INT
) ROW FORMAT DELIMITED 
FIELDS TERMINATED BY ',' 
STORED AS TEXTFILE;

```

## 2. Cargar Datos en la Tabla HDI
### Opción 1: Cargar datos directamente en HDFS.

```bash
hdfs dfs -put /user/hadoop/datasets/onu/hdi-data.csv /user/hive/warehouse/usernamedb.db/hdi
```

## Opción 2: Cargar datos desde Hive (otorgar permisos al directorio antes de cargar).

```bash
hdfs dfs -chmod -R 777 /user/hadoop/datasets/onu/
beeline
LOAD DATA INPATH '/user/hadoop/datasets/onu/hdi-data.csv' INTO TABLE HDI;
```


## 3. Crear una Tabla Externa HDI en HDFS o S3

```bash
CREATE EXTERNAL TABLE HDI (
    id INT, 
    country STRING, 
    hdi FLOAT, 
    lifeex INT, 
    mysch INT, 
    eysch INT, 
    gni INT
) ROW FORMAT DELIMITED 
FIELDS TERMINATED BY ',' 
STORED AS TEXTFILE 
LOCATION '/user/hadoop/datasets/onu/hdi/'; 

-- Para S3
LOCATION 's3://aadiazm-datasets/onu/hdi/';
```


## Consultas y Cálculos sobre la Tabla HDI
### 1. Mostrar Tablas y Describir HDI

```bash
use usernamedb;
show tables;
describe hdi;
```


## 2. Consultar Todos los Datos y Filtrar por GNI > 2000 

```bash
select * from hdi;
select country, gni from hdi where gni > 2000;
```

## 3. Realizar un JOIN entre Tablas HDI y EXPO
### 1. Crear la tabla EXPO en Hive:

```bash
CREATE EXTERNAL TABLE EXPO (
    country STRING, 
    expct FLOAT
) ROW FORMAT DELIMITED 
FIELDS TERMINATED BY ',' 
STORED AS TEXTFILE 
LOCATION 's3://aadiazm-datasets/onu/export/';
```

### 2. Ejecutar el JOIN:

```bash
SELECT h.country, gni, expct 
FROM HDI h 
JOIN EXPO e ON (h.country = e.country) 
WHERE gni > 2000;
```

## WordCount en Hive
### Crear la Tabla docs para WordCount
Opción HDFS:

```bash
CREATE EXTERNAL TABLE docs (line STRING) 
STORED AS TEXTFILE 
LOCATION 'hdfs://localhost/user/hadoop/datasets/gutenberg-small/';
```

### Opción S3:

```bash
CREATE EXTERNAL TABLE docs (line STRING) 
STORED AS TEXTFILE 
LOCATION 's3://aadiazm-datasets/gutenberg-small/';
```

## Consultas para WordCount
### 1. WordCount ordenado por palabra:

```bash
SELECT word, count(1) AS count 
FROM (SELECT explode(split(line,' ')) AS word FROM docs) w 
GROUP BY word 
ORDER BY word DESC 
LIMIT 10;
```

### 2. WordCount ordenado por frecuencia (de mayor a menor):

```bash
SELECT word, count(1) AS count 
FROM (SELECT explode(split(line,' ')) AS word FROM docs) w 
GROUP BY word 
ORDER BY count DESC 
LIMIT 10;
```

## Reto: Almacenar Resultados de WordCount en una Tabla
### 1. Crear la Tabla wordcount_results

```bash
CREATE TABLE wordcount_results (
    word STRING,
    count INT
) STORED AS TEXTFILE;
```

### 2. Insertar Resultados del WordCount en la Tabla

```bash
INSERT INTO TABLE wordcount_results
SELECT word, count(1) AS count
FROM (SELECT explode(split(line, ' ')) AS word FROM docs) w
GROUP BY word;
```

### 3. Consultar la Tabla wordcount_results

```bash
SELECT * FROM wordcount_results
ORDER BY count DESC
LIMIT 10;
```
