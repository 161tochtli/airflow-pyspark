## This project includes:
 
- Docker configuration files for building and orchestrate Apache Airflow and Pyspark Containers
- Configuration files for the project
- A folder called "ejericio1" holding files for an ETL job making use of Airflow and Pyspark 

## ETL Structure

The ETL project structure is as follows: 

- `raw`: The original, immutable data dump
- `interim`: Intermediate data that has been transformed
- `processed`: The final, data sets to be uploaded to the datalake
- `helpers` : Helpers for the development workflow 
- `libs`: Objects to be used within the ETL process
- `tasks`: The ETL tasks
- `my_etl.py`: An airflow DAG to orchestrate the ETL process


# Set up
In this section we describe the steps towards building the containes as well as configuring spark
connection with airflow.

## Prerequisites

Before setting up the project, ensure you have Docker and Docker Compose installed on your system.


### Docker Setup

To run this project using Docker, follow these steps:

1. Clone this repository to your local machine.
2. Navigate to the folder of the repo.
3. Before step 4, you should grant 
4. Build and run the containers using Docker Compose:

```bash
docker-compose up -d --build
```

The container holding airflow webserver will fail in the first run, so do the following:
1. Find the container holding airflow webserver
```bash
docker ps -a | grep airflow-pyspark-webserver
```
2. Copy the first number (the container id), replace the placeholder (container_id) below and run the command:
```bash
docker restart container_id
```

## Airflow Setup
For this step, we will connect Airflow and Spark.

Go to "Admin" tab
![Screenshot](readme_images/1_admin.png)

Click on "Connections" options
![Screenshot](readme_images/2_connections.png)

Click on the add symbol
![Screenshot](readme_images/3_add_connection.png)

Fill connection with the required info, use the info from the image.
![Screenshot](readme_images/4_fill_connection.png)

For Port, you should find wich port is airflow webserver listening. For this, execute

```bash
docker ps | grep airflow-pyspark-spark-master
```
Encuentra el número de puerto, situado a la derecha de la flecha. Ejemplo:
![Screenshot](readme_images/5_find_port.png)

Escribe este último número en la casilla de "Puerto", y finalmente, guarda la conección haciendo click sobre "Save"
![Screenshot](readme_images/6_write_port_and_save_connection.png)

Now, you can trigger a DAG
![Screenshot](readme_images/7_trigger_dag.png)


## Directory Structure for Jobs
Ensure your Spark job files are placed in the following directories and are accessible to the Airflow container:

* Python job: jobs/python/

These paths should be relative to the mounted Docker volume for Airflow DAGs.

## Usage
After the Docker environment is set up, the `my_etl` DAG will be available in the Airflow web UI [localhost:8080](localhost:8080), where it can be triggered.

### The DAG will execute the following steps:
* Print "Jobs started" in the Airflow logs.
* Submit the Python Spark job to the Spark cluster.
* Print "Jobs completed successfully" in the Airflow logs after all jobs have finished.


Special BIG THANKS to https://github.com/airscholar 

Ideas from:
https://github.com/airscholar/SparkingFlow
https://github.com/drivendata/cookiecutter-data-science