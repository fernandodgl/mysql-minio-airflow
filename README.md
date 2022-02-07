# Human Resources Turnover Analysis Tool

Tool that estimates if a employee is more likely to leave the company or not and what are the odds of this situation to happen.
#
* Used .xls, .csv and .json files to build the database (simulating a real world situation) 
* Docker to build enviroments for:
  *  Apache Airflow (orquestrate and schedule data pipelines); 
  *  MinIO (Local Datalake); 
  *  MySQL(Database server).
* Streamlit to build an app for our tool.
<!--
## Code and Resources Used 
**Python Version:** 3.8 (only because Pycaret doesn't work on Python 3.9/3.10 yet)
**Packages:** numpy, pandas, datetime, math, glob, pymysql, sklearn, matplotlib, seaborn, minio, sweetviz, pycaret, streamlit

## Productionization 
In this step, I built a flask API endpoint that was hosted on a local webserver by following along with the TDS tutorial in the reference section above. The API endpoint takes in a request with a list of values from a job listing and returns an estimated salary. 
-->

# Preparing the Enviroment
 1. Download **Anaconda** from the website: https://www.anaconda.com/products/individual#Downloads
 2. Download **Docker Desktop**: https://www.docker.com/get-started
 3. Download **Visual Studio Code**: https://code.visualstudio.com/download

## Create a folder on your machine to store scripts and other files (example: c:\hrproject).
** For Windows users, it's recommended to create the folder inside your 'Documents' folder, so your files will be 'visible' to Jupyter.

## Installing and configuring MySQL Server
 1. Start terminal (ex. Powershell)
 2. Create a container for MySQL by enabling port 3307:
     'docker run --name mysqldb -e MYSQL_ROOT_PASSWORD=bootcamp -p "3307:3306" -d mysql'
 3. Test your access to the database. 
   . Open VSCode and look for an extension called 'Database Client' (could be any client which support mysql).
   . Input the following settings:
     - Host: 127.0.0.1
     - Username: root
     - Port: 3307
     - Password: 470917
   . You should get a "Success!" message if you followed the steps correctly.
 
# Datalake setup using MinIO Server
 1. Inside the folder created by you for this project, create a subfolder called 'datalake'.
 2. Open the terminal again and navigate to the main folder created.
 3. Run the command (so we can create the MinIO container):
     'docker run --name minio -d -p 9000:9000 -p 9001:9001 -v /datalake:/data minio/minio server /data --console-address :9001'
 4. Test the access to MinIO:
    . Open your browser;
    . Type http://localhost:9001/login
    . Username: minioadmin
    . Password: minioadmin

 ## Installing and configuring Apache Airflow
 1. Inside your main folder, create another subfolder called 'airflow'
 2. Copy the subfolder Dags (with all the 7 files) available here to your subfolder airflow. Your directory structure now should look like this:
    'your_folder' -
                   | - datalake
                   | - aiflow
                             | - dags
 3. Inside the main folder, open again your terminal and type: 
    3a. (for Airflow container building):
     'docker run -d -p 8080:8080 -v "$PWD/airflow/dags:/opt/airflow/dags/" --entrypoint=/bin/bash --name airflow        apache/airflow:2.1.1-python3.8 -c '(airflow db init && airflow users create --username admin --password password --firstname Firstname --lastname Lastname --role Admin --email admin@example.org); airflow webserver & airflow scheduler'  

    3b. Install the necessary libraries for the enviroment:
     'docker container exec -it airflow bash' (so you can enter in bash mode of the container)
     'pip install pymysql xlrd openpyxl minio'
     ** Not necessary to upgrade PIP for this project

 4. After a few minutes, access Apache Airflow web interfacing by the link https://localhost:8080
        . Login: admin
        . Password: password
 5. Now we create the variables to be used to access both database and datalake to run our dags.
    Go to Admin >> Variables.
     Create the following variables:

       KEY                | VALUE
       -------------------|----------------
       data_lake_server   | 172.17.0.4:9000 **check IP on terminal
       data_lake_login    | minioadmin
       data_lake_password | minioadmin
       database_server    | 172.17.0.3      **check IP on terminal 
       database_login     | root
       database_password  | bootcamp
       database_name      |  employees


    ** For data_lake_server and database_server IPs, you should check on terminal if those IPs are the same for each container.
    Run the following commands on terminal:
     - docker container inspect mysqldb
     - docker container inspect minio

# Data Modeling

     
       
   
       


