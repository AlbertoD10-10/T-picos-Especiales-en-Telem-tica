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
