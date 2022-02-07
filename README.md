# Human Resources Turnover Analysis Tool

Tool that estimates if a employee is more likely to leave the company or not and what are the odds of this situation to happen.
#
* Used .xls, .csv and .json files to build the database (simulating a real world situation) 
* Docker to build enviroments for:
  *  Apache Airflow (orquestrate and schedule data pipelines); 
  *  MinIO (Local Datalake); 
  *  MySQL(Database server).
* Streamlit to build the app for our tool.

## Code and Resources Used 
**Python Version:** 3.8 (only because Pycaret doesn't work on Python 3.9/3.10 yet)
**Packages:** numpy, pandas, datetime, math, glob, pymysql, sklearn, matplotlib, seaborn, minio, sweetviz, pycaret, streamlit

# Guidelines

# Preparing the Enviroment
 1. Download **Anaconda** from the website: https://www.anaconda.com/products/individual#Downloads
 2. Download **Docker Desktop**: https://www.docker.com/get-started
 3. Download **Visual Studio Code**: https://code.visualstudio.com/download

## Create a folder on your machine to store scripts and other files (example: c:\hrproject).
** For Windows users, it's recommended to create the folder inside your 'Documents' folder, so your files will be 'visible' to Jupyter.

# Installing and configuring MySQL Server
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

 # Installing and configuring Apache Airflow
 1. Inside your main folder, create another subfolder called 'airflow'
 2. Copy the subfolder Dags (with all the 7 .py files) available here to your subfolder airflow. Your directory structure now should look like this:
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
       database_name      | employees


    ** For data_lake_server and database_server IPs, you should check on terminal if those IPs are the same for each container.
    Run the following commands on terminal:
     - docker container inspect mysqldb
     - docker container inspect minio

# Data Modeling
 1. Let's access again the MinIO web inteface at the link https://localhost:9000
 2. On "Buckets", create the following buckets:
   . processing
   . curated
   . landing (and then create two folders called 'performance-evaluation' and 'working-hours')
 	 on folder *performance-evaluation* (inside the bucket *Landing*):
   	  - upload the file employee_performance_evaluation.json available here in '/datalake/performance-evaluation'
	 on folder *working-hours* (inside the bucket *Landing*):
	  - upload the all the six .xlsx files available in '/datalake/landing/working-hours'
 3. Open Anaconda > Jupyter Notebook and load the notebook named 'modelagem_dados'. Run it so we can execute the data modeling process.

# Uploading the Database
 1. Open VSCode, go to the database tab on the left and left-click on '127.0.0.1@3307' > 'Import SQL'.
 2. Select the file 'employers_db.sql' available here.
 3. Wait until the process finishes.

# Running the Dags
 Back to the Airflow web interface, click on the DAGS tab on the top of the page. Should appear seven different dags which you should run one-by-one (so you don't 'overcharge' the airflow scheduler and get some errors).
 If everything works fine, you'll see on the middle of page, right after each Dag, a Dark Green Circle indicating that the Dag was started and worked well.

# Analysis and Machine Learning Deploy
 Now that we have almost everything settled up, we need to run the last two notebooks: 'analise' and 'machine_learn_deploy'.
 On the first one we run a series of analysis using the most important features, then uploading it to the Datalake and turning all the 'job' into a exploratory data analysis visualization.
 Second, we run 'machine_learning_deploy' notebook so we can try different models of machine learning and finding the most accurate/better performer for the job.

# Building an App for Model Visualization
 Finally we get to see the project running as an App... For the last part we need to go back to anaconda navigator and launch *CMD.exe Prompt*
 After that, run the following commands:
	. pip install streamlit (installing the library on the enviroment of the project)
	. streamlit run 'directory where app.py is saved (for example; 'c:/Users/****/Documents/Hrproject/app.py')
 Just wait a few seconds and... voilá! App is running on your browser! 
 ***Feel free to try different settings and find interesting 'intel' about your employees! ***
