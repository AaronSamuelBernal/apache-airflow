# Apache airflow

**¿Qué es apache airflow?**

Airflow se usa para automatizar trabajos programáticamente dividiéndolos en subtareas. 
Permite su planificación y monitorización desde una herramienta centralizada. Los casos de uso más comunes son la automatización de ingestas de datos, acciones de mantenimiento periódicas y tareas de administración. Para ello, permite planificar trabajos como un cron y también ejecutarlos bajo demanda.

Para las tareas se utilizan DAGs (Grafo acíclico dirigido)
son colecciones de tareas o de trabajos a ejecutar conectados mediante relaciones y dependencias. Son la representación de los flujos de trabajo.

**Los grafos deben cumplir dos condiciones: ser dirigidos y acíclicos:**

**Dirigidos:** Las relaciones entre los nodos tienen solo un sentido.

**Acíclicos:** No pueden formar ciclos, es decir, la ejecución no puede volver a un nodo que ya ha ejecutado.


**Casos de uso**
ayuda a gestionar y a monitorizar este tipo de procesos. Además, Airflow integra una interfaz de usuario sencilla, una herramienta CLI que proporciona control del estado de ejecución de todo el sistema. También se encuentra en desarrollo su propia API.

Además, podemos usar Airflow para orquestar testing automático de componentes, backups y generación de métricas y reportes.

Por ejemplo, un workflow o pipeline sencillo podría contener las siguientes tareas:

1.Descargar datos de una base de datos MySQL

2.Enviar los datos a un clúster de Apache Kafka

3.Realizar transformaciones sobre los datos con Apache Spark

4.Generar un mensaje de terminación


**Los componentes de Airflow**

Apache Airflow tiene varios componentes. Estos son los principales 

- Servidor Web – Servidor Flask con Gunicorn
- Scheduler – Demonio encargado de los flujos de trabajo
- Metastore – Base de datos donde se guardan los metadatos
- Executor – Define cómo se ejecutan las tareas
- Worker – Proceso que ejecuta tu tarea


**Ventajas de Airflow**


1. Dinámico
Como se programa en Python, todo lo que puedes hacer en Python lo puedes hacer en Airflow en tus flujos de trabajo o data pipelines.
2. Escalable
Puedes ejecutar muchas tareas a la vez.
3. Interfaz de usuario
Tiene una muy buena interfaz de usuario para interactuar con Airflow de forma dinámica.
4. Extensibilidad
Puedes crear tu propio plugin para que Airflow pueda interactuar con cualquier herramienta.


**Arquitectura de Apache Airflow: Ejecutores**

Airflow se compone de un servidor web que sirve la API y la interfaz de usuario y de un planificador (Scheduler) encargado de interpretar y ejecutar las tareas definidas en los DAGs.

Existen varias formas de desplegar Apache Airflow, con múltiples arquitecturas para sus ejecutores: Local, Sequential, Celery, Dask, Mesos o Kubernetes. También se puede usar con servicios en la nube de Azure, AWS o Google Cloud. A continuación se listan los más usados.

**Single-Node Executors**

Los ejecutores de este tipo solo permiten la ejecución de tareas en un nodo o worker, que es el mismo host en el que se encuentra el Scheduler. No debemos considerar estos ejecutores para sistemas en producción ya que no son escalables y son puntos únicos de fallo.

Sequential: Este tipo de ejecutor secuencial se usa para debugging de DAGs
Local: Ejecuta las tareas en paralelo. Es el entorno mínimo que se podría considerar para una aplicación real. 

**Cluster Executors**

Para gestionar los recursos, Apache Airflow necesita una asignación fija de workers sobre los que distribuir las carga de trabajo.

Celery: Ejecuta tareas en paralelo en varios nodos separados, por lo que permite escalar el sistema de forma horizontal y vertical

Kubernetes: Ejecuta cada tarea en un pod de Kubernetes, desplegando nuevos pods según la demanda de recursos.

**Despliegue con docker**

1. Prerequisitos

- - Instalar python3
`sudo apt-get install python3-pip`

- Instalar celery
`pip install 'apache-airflow[celery]'`

- Docker-compose (versión 1.29 como mínimo) **Si ya tiene docker-compose, compruebe utilizando docker-compose --version, si la versión es menor a la requerida, deberá desinstalar anter de realizar los siguientes pasos**

`curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`

`chmod +x /usr/local/bin/docker-compose`

**Instalación**

- Clonar el repositorio

- Ingresar al directorio docker y ejecutar lo siguiente

`echo -e "AIRFLOW_UID=$(id -u)" > .env`

- Modificar el .env colocando el host al que será publicado y el email, por ejemplo
`LET_ENC_HOST=airflow.domain.com`
`LET_ENC_EMAIL=name@super-email.com`

`docker-compose up airflow-init`

- Debería obtener la siguiente salida:

`airflow-init_1        Upgrades done
airflow-init_1        Admin user airflow created
airflow-init_1       2.2.2
start_airflow-init_1 exited with code 0`

- Luego inicie el compose

`sudo docker-compose up -d`

**Despliegue en kubernetes**

Apache Airflow aims to be a very Kubernetes-friendly project, and many users run Airflow from within a Kubernetes cluster in order to take advantage of the increased stability and autoscaling options that Kubernetes provides.

Airflow community maintain official Helm chart for Airflow that helps you define, install, and upgrade deployment. The Helm Chart uses official Docker image and Dockerfile that is also maintained and released by the community.

**To install this chart using helm 3, run the following commands:**

`kubectl create namespace airflow`

`helm repo add apache-airflow https://airflow.apache.org`

`helm install airflow apache-airflow/airflow --namespace airflow`
