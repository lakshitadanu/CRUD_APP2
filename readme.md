# Deploying a Flask Application with PostgreSQL on an EC2 Server.

 This guide outlines the steps to deploy a Flask-based application on an AWS EC2 instance. 

## 1. Launch an EC2 Instance

   a. Log in to your AWS Management Console.

   b. Navigate to EC2 Dashboard → Instances → Launch Instance.

   c. Choose an Amazon Machine Image (AMI) (e.g., Ubuntu 22.04 LTS).

   d. Select an instance type (e.g., t2.micro for free tier eligibility).

   e. Configure security group:

      - Allow SSH (22) from your IP.

      - Allow HTTP (80) for web traffic.

      - Allow port 5000 for Flask (optional, for testing before using a reverse proxy).

   f. Launch the instance and note down the Public IP or Public DNS.

## 2. Connect to the EC2 Instance 

   **Open a terminal and SSH into your EC2 instance:**

      ssh -i your-key.pem ubuntu@your-ec2-public-ip

   **Or use another way by giving the absolute path ex-**

      ssh -i ~/Downloads/aws-ssh/neha1.pem ubuntu@54.227.229.246


## 3. Clone Your Flask Application      
 
   **Clone your project from GitHub**

     git clone https://github.com/sanjanagupta10/CRUD_App.git

## 4. Set Up a Virtual Environment and Install Dependencies

    sudo apt update
    sudo apt install python3-venv -y
    python3 -m venv myenv
    source myenv/bin/activate
   
   **Command for installing neccessary packages-**

    pip install Flask Flask-CORS  mysql-connector-python psycopg2-binary

## 5. Install and Configure PostgreSQL    
   
    sudo apt install postgresql postgresql-contrib -y
    sudo systemctl start postgresql
    sudo systemctl status postgresql

   **Switch to the postgres user:** 

    sudo -i -u postgres

   **Open the PostgreSQL shell:** 

    psql

   **Change username and password:**

    ALTER USER postgres WITH PASSWORD 'Welcome@123';

   **Create a new database:**

    CREATE DATABASE demo_flask;

   **Connect to database** 

    \c demo_flask;

   **Create new table** 
    
    CREATE TABLE book ( id SERIAL PRIMARY KEY, publisher VARCHAR(255), name VARCHAR(255), date DATE );

   **Exit PostgreSQL:**

    \q

   **Return to the Ubuntu shell:** 

    exit

## 6. Modify Database Configuration in app.py

   **Navigate to the project directory**

     cd CRUD_App/CRUD_app_Flask/Server

   **Open the app.py file using Nano editor**

     nano app.py

   **Locate the database configuration section in app.py and modify it as follows**  

     db_config = {
    'host': 'localhost',
    'user': 'your_username',  # Replace with your PostgreSQL username i.e postgres
    'password': 'your_password',  # Replace with your PostgreSQL password i.e Welcome@123
    'dbname': 'demo_flask'  # Ensure this matches your PostgreSQL database name
}
   
   **Save and exit**

     -Press CTRL + X
     -Press Y to confirm changes
     -Press Enter to save the file

   **View the file to confirm the changes (OPTIONAL)**

     less app.py


## 7. Run Flask Application 

   **Make sure you are inside the Server within virtual environment**

   **a. If running locally:**
   Purpose: Runs the Flask application directly as a Python script.


    python3 app.py


   **b. If deploying on a server:**
   
    export FLASK_APP=app.py

   Purpose: Sets an environment variable so Flask knows which file to use when running the server. 

    flask run --host=0.0.0.0 --port=5000

   Required when running Flask on an EC2 instance or remote server to allow access from other devices.


   **Access the app via:** 
   
    http://your-ec2-public-ip:5000




    

