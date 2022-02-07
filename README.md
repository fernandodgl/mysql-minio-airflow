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
   'docker run --name mysqlbd1 -e MYSQL_ROOT_PASSWORD=bootcamp -p "3307:3306" -d mysql'
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
3. Run the command:
   '  


