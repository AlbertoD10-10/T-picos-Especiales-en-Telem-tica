Guía de Instalación de AWS EMR 7.3.0
Parte 1: Crear un Clúster AWS EMR
Buscar AWS EMR

Ingresa a la consola web de AWS y busca el servicio EMR.
Crear Clúster

Configura el nombre y la versión del clúster.
Selecciona los paquetes necesarios (Hadoop/Spark) y activa los catálogos Glue y Hive para gestionar tablas.
Configurar Máquinas EC2

Selecciona la instancia m5.xlarge. Si hay problemas de recursos, considera m4.xlarge.
Configuración de Software

Crea un bucket en S3 para guardar notebooks de JupyterHub.
Configuración:
[
    {
        "Classification": "jupyter-s3-conf",
        "Properties": {
            "s3.persistence.enabled": "true",
            "s3.persistence.bucket": "aadiazmnotebooks"
        }
    }
]
Seguridad y Roles IAM

Configura los grupos de seguridad necesarios y asigna los roles IAM:
Service role: EMR_DefaultRole
Instance profile: EMR_EC2_DefaultRole
Custom automatic scaling role: EMR_AutoScaling_DefaultRole
Crear el Clúster

La creación del clúster puede tardar unos 20 minutos.
Abrir Puertos TCP

Accede al Security Group de la instancia Master y abre los puertos necesarios: 22, 14000, 9870.
Parte 2: Borrar y Recrear el Clúster
Los clústeres EMR son temporales y deben eliminarse cuando no se utilicen.
Para reusarlos, clónalos y configura el usuario hadoop nuevamente.
Cambia el archivo hue.ini para modificar el puerto 14000 a 9870.
Parte 3: Ingresar al Clúster EMR con Hue
Abre la aplicación Hue en el puerto 8888 usando la IP o nombre del nodo Master.
Configura un usuario:
Username: hadoop
Password: elige una contraseña.
Parte 4: Acceder a JupyterHub
Abre JupyterHub en el puerto 9443:
Username: jovyan
Password: jupyter
Verifica que las variables de contexto de Spark están activas en un notebook PySpark.
Referencias: Guía oficial de AWS EMR JupyterHub
